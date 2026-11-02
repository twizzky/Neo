cmake_minimum_required(VERSION 3.20)

project(NEO
    VERSION 0.1.0
    DESCRIPTION "NEO Programming Language Compiler"
    LANGUAGES C CXX
)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(LLVM REQUIRED CONFIG)

message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "LLVM installation: ${LLVM_DIR}")

add_executable(neo
    compiler/src/main.cpp
)

target_include_directories(neo SYSTEM PRIVATE
    ${LLVM_INCLUDE_DIRS}
)

separate_arguments(LLVM_DEFS NATIVE_COMMAND "${LLVM_DEFINITIONS}")
add_definitions(${LLVM_DEFS})

llvm_map_components_to_libnames(LLVM_LIBS
    Core
    Support
    IRReader
    Analysis
    Target
    MC
    CodeGen
)

target_link_libraries(neo PRIVATE
    ${LLVM_LIBS}
)