
APC delivering to a waiting thread

```
        ffffda06`25699030 fffff803`66cc7811 nt!KiDeliverApc+0x27b
        ffffda06`256990c0 fffff803`66cc6dcb nt!KiSwapThread+0x501
        ffffda06`25699190 fffff803`66cc5cc7 nt!KiCommitThreadWait+0x13b
        ffffda06`25699230 fffff803`671724b0 nt!KeWaitForMultipleObjects+0x467
        ffffda06`25699310 fffff803`67172fd7 nt!ObWaitForMultipleObjects+0x2a0
        ffffda06`25699810 fffff803`66e47a43 nt!NtWaitForMultipleObjects+0xf7
        ffffda06`25699a90 00007ff8`1369aa44 nt!KiSystemServiceCopyEnd+0x13 (TrapFrame @ ffffda06`25699b00)
```
