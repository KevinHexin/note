### resnet50

platfrom | cpu-core | openMP | img-size | data-type | AVX2/NEON | 耗时(ms)
:-:|:-:|:-:|:-:|:-:|:-:|:-:
x86 | E5-2630@2.20GHz-40 | OFF | 313x513 | FP32 | OFF | 8343 |
x86 | E5-2630@2.20GHz-40 | ON | 313x513 | FP32 | OFF | 2643 |
x86 | E5-2630@2.20GHz-40 | ON | 313x513 | FP32 | ON | 1528 |
aarch64 | RK-3390Pro@1.416GHz-6 | ON | 313x513 | FP32 | ON | 6649 |