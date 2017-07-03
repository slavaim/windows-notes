

```FltCreateFile``` calls IoCreateFileEx with ```IO_DRIVER_CREATE_CONTEXT.DeviceObjectHint``` pointing to the Filter Manager's filter object and then calls the lower registered filters. That allows the created file object to have ```TopDeviceObjectHint``` pointing to the Filetr Manager's object. Below is a debug session for Windows 10 1703.

```
 # Child-SP          RetAddr           Call Site
00 ffffe101`3fffebf8 fffff801`92125200 FLTMGR!FltpCreate
01 ffffe101`3fffec00 fffff801`9213058b nt!IopParseDevice+0x7f0
02 ffffe101`3fffedd0 fffff801`921340c0 nt!ObpLookupObjectName+0x46b
03 ffffe101`3fffefa0 fffff801`9213803a nt!ObOpenObjectByNameEx+0x1e0
04 ffffe101`3ffff0e0 fffff801`920b0eb4 nt!IopCreateFile+0x3aa
05 ffffe101`3ffff180 fffff808`485240d5 nt!IoCreateFileEx+0x124
06 ffffe101`3ffff210 fffff808`4853d32d FLTMGR!FltpCreateFile+0x1cd
07 ffffe101`3ffff310 fffff808`4b6a79f8 FLTMGR!FltCreateFile+0x8d
08 ffffe101`3ffff3a0 fffff808`484f4b4c avscan!AvPreCreate+0x378 [d:\work\avscan\filter\avscan.c @ 2106]
09 ffffe101`3ffff4b0 fffff808`484f46ec FLTMGR!FltpPerformPreCallbacks+0x2ec
0a ffffe101`3ffff5d0 fffff808`48526117 FLTMGR!FltpPassThroughInternal+0x8c
0b ffffe101`3ffff600 fffff801`92125200 FLTMGR!FltpCreate+0x2d7
0c ffffe101`3ffff6b0 fffff801`9213058b nt!IopParseDevice+0x7f0
0d ffffe101`3ffff880 fffff801`921340c0 nt!ObpLookupObjectName+0x46b
0e ffffe101`3ffffa50 fffff801`920c9e90 nt!ObOpenObjectByNameEx+0x1e0

0: kd> dt nt!_FILE_OBJECT ffff948c621a3330
   +0x000 Type             : 0n5
   +0x002 Size             : 0n216
   +0x008 DeviceObject     : 0xffff948c`60a3bc80 _DEVICE_OBJECT
   +0x010 Vpb              : 0xffff948c`60a556e0 _VPB
   +0x018 FsContext        : 0xffff948c`6111a740 Void
   +0x020 FsContext2       : 0xffff8483`76cf78f0 Void
   +0x028 SectionObjectPointer : (null) 
   +0x030 PrivateCacheMap  : (null) 
   +0x038 FinalStatus      : 0n0
   +0x040 RelatedFileObject : (null) 
   +0x048 LockOperation    : 0 ''
   +0x049 DeletePending    : 0 ''
   +0x04a ReadAccess       : 0x1 ''
   +0x04b WriteAccess      : 0 ''
   +0x04c DeleteAccess     : 0 ''
   +0x04d SharedRead       : 0x1 ''
   +0x04e SharedWrite      : 0x1 ''
   +0x04f SharedDelete     : 0x1 ''
   +0x050 Flags            : 0x40000
   +0x058 FileName         : _UNICODE_STRING "\"
   +0x068 CurrentByteOffset : _LARGE_INTEGER 0x0
   +0x070 Waiters          : 0
   +0x074 Busy             : 0
   +0x078 LastLock         : (null) 
   +0x080 Lock             : _KEVENT
   +0x098 Event            : _KEVENT
   +0x0b0 CompletionContext : (null) 
   +0x0b8 IrpListLock      : 0
   +0x0c0 IrpList          : _LIST_ENTRY [ 0xffff948c`621a33f0 - 0xffff948c`621a33f0 ]
   +0x0d0 FileObjectExtension : 0xffff948c`6226f1b0 Void
   
0: kd> dq 0xffff948c`6226f1b0
ffff948c`6226f1b0  00000000`00000000 00000000`00000000
ffff948c`6226f1c0  ffff948c`60dff0d0 00000000`00000000
ffff948c`6226f1d0  ffff948c`6243f2c0 00000000`00000000
ffff948c`6226f1e0  00000000`00000000 00000000`00000000
ffff948c`6226f1f0  00000000`00000000 00000000`00000000
ffff948c`6226f200  61436d4d`02120006 00000000`0000034c
ffff948c`6226f210  ffff8483`7535ed10 ffff948c`62161e28
ffff948c`6226f220  ffff948c`6221da78 00000000`00000000

0: kd> dq ffff948c`60dff0d0
ffff948c`60dff0d0  ffff948c`610734a0 00000000`00000000
ffff948c`60dff0e0  00000000`00000000 00000000`00000000
ffff948c`60dff0f0  65536d4d`02060003 6c8da38a`069a7123
ffff948c`60dff100  00000000`00000000 0000024e`49c8000a
ffff948c`60dff110  0000024e`49c80fff 00000000`00000000
ffff948c`60dff120  00000000`00000000 00000000`00000000
ffff948c`60dff130  00000000`00000000 00000000`00000000
ffff948c`60dff140  00000000`00000002 00000000`00000000

0: kd> !object ffff948c`610734a0
Object: ffff948c610734a0  Type: (ffff948c5e34eb00) Device
    ObjectHeader: ffff948c61073470 (new version)
    HandleCount: 0  PointerCount: 1
    
0: kd> !devstack ffff948c610734a0
  !DevObj           !DrvObj            !DevExt           ObjectName
> ffff948c610734a0  \FileSystem\FltMgr ffff948c610735f0  
  ffff948c61048060  \FileSystem\fastfatffff948c610481b0 
  
0: kd> !vpb 0xffff948c`60a556e0
Vpb at 0xffff948c60a556e0
Flags: 0x1 mounted 
DeviceObject: 0xffff948c61048060
RealDevice:   0xffff948c60a3bc80
RefCount: 8
Volume Label: 
```
