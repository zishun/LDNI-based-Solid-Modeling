cmake_minimum_required(VERSION 3.11...3.18)

if (UNIX)
    set(CMAKE_CUDA_COMPILER /usr/local/cuda/bin/nvcc)
endif (UNIX)

set(PROJECT_NAME LDNIcuda)
project (${PROJECT_NAME} LANGUAGES CUDA CXX)

add_subdirectory("./GLKLib")

set(SRC_FILES
    ./LDNIcuda/LDNIcpuOperation.cpp
    ./LDNIcuda/LDNIcpuSolid.cpp
    ./LDNIcuda/LDNIDataBoard.cpp
    ./LDNIcuda/main.cpp
    ./LDNIcuda/PMBody.cpp
    ./LDNIcuda/LDNIcudaContouring.cu
    ./LDNIcuda/LDNIcudaNormalProcessing.cu
    ./LDNIcuda/LDNIcudaOffsetting.cu
    ./LDNIcuda/LDNIcudaOffsettingByHashing.cu
    ./LDNIcuda/LDNIcudaOperation.cu
    ./LDNIcuda/LDNIcudaSolid.cu
    ./LDNIcuda/LDNIcpuOperation.h
    ./LDNIcuda/LDNIcpuSolid.h
    ./LDNIcuda/LDNIcudaOperation.h
    ./LDNIcuda/LDNIcudaSolid.h
    ./LDNIcuda/LDNIDataBoard.h
    ./LDNIcuda/PMBody.h
    ./LDNIcuda/scan_common.h
)

add_executable(${PROJECT_NAME} ${SRC_FILES})
target_link_libraries(${PROJECT_NAME} GLKLib)

# CUDA
find_library(CUDA_LIBRARY_CUDART cudart ${CMAKE_CUDA_IMPLICIT_LINK_DIRECTORIES})
find_library(CUDA_LIBRARY_CUSOLVER cusolver ${CMAKE_CUDA_IMPLICIT_LINK_DIRECTORIES})
find_library(CUDA_LIBRARY_CUSPARSE cusparse ${CMAKE_CUDA_IMPLICIT_LINK_DIRECTORIES})
target_include_directories(
    ${PROJECT_NAME}
    PRIVATE
    ${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES}
)
target_link_libraries(
    ${PROJECT_NAME}
    ${CUDA_LIBRARY_CUDART}
    ${CUDA_LIBRARY_CUSOLVER}
    ${CUDA_LIBRARY_CUSPARSE}
)

target_link_libraries(
    ${PROJECT_NAME}
    GL GLU glut
)

# glew
find_package(GLEW REQUIRED)
include_directories(${GLEW_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME} ${GLEW_LIBRARIES})

# openmp
find_package(OpenMP)
if(OpenMP_CXX_FOUND)
    target_link_libraries(${PROJECT_NAME} OpenMP::OpenMP_CXX)
endif()

add_custom_command(TARGET ${PROJECT_NAME} PRE_BUILD
            COMMAND ${CMAKE_COMMAND} -E create_symlink
            ${CMAKE_SOURCE_DIR}/LDNIcuda/shader
            ${CMAKE_CURRENT_BINARY_DIR}/shader
)
add_custom_command(TARGET ${PROJECT_NAME} PRE_BUILD
            COMMAND ${CMAKE_COMMAND} -E create_symlink
            ${CMAKE_SOURCE_DIR}/LDNIcuda/rp_files
            ${CMAKE_CURRENT_BINARY_DIR}/rp_files
)

if (MSVC)
endif (MSVC)
if (UNIX)
endif (UNIX)
