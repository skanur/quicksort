cmake_minimum_required(VERSION 2.6)
project(qsort)

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Intel")
        set(MATH_LIB "")
else()
    set(MATH_LIB m)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fcilkplus")
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fcilkplus")
endif()

add_executable(qsort qsort.cpp cilktime.h)
