A write through cache flush for a removable drive

```
07 nt!IofCallDriver
08 nt!IoSynchronousPageWriteEx
09 nt!MiIssueSynchronousFlush
0a nt!MiFlushSectionInternal
0b nt!MmFlushSection
0c nt!CcFlushCachePriv
0d nt!CcMapAndCopyInToCache
0e nt!CcCopyWriteEx
0f exfat!FppCommonWrite
10 exfat!FppFsdWrite
11 nt!IofCallDriver
12 FLTMGR!FltpLegacyProcessingAfterPreCallbacksCompleted
13 FLTMGR!FltpDispatch
.... <private>
1a nt!IofCallDriver
1b nt!IopSynchronousServiceTail
1c nt!NtWriteFile
1d nt!KiSystemServiceCopyEnd
1e ntdll!NtWriteFile
```
