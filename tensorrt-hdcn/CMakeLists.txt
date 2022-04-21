cmake_minimum_required(VERSION 2.6)

project(yolov5)

add_definitions(-std=c++11)
add_definitions(-DAPI_EXPORTS)
option(CUDA_USE_STATIC_CUDA_RUNTIME OFF)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_BUILD_TYPE Debug)

find_package(CUDA REQUIRED)
if(WIN32)
enable_language(CUDA)
endif(WIN32)


include_directories(${PROJECT_SOURCE_DIR}/include)
# include and link dirs of cuda and tensorrt, you need adapt them if yours are different
# cuda
include_directories(/usr/local/cuda/include)
link_directories(/usr/local/cuda/lib64)
#cublas
#include_directories(/usr/local/cuda-11.3/targets/x86_64-linux/include)
# ln -s /usr/local/cuda/include/cublas_v2.h /usr/local/cuda-11.3/targets/x86_64-linux/include/cublas_v2.h
# tensorrt
include_directories(/usr/include/x86_64-linux-gnu/)
link_directories(/usr/lib/x86_64-linux-gnu/)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -Ofast -g -Wfatal-errors -D_MWAITXINTRIN_H_INCLUDED")
cuda_add_library(myplugins SHARED yololayer.cu)
target_link_libraries(myplugins nvinfer cudart)

find_package(OpenCV)
include_directories(${OpenCV_INCLUDE_DIRS})

cuda_add_executable(yolov5 calibrator.cpp yolov5.cpp preprocess.cu common_cuda_helper.hpp deform_conv_cuda_kernel.cuh deform_roi_pool_cuda_kernel.cuh trt_deform_conv.hpp trt_deform_conv_kernel.cu trt_deform_conv.cpp trt_cuda_helper.cuh trt_plugin_helper.hpp trt_deform_conv.hpp trt_serialize.hpp)

target_link_libraries(yolov5 nvinfer)
target_link_libraries(yolov5 cudart)
target_link_libraries(yolov5 myplugins)
target_link_libraries(yolov5 ${OpenCV_LIBS})

if(UNIX)
add_definitions(-O2 -pthread)
endif(UNIX)


