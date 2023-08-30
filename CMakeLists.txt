cmake_minimum_required(VERSION 3.15)

project(libelfin CXX)

set(CMAKE_CXX_STANDARD 17)

add_subdirectory(elf)
add_subdirectory(dwarf)

# examples
# option to build examples
option(BUILD_EXAMPLES "Build examples" OFF)
if (BUILD_EXAMPLES)
    message(STATUS "Building examples")
    add_subdirectory(examples)
endif ()