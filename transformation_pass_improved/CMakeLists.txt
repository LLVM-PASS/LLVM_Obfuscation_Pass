cmake_minimum_required(VERSION 3.20)
project(llvm-transformation-pass)

set(LT_LLVM_INSTALL_DIR "" CACHE PATH "LLVM installation directory")

# LLVMConfig.cmake location
list(APPEND CMAKE_PREFIX_PATH "${LT_LLVM_INSTALL_DIR}/lib/cmake/llvm/")
find_package(LLVM CONFIG)

if("${LLVM_VERSION_MAJOR}" VERSION_LESS 18)
  message(FATAL_ERROR "Found LLVM ${LLVM_VERSION_MAJOR}, but need LLVM 18 or above")
endif()

add_definitions(${LLVM_DEFINITIONS})
include_directories(${LLVM_INCLUDE_DIRS})

# C++ standard
set(CMAKE_CXX_STANDARD 17 CACHE STRING "")

# LLVM is normally built without RTTI
if(NOT LLVM_ENABLE_RTTI)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-rtti")
endif()

# Adding the target
add_library(llvm-transformation-pass-improved SHARED IR_Transformation_Improved.cpp)

target_link_libraries(llvm-transformation-pass-improved
  "$<$<PLATFORM_ID:Darwin>:-undefined dynamic_lookup>")
