# PHY-480-sample-automake
A sample CMake system for the Project 2 sample code from PHY 480

## Information
The sample project was drafted up by David Miller (mill2723 at msu dot edu). If you have any questions or concerns they are likely best directed to him. It is licensed under the MIT License (please see LICENSE file for details).

## Using this project

In order to use this you need to go through the standard process for
initializing and compiling with CMake

1. First run `cmake ./` which intializes CMake in the current directory
2. Run `make` to compile the program(s)

You can run `cmake --help` for additional information

## Prerequisites

I compiled and tested this on Ubuntu 16.04 running the following versions:
+ gcc       -   5.4.0
+ g++       -   5.4.0
+ gfortran  -   5.4.0
+ cmake     -   3.5.1
+ armadillo -   6.500.5
+ lapack    -   3.6.0
+ blas      -   3.6.0

These should **not** represent the minimum versions required to run this but your results *may* vary for different versions (both older and newer).
