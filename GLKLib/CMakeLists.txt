cmake_minimum_required(VERSION 3.11...3.18)

set(PROJECT_NAME GLKLib)
project (${PROJECT_NAME} CXX)

set(SRC_FILES
    GLK.cpp
    GLKGeometry.cpp
    GLKGraph.cpp
    GLKHeap.cpp
    GLKMatrixLib.cpp
    GLKObList.cpp
    GLK.h
    GLKCameraTool.h
    GLKGeometry.h
    GLKGLList.h
    GLKGraph.h
    GLKHeap.h
    GLKMatrixLib.h
    GLKObList.h
)

add_library(${PROJECT_NAME} STATIC
            ${SRC_FILES})

if (MSVC)
endif (MSVC)
if (UNIX)
endif (UNIX)
