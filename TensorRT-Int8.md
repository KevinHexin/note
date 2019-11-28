### 理论

>ref:<br>
https://zhuanlan.zhihu.com/p/58182172

### TRT如何校准

>ref:<br>
https://github.com/NVIDIA/TensorRT/tree/master/samples/opensource/sampleINT8<br>

### 具体实现

参看`openpifpaf-int8-calibration`项目与注释<br>

使用`60000`张图片生成校准表(来自`MSCOCO2017 keypoint`数据集, 共`118288`张图片), 计算非常慢(约5天跑完)<br>

### 效果

GTX 1080Ti<br>

输入 | 精度 | 耗时(ms) | 模型大小 | 效果
:-:|:-:|:-:|:-:|:-:
313x513 | FP32 | ~32ms | 96M | 基准 |
313x513 | INT8 | ~17ms | 23M | -1% |
