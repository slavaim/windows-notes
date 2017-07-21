

```
2: kd> !idt

Dumping IDT: ffff9781a924e000
...
1f:	fffff80032bee810 nt!KiApcInterrupt
...
```

```KiApcInterrupt``` is used to "inject" APC processing in the running thread by invoking interrupt through LAPIC, this is done from 

```
VOID
FASTCALL
KiInsertQueueApc (
    IN PKAPC Apc,
    IN KPRIORITY Increment
    )
    {
    [...]
    
    Thread = Apc->Thread;
    
    [...]
            ThreadState = Thread->State;
            if (ThreadState == Running) {
                RequestInterrupt = TRUE;
            }
     [...]
        if (RequestInterrupt == TRUE) {
            KiRequestApcInterrupt(Thread->NextProcessor);
        }
     }
```

Prior to calling ```KiApcInterrupt``` a CPU and the kernel creates a frame on the stack that contains register state for interrupted thread.
As you can se on the eample below there is no call to KiApcInterrupt from the interrupted function ( ironicaly KiDeliverApc here but can be any ) so this is an interrupt.
```
1: kd> kn
 # Child-SP          RetAddr           Call Site
00 ffff9781`aa9fd588 fffff800`32ab44e9 nt!KiApcInterrupt
01 ffff9781`aa9fd5b0 fffff800`32ab2c67 nt!KiDeliverApc+0x229
02 ffff9781`aa9fd640 fffff800`32ab2461 nt!KiSwapThread+0x337
03 ffff9781`aa9fd6f0 fffff800`32ab06b7 nt!KiCommitThreadWait+0x101

fffff800`32ab44e3 33c0            xor     eax,eax
fffff800`32ab44e5 440f22c0        mov     cr8,rax
fffff800`32ab44e9 4c8b45e8        mov     r8,qword ptr [rbp-18h] <-- return address
fffff800`32ab44ed 488b55f0        mov     rdx,qword ptr [rbp-10h]
fffff800`32ab44f1 488b4df8        mov     rcx,qword ptr [rbp-8]
fffff800`32ab44f5 488b45e0        mov     rax,qword ptr [rbp-20h]
```

Some notes on APC damaging the stack if a caller failes to wait for IRP completion in case of STATUS_PENDING returned by ```IoCallDriver```. Completion APC can change a registers frame pushed on a thread stack during interrupt processing, e.g. to deliver APC by invoking KiApcInterrupt through LAPC. The other scenario that was met in practice when the RDI register had its lower part zeroed is outlined below ( details here http://www.osronline.com/cf.cfm?PageURL=showThread.CFM?link=285110 )

```
KiDeliverApc()
{

    //
    // RDI == &Thread->ApcState.ApcListHead[KernelMode]
    //
    // IsListEmpty is translated to cmp and je machine instructions
    //
    //  cmp     qword ptr [rdi],rdi
    //  je        nt!KiDeliverApc+Offset_Outside_While
    //
    while( ! IsListEmpty( &Thread->ApcState.ApcListHead[KernelMode] ) 
    {
         //
         // RDI is an invariant in the cycle and points to  
&Thread->ApcState.ApcListHead[KernelMode]
         //
         ApcEntry = RemoveHeadList( &Thread->ApcState.ApcListHead[KernelMode] );
         Apc = CONTAINING_RECORD(ApcEntry, KAPC, ApcListEntry);
         CompletionRoutine = Apc->KernelRoutine;
         ....
         call CompletionRoutine
         {
             //
             // inside CompletionRoutine
             //
             ....
             push ...
             push RDI   // after push RSP == Irp->UserIosb
             push ...
             push ...
             ....
             Irp->UserIosb.Status = STATUS_SUCCESS // this changes the saved RDI
             ....
             pop ...
             pop ...
             pop RDI // the changed RDI is restored from the stack
             pop ...
             ret
         }
         //
         // go to while with RDI having its lower half being zeroed
         //
    }

}
```
