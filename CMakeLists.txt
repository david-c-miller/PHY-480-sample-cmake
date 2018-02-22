cmake_minimum_required (VERSION 2.6)
project (phy480-cmake-example)

#versioning
set (phy480-cmake-example_VERSION_MAJOR 1)
set (phy480-cmake-example_VERSION_MINOR 0)

## include directories (if different)
#include_directories(inc)

## set flags
set (CMAKE_CXX_FLAGS "-O0 -g")

## set sources
set(SOURCES src/TridiagToeplitz.cpp)

## check for libraries and header files
find_library(ARMA_LIB armadillo)
find_library(LAPACK_LIB lapack)
find_library(BLAS_LIB blas)

## add a binary file to be built
## this places it in the bin directory
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY bin)
set(PROG "Tridiag.x")

add_executable(${PROG} ${SOURCES})

## declare subdirs
## add_subdirectory(src/UnitTestscpp/JacobiTests EXCLUDE_FROM_ALL)

## external build targets
set(JACOBI_DIR src/UnitTestscpp/JacobiTests)
set(TOEPLITZ_DIR src/UnitTestscpp/ToeplitzTests)

file(GLOB testcode_jacobi_sources ${JACOBI_DIR}/*.cpp ${JACOBI_DIR}/*.hpp ${JACOBI_DIR}/*.h)
file(GLOB testcode_toeplitz_sources ${TOEPLITZ_DIR}/*.cpp ${TOEPLITZ_DIR}/*.hpp ${TOEPLITZ_DIR}/*.h)

add_custom_target(
    testcode_jacobi
    COMMAND make
    WORKING_DIRECTORY ${JACOBI_DIR}
    SOURCES ${testcode_jacobi_sources}
)

add_custom_target(
    testcode_toeplitz
    COMMAND make
    WORKING_DIRECTORY ${TOEPLITZ_DIR}
    SOURCES ${testcode_toeplitz_sources}
)

add_custom_target(
    testcode ALL
    DEPENDS testcode_jacobi testcode_toeplitz
)

## link external libraries
target_link_libraries(${PROG} ${ARMA_LIB})
target_link_libraries(${PROG} ${LAPACK_LIB})
target_link_libraries(${PROG} ${BLAS_LIB})
