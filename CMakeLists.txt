cmake_minimum_required (VERSION 2.6)
project (phy480-cmake-example)

#versioning
set (phy480-cmake-example_VERSION_MAJOR 1)
set (phy480-cmake-example_VERSION_MINOR 0)

## include directories (if different)
#include_directories(inc)

## set additional clean options
set_directory_properties(DIRECTORY ADDITIONAL_MAKE_CLEAN_FILES "*~")

## set flags
set (CMAKE_CXX_FLAGS "-O0 -g")

## set sources
set(SOURCES src/TridiagToeplitz.cpp)

## add a binary file to be built
add_executable(Tridiag.x ${SOURCES})

## check for libraries and header files
find_library(ARMA_LIB armadillo)
find_library(LAPACK_LIB lapack)
find_library(BLAS_LIB blas)

target_link_libraries(Tridiag.x ${ARMA_LIB})
target_link_libraries(Tridiag.x ${LAPACK_LIB})
target_link_libraries(Tridiag.x ${BLAS_LIB})
