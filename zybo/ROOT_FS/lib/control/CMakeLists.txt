cmake_minimum_required(VERSION 2.8)
project(control)

IF(NOT CMAKE_BUILD_TYPE)
  SET(CMAKE_BUILD_TYPE Release)
ENDIF()

MESSAGE("Build type: " ${CMAKE_BUILD_TYPE})

#-- enable output compile_command.json
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

#-- cmake -DCMAKE_BUILD_TYPE=debug ..
set(CMAKE_CXX_FLAGS_DEBUG   "-O0 -g -MMD -Wall -Wextra -Winit-self")

#-- cmake ..
# -march=native : CPUに合わせた最適化オプションが得られるようになる
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native ")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall   -O3 -march=native")

# Check C++14 support
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++14" COMPILER_SUPPORTS_CXX14)
if(COMPILER_SUPPORTS_CXX14)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")
  add_definitions(-DCOMPILEDWITHC11)
  message(STATUS "Using flag -std=c++14.")
else()
  message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++14 support. Please use a different C++ compiler.")
endif()

include_directories(
  ${PROJECT_SOURCE_DIR}/include
  )

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/build)

add_library(${PROJECT_NAME} SHARED
  src/PID/PID.cpp
  )
