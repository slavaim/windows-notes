
An example of deep recursion with DmkCommonWrite in a call from FltWriteFile in post-create callback

```
1: kd> kn
 # Child-SP          RetAddr           Call Site
00 ffffa801`64f53458 fffff80a`73244ebe DeviceLockDMK0!DmkCommonWrite [d:\work\dl2\8.2\windows\drivers\private\devicelockdmk\dmkwrite.c @ 262]
01 ffffa801`64f53460 fffff80a`7322c536 DeviceLockDMK0!DmkWrite+0x14e [d:\work\dl2\8.2\windows\drivers\private\devicelockdmk\dmkwrite.c @ 64]
02 ffffa801`64f534b0 fffff80a`7336768a DeviceLockDMK0!DmkCommonDispatchHook+0x776 [d:\work\dl2\8.2\windows\drivers\private\devicelockdmk\dmkhooks.c @ 774]
03 ffffa801`64f53570 fffff80a`73367fbe DeviceLockDriver0!DldFsdPreoperationCallback+0x7d5a [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 7573]
04 ffffa801`64f53c50 fffff80a`7336876a DeviceLockDriver0!GeneralFsdHookedDispatchEx+0x30e [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 7922]
05 ffffa801`64f53e90 fffff80a`73369333 DeviceLockDriver0!GeneralFsdHookedDispatch+0x5a [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 8205]
06 ffffa801`64f53f00 fffff80a`7331b3ff DeviceLockDriver0!FltmgrHookedDispatch+0x23 [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 8913]
07 ffffa801`64f53f30 fffff803`790052a7 DeviceLockDriver0!DldDispatchWraperCallback+0xcf [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1303]
08 ffffa801`64f53f80 fffff803`7900526d nt!KxSwitchKernelStackCallout+0x27
09 ffffa801`64b2b150 fffff803`78eac4ac nt!KiSwitchKernelStackContinue
0a ffffa801`64b2b170 fffff803`78eac2ee nt!KiExpandKernelStackAndCalloutOnStackSegment+0x12c
0b ffffa801`64b2b1f0 fffff803`78eac1af nt!KiExpandKernelStackAndCalloutSwitchStack+0x9e
0c ffffa801`64b2b260 fffff80a`7331b5df nt!KeExpandKernelStackAndCalloutInternal+0x2f
0d ffffa801`64b2b2b0 fffff80a`7331bbc3 DeviceLockDriver0!DldDispatchWraper+0x1bf [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1353]
0e ffffa801`64b2b350 fffff80a`7331bfe4 DeviceLockDriver0!DlHookerFunctionChecker+0x123 [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1484]
0f ffffa801`64b2b3d0 fffff803`78ebbf2f DeviceLockDriver0!FltmgrHookedDispatchDBG+0x24 [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1535]
10 ffffa801`64b2b400 fffff803`78f9b9ee nt!IoSynchronousPageWriteEx+0x13b
11 ffffa801`64b2b440 fffff803`78f33f95 nt!MiIssueSynchronousFlush+0x72
12 ffffa801`64b2b4c0 fffff803`78ee82ac nt!MiFlushSectionInternal+0x8a5
13 ffffa801`64b2b700 fffff803`78ee7dc5 nt!MmFlushSection+0xc8
14 ffffa801`64b2b7b0 fffff803`78ed766e nt!CcFlushCachePriv+0x4a5
15 ffffa801`64b2b8f0 fffff803`78ed6541 nt!CcMapAndCopyInToCache+0x7ce
16 ffffa801`64b2ba50 fffff803`79070d69 nt!CcCopyWriteEx+0x111
17 ffffa801`64b2bad0 fffff80a`732472c7 nt!CcCopyWrite+0x19
18 ffffa801`64b2bb10 fffff80a`73233fa7 DeviceLockDMK0!DmkCommonWrite+0x23b7 [d:\work\dl2\8.2\windows\drivers\private\devicelockdmk\dmkwrite.c @ 929]
19 ffffa801`64b2c030 fffff80a`732352e5 DeviceLockDMK0!DmkProcessIrpContext+0x1a7 [d:\work\dl2\8.2\windows\drivers\private\devicelockdmk\dmkinterface.c @ 314]
1a ffffa801`64b2c0a0 fffff80a`734b423c DeviceLockDMK0!DmkMinifilterPreOperationCallback+0xf5 [d:\work\dl2\8.2\windows\drivers\private\devicelockdmk\dmkminifilter.c @ 251]
1b ffffa801`64b2c100 fffff80a`734b4f73 DLDriverMiniFlt0!MftPreOperationCallDMK+0x6c [d:\work\dl2\8.2\windows\drivers\private\dldriverminiflt\minifltdmk.c @ 290]
1c ffffa801`64b2c150 fffff80a`6fd84b4c DLDriverMiniFlt0!PtPreOperationPassThrough+0x73 [d:\work\dl2\8.2\windows\drivers\private\dldriverminiflt\minifltmain.c @ 1289]
1d ffffa801`64b2c190 fffff80a`6fd832d3 FLTMGR!FltpPerformPreCallbacks+0x2ec
1e ffffa801`64b2c2b0 fffff80a`6fd96675 FLTMGR!FltpPassThroughFastIo+0x73
1f ffffa801`64b2c310 fffff80a`6fd96331 FLTMGR!FltWriteFileEx+0x335
20 ffffa801`64b2c400 fffff80a`731e828f FLTMGR!FltWriteFile+0x51
21 ffffa801`64b2c470 fffff80a`6fd8413c avscan!AvPostCreate+0x7bf [d:\work\avscan\filter\avscan.c @ 2753]
22 ffffa801`64b2c560 fffff80a`6fd83af3 FLTMGR!FltpPerformPostCallbacks+0x2ac
23 ffffa801`64b2c630 fffff80a`6fd85a35 FLTMGR!FltpPassThroughCompletionWorker+0x73
24 ffffa801`64b2c670 fffff80a`6fdb612b FLTMGR!FltpLegacyProcessingAfterPreCallbacksCompleted+0x585
25 ffffa801`64b2c6e0 fffff80a`73235f0f FLTMGR!FltpCreate+0x2eb
26 ffffa801`64b2c790 fffff80a`73233eae DeviceLockDMK0!DmkPassthroughIrpContextToMinifilter+0x32f [d:\work\dl2\8.2\windows\drivers\private\devicelockdmk\dmkminifilter.c @ 539]
27 ffffa801`64b2c830 fffff80a`7323430a DeviceLockDMK0!DmkProcessIrpContext+0xae [d:\work\dl2\8.2\windows\drivers\private\devicelockdmk\dmkinterface.c @ 291]
28 ffffa801`64b2c8a0 fffff80a`73305a80 DeviceLockDMK0!DmkProcessClientIrpSI+0x12a [d:\work\dl2\8.2\windows\drivers\private\devicelockdmk\dmkinterface.c @ 388]
29 ffffa801`64b2c8f0 fffff80a`7330ba12 DeviceLockDriver0!DldCreateDmkFileEntry+0x12d0 [d:\work\dl2\8.2\windows\drivers\private\dldriver\dmkclient.c @ 2772]
2a ffffa801`64b2cb00 fffff80a`73360abc DeviceLockDriver0!DldProcessCreateRequestByDmk+0x2292 [d:\work\dl2\8.2\windows\drivers\private\dldriver\dmkclient.c @ 5246]
2b ffffa801`64b2cf30 fffff80a`73367fbe DeviceLockDriver0!DldFsdPreoperationCallback+0x118c [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 5158]
2c ffffa801`64b2d610 fffff80a`7336876a DeviceLockDriver0!GeneralFsdHookedDispatchEx+0x30e [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 7922]
2d ffffa801`64b2d850 fffff80a`73369333 DeviceLockDriver0!GeneralFsdHookedDispatch+0x5a [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 8205]
2e ffffa801`64b2d8c0 fffff80a`7331b3ff DeviceLockDriver0!FltmgrHookedDispatch+0x23 [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 8913]
2f ffffa801`64b2d8f0 fffff80a`7331b550 DeviceLockDriver0!DldDispatchWraperCallback+0xcf [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1303]
30 ffffa801`64b2d940 fffff80a`7331bbc3 DeviceLockDriver0!DldDispatchWraper+0x130 [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1332]
31 ffffa801`64b2d9e0 fffff80a`7331bfe4 DeviceLockDriver0!DlHookerFunctionChecker+0x123 [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1484]
32 ffffa801`64b2da60 fffff803`7932d200 DeviceLockDriver0!FltmgrHookedDispatchDBG+0x24 [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1535]
33 ffffa801`64b2da90 fffff803`7933858b nt!IopParseDevice+0x7f0
34 ffffa801`64b2dc60 fffff803`7933c0c0 nt!ObpLookupObjectName+0x46b
35 ffffa801`64b2de30 fffff803`7934003a nt!ObOpenObjectByNameEx+0x1e0
36 ffffa801`64b2df70 fffff803`792b8eb4 nt!IopCreateFile+0x3aa
37 ffffa801`64b2e010 fffff803`793dfa61 nt!IoCreateFileEx+0x124
38 ffffa801`64b2e0a0 fffff80a`7336fdbd nt!IoCreateFileSpecifyDeviceObjectHint+0xf1
39 ffffa801`64b2e160 fffff80a`73370b4d DeviceLockDriver0!InternalCreateFileForCreateRequest+0x8ad [d:\work\dl2\8.2\windows\drivers\private\dldriver\fsdhook.c @ 1637]
3a ffffa801`64b2e360 fffff803`78eac20b DeviceLockDriver0!InternalCreateFileForCreateRequestCallback+0x2d [d:\work\dl2\8.2\windows\drivers\private\dldriver\fsdhook.c @ 1917]
3b ffffa801`64b2e3a0 fffff80a`73370d5c nt!KeExpandKernelStackAndCalloutInternal+0x8b
3c ffffa801`64b2e3f0 fffff80a`733721ab DeviceLockDriver0!InternalCreateFile+0x1ec [d:\work\dl2\8.2\windows\drivers\private\dldriver\fsdhook.c @ 2077]
3d ffffa801`64b2e4b0 fffff80a`733737ee DeviceLockDriver0!AddFileDescriptor+0xe4b [d:\work\dl2\8.2\windows\drivers\private\dldriver\fsdhook.c @ 2479]
3e ffffa801`64b2e5e0 fffff80a`73375190 DeviceLockDriver0!DldProcessPreOperationForFsdIrp+0x62e [d:\work\dl2\8.2\windows\drivers\private\dldriver\fsdhook.c @ 3437]
3f ffffa801`64b2e7b0 fffff80a`73365e1b DeviceLockDriver0!FileContentHookedDispatch+0xd0 [d:\work\dl2\8.2\windows\drivers\private\dldriver\fsdhook.c @ 4057]
40 ffffa801`64b2e810 fffff80a`73367fbe DeviceLockDriver0!DldFsdPreoperationCallback+0x64eb [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 6993]
41 ffffa801`64b2eef0 fffff80a`7336876a DeviceLockDriver0!GeneralFsdHookedDispatchEx+0x30e [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 7922]
42 ffffa801`64b2f130 fffff80a`73369333 DeviceLockDriver0!GeneralFsdHookedDispatch+0x5a [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 8205]
43 ffffa801`64b2f1a0 fffff80a`7331b3ff DeviceLockDriver0!FltmgrHookedDispatch+0x23 [d:\work\dl2\8.2\windows\drivers\private\dldriver\filesystems.c @ 8913]
44 ffffa801`64b2f1d0 fffff80a`7331b550 DeviceLockDriver0!DldDispatchWraperCallback+0xcf [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1303]
45 ffffa801`64b2f220 fffff80a`7331bbc3 DeviceLockDriver0!DldDispatchWraper+0x130 [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1332]
46 ffffa801`64b2f2c0 fffff80a`7331bfe4 DeviceLockDriver0!DlHookerFunctionChecker+0x123 [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1484]
47 ffffa801`64b2f340 fffff803`7932d200 DeviceLockDriver0!FltmgrHookedDispatchDBG+0x24 [d:\work\dl2\8.2\windows\drivers\private\dldriver\driver_hooker.c @ 1535]
48 ffffa801`64b2f370 fffff803`7933858b nt!IopParseDevice+0x7f0
49 ffffa801`64b2f540 fffff803`7933c0c0 nt!ObpLookupObjectName+0x46b
4a ffffa801`64b2f710 fffff803`792d1e90 nt!ObOpenObjectByNameEx+0x1e0
4b ffffa801`64b2f850 fffff803`7900d313 nt!NtQueryAttributesFile+0x180
4c ffffa801`64b2fb00 00007ffe`b86d5b54 nt!KiSystemServiceCopyEnd+0x13
4d 00000000`047cd438 00007ffe`b4cdaabe ntdll!NtQueryAttributesFile+0x14
4e 00000000`047cd440 00000000`00000000 0x00007ffe`b4cdaabe
```
