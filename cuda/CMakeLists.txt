cmake_minimum_required(VERSION 3.18 FATAL_ERROR)
project(VectorAdd CXX C CUDA)
set(CMAKE_CUDA_FLAGS "-arch=compute_50 -g -G -O3")

# 设置CUDA架构
set(CMAKE_CUDA_ARCHITECTURES 50) # 适用于GTX 750 Ti

add_executable(vector_add vector_add.cu)

set_target_properties(vector_add PROPERTIES
    CUDA_SEPARABLE_COMPILATION ON)
