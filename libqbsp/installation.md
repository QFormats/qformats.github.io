---
title: installation
layout: default
parent: libqbsp
toc_enabled: false
---

### Installation

This Library usees CMake, you can easily download the lib and load it into your project.

here is a minimal cmake file that includes the lib from a subfolder:

```cmake
cmake_minimum_required(VERSION 3.10)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_VERBOSE_MAKEFILE 1)

project(
    bspviewer
    VERSION 1.0
    LANGUAGES CXX
)

add_subdirectory(deps/libqbsp)

add_executable( ${PROJECT_NAME}
    src/main.cpp
)

target_link_libraries(${PROJECT_NAME}
    qbsp
)
```