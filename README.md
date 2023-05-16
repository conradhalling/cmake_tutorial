# cmake_tutorial

## Introduction

This repository contains a copy of the CMake Tutorial from
https://cmake.org/cmake/help/latest/guide/tutorial/index.html.

The directory contains source code examples for the CMake Tutorial.
Each step has its own subdirectory containing code that may be used as a
starting point. The tutorial examples are progressive so that each step
provides the complete solution for the previous step.

Since the CMake documentation doesn't provide examples for the CMake commands,
I include the contents of each CMakeLists.txt file here. I have deviated
slightly from the tutorial instructions for:

-   the minimum required CMake version
-   the name of the executable
-   one of the examples for calling the executable

## Step01: A Basic Starting Point

### Exercise 1: Building a Basic Project

This exercise creates a minimal CMakeLists.txt file for building an executable
from a single code file (where I used CMake 3.26.3):

```cmake
cmake_minimum_required(VERSION 3.26)
project(Tutorial)
add_executable(tutorial tutorial.cxx)
```

Build and run the executable:

```shell
cd ~/src/conradhalling/cmake_tutorial/Step01
mkdir build
cd build
cmake ..
cmake --build .
./tutorial 65536
./tutorial 10
./tutorial
```

The output looks like this:

    $ mkdir build

    $ cd build

    $ cmake ..
    -- The C compiler identification is AppleClang 14.0.3.14030022
    -- The CXX compiler identification is AppleClang 14.0.3.14030022
    -- Detecting C compiler ABI info
    -- Detecting C compiler ABI info - done
    -- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc - skipped
    -- Detecting C compile features
    -- Detecting C compile features - done
    -- Detecting CXX compiler ABI info
    -- Detecting CXX compiler ABI info - done
    -- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ - skipped
    -- Detecting CXX compile features
    -- Detecting CXX compile features - done
    -- Configuring done (0.4s)
    -- Generating done (0.0s)
    -- Build files have been written to: /Users/halto/src/conradhalling/cmake_tutorial/Step01/build

    $ cmake --build .
    [ 50%] Building CXX object CMakeFiles/tutorial.dir/tutorial.cxx.o
    [100%] Linking CXX executable tutorial
    [100%] Built target tutorial

    $ ./tutorial 65536
    The square root of 65536 is 256

    $ ./tutorial 10
    The square root of 10 is 3.16228

    $ ./tutorial
    Usage: ./tutorial number

### Exercise 2: Specifying the C++ Standard

This exercise modifies the CMakeLists.txt file to specify the C++11 standard
(because the modified tutorial.cxx file calls `std::stod()`, a C++11
feature):

```cmake
cmake_minimum_required(VERSION 3.26)
project(Tutorial)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
add_executable(tutorial tutorial.cxx)
```

The same shell commands are used to build and run the executable:

```shell
cd ~/src/conradhalling/cmake_tutorial/Step01
rm -rf build
mkdir build
cd build
cmake ..
cmake --build .
./tutorial 65536
./tutorial 10
./tutorial
```

### Exercise 3: Adding a Version Number and Configured Header File

This exercise shows how to specify a project version and include it in the
executable via a CMake-configured header file, TutorialConfig.h.in. This is
the CMakeLists.txt file:

```cmake
cmake_minimum_required(VERSION 3.26)
project(Tutorial VERSION 1.0)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
configure_file(TutorialConfig.h.in tutorial.h)
add_executable(tutorial tutorial.cxx)
target_include_directories(tutorial PUBLIC "${PROJECT_BINARY_DIR}")
```

This is the TutorialConfig.h.in file:

```C++
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
```

The file tutorial.cxx was modified to add the following line:

```C++
#include "tutorial.h"
```

This modification makes VSCode unhappy because it can't find the
tutorial.h file included by the tutorial.cxx file.
