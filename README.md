# yolov5-hdcn
yolov5-hdcn is a project fusion with YOLOV5 and Half-Deformable Convolution, since the original dcn-v2 is not work on high version pytorch, we introduce the mmcv.
## Reference
* [TensorrtX](https://github.com/wang-xinyu/tensorrtx) provide  the implementation of yolov5 with TensorRT API.
* [MMCV](https://github.com/open-mmlab/mmcv) provide the implementation of relative operations and TensorRT implementation.
* [Yolov5](https://github.com/ultralytics/yolov5) provide the framework of Yolov5.
## Floder torch-hdcn
Based [this](https://github.com/ultralytics/yolov5) project provided yolov5-v6 we build the HDCN module and PIS loss.
## Floder tensorrt-hdcn
Based [this](https://github.com/wang-xinyu/tensorrtx),we deploy our module in RTX3090,Jetson Nano, Jetson Tx2, which about 3x faster than the python implementation.
## Quick Start
1 clone our project to your device

* `git clone https://github.com/entropyfeng/yolov5-hdcn.git`

2 build our project and the more detail guidance can be achive from [this](https://github.com/wang-xinyu/tensorrtx/tree/master/yolov5). 
```
cd {projectPath}
mkdir build
cd build
cmake ..
make
```
