#########################################################################
## A Sample CMake Make System
## Written by David Miller (mill2723 at msu dot edu)

## Set a minimum version
cmake_minimum_required (VERSION 2.8)

## set compilers (if needed)
# set (CMAKE_C_COMPILER "gcc")
set (CMAKE_CXX_COMPILER "g++")
#########################################################################

#########################################################################
## name the project (VERSION statement requires 3.0 or newer)
project (PHY480_CMAKE_EXAMPLE LANGUAGES CXX Fortran C)
# project (PHY480_CMAKE_EXAMPLE VERSION 1.0.0 LANGUAGES CXX Fortran C)

## version for the project
set (PHY480_CMAKE_EXAMPLE_MAJOR_VERSION 1)
set (PHY480_CMAKE_EXAMPLE_MINOR_VERSION 0)
set (PHY480_CMAKE_EXAMPLE_PATCH_VERSION 0)
#########################################################################

#########################################################################
## include directories (if different)
# include_directories(inc)
#########################################################################

#########################################################################
## set compiler flags

set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O0 -g")
set (CMAKE_C_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra")

set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0 -g")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra")

## set sources
set (SOURCES src/TridiagToeplitz.cpp)
#########################################################################

#########################################################################
## check for header files
include (CheckIncludeFiles)
include (CheckIncludeFileCXX)

CHECK_INCLUDE_FILE_CXX (cmath       HAVE_CMATH)
CHECK_INCLUDE_FILE_CXX (iostream    HAVE_IOSTREAM)
CHECK_INCLUDE_FILE_CXX (fstream     HAVE_FSTREAM)
CHECK_INCLUDE_FILE_CXX (iomanip     HAVE_IOMANIP)
CHECK_INCLUDE_FILE_CXX (armadillo   HAVE_ARMADILLO)
#########################################################################

#########################################################################
## check for libraries

## some libraries have a cmake module
include (FindLAPACK)
include (FindBLAS)

## this module can be used to check for C Libraries
## include (CheckLibraryExists)

## For C++ use find_library
find_library (ARMA_LIBRARIES  armadillo)

if (ARMA_LIBRARIES STREQUAL "ARMA_LIBRARIES-NOTFOUND")
	message (FATAL_ERROR "Armadillo library not found" )
else ()
	message (STATUS "Found library: ${ARMA_LIBRARIES}" )
endif ()
#########################################################################

#########################################################################
## add a binary file to be built
## this places it in the bin directory
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY bin)
set (PROG "Tridiag.x")

add_executable (${PROG} ${SOURCES})

## link external libraries
target_link_libraries (${PROG} ${ARMA_LIBRARIES} ${LAPACK_LIBRARIES}  ${BLAS_LIBRARIES})
#########################################################################

## this is the end of a typical CMake file

#########################################################################
## This is a kinda hacked together method to force compilation of
## subdirectories with their own Makefiles
## This is **definitely** not the best way of doing this but is a good
## example of how to make this work if you were building something else
## in a separate subdir (think documentation or figures)

## If you have a CMakeLists.txt file in the subdirectory you can just
## use this to declare subdirs
## add_subdirectory (src/UnitTestscpp/JacobiTests EXCLUDE_FROM_ALL)

## Otherwise we setup some external build targets and then create a single
## target for `make all` which depends on the individual targets

## external build targets
set (JACOBI_DIR src/UnitTestscpp/JacobiTests)
set (TOEPLITZ_DIR src/UnitTestscpp/ToeplitzTests)

## This gets all the files from the subdirectories which we want to rebuild
## on changes to
file (
    GLOB testcode_jacobi_sources
    ${JACOBI_DIR}/*.cpp
    ${JACOBI_DIR}/*.hpp
    ${JACOBI_DIR}/*.h
)

file (
    GLOB testcode_toeplitz_sources
    ${TOEPLITZ_DIR}/*.cpp
    ${TOEPLITZ_DIR}/*.hpp
    ${TOEPLITZ_DIR}/*.h
)

add_custom_target (
    testcode_jacobi
    COMMAND make
    WORKING_DIRECTORY ${JACOBI_DIR}
    SOURCES ${testcode_jacobi_sources}
)

add_custom_target (
    testcode_toeplitz
    COMMAND make
    WORKING_DIRECTORY ${TOEPLITZ_DIR}
    SOURCES ${testcode_toeplitz_sources}
)

add_custom_target (
    testcode ALL
    DEPENDS testcode_jacobi testcode_toeplitz
)
#########################################################################
