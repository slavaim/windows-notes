
The kernel is mapped with large pages ( 2 MB page on 64 bit, 4 MB page on 32 bit )

```
1: kd> u nt!ExAllocatePoolWithTag
nt!ExAllocatePoolWithTag:
fffff803`791153a0 4055            push    rbp
fffff803`791153a2 53              push    rbx
fffff803`791153a3 56              push    rsi
fffff803`791153a4 57              push    rdi
fffff803`791153a5 4156            push    r14
fffff803`791153a7 4157            push    r15
fffff803`791153a9 488d6c24d1      lea     rbp,[rsp-2Fh]
fffff803`791153ae 4881ecb8000000  sub     rsp,0B8h

1: kd> !pte fffff803`791153a0
                                           VA fffff803791153a0
PXE at FFFF80C060301F80    PPE at FFFF80C0603F0068    PDE at FFFF80C07E00DE40    PTE at FFFF80FC01BC88A8
contains 0000000000B09063  contains 0000000000B0A063  contains 000000013A0009E3  contains 0000000000000000
pfn b09       ---DA--KWEV  pfn b0a       ---DA--KWEV  pfn 13a000    -GLDA--KWEV  LARGE PAGE pfn 13a115 
```
