set(CMAKE_CUDA_ARCHITECTURES "75;86")

cmake_minimum_required (VERSION 3.17)
project(ACMMP LANGUAGES CXX CUDA)

find_package(OpenCV REQUIRED COMPONENTS calib3d imgcodecs)
find_package(OpenMP REQUIRED)
find_package(CUDAToolkit REQUIRED)

add_executable(ACMMP)
target_sources(ACMMP PRIVATE
        main.cpp
        ACMMP.cpp
        ACMMP.cu
        )
target_compile_options(ACMMP PRIVATE
        $<$<COMPILE_LANGUAGE:CXX>:
        -Wall
        -Wextra
        -pedantic
        -Wno-unused-function
        -Wno-switch
        -march=native
        -ffast-math
        >
        $<$<COMPILE_LANGUAGE:CUDA>:
        --use_fast_math
        --maxrregcount=128
        --compiler-options -Wall
        >
        )
target_compile_features(ACMMP PRIVATE cxx_std_17)
target_link_libraries(ACMMP PRIVATE OpenMP::OpenMP_CXX opencv_calib3d opencv_imgcodecs CUDA::cudart)
