## Google Test FrameWork Setup

This is a simple guide to proceed with the configuration of the Google Test Framework, all the functional documentation is described on the site: http://google.github.io/googletest/
Althoug Google Test supports:
#### Operating systems
- Windows
- Linux
- macOS

#### Compiler Tools
- gcc 5.0+
- clang 5.0+
- MSVC 2015+
#### Bulid systems
- Bazel
- CMake

For this guide, the following platform was taken into account:
- Compiler Tool: MSVC 2019
- Operating system: Windows
- Build system: CMake

#### 1.: Locate the working directory (eg WorkspaceFolder) and clone the GoogleTest Framework from: https://github.com/google/googletest

```BASH
PS C:\WorkspaceFolder> git clone https://github.com/google/googletest.git
Cloning into 'googletest'...
remote: Enumerating objects: 25960, done.
remote: Counting objects: 100% (265/265), done.
remote: Compressing objects: 100% (113/113), done.
remote: Total 25960 (delta 155), reused 208 (delta 133), pack-reused 25695
Receiving objects: 100% (25960/25960), 12.12 MiB | 2.71 MiB/s, done.
Resolving deltas: 100% (19161/19161), done.
```

#### 2.: Create master file CMakeLists.txt in the work folder (eg WorkspaceFolder) and write the following lines:
CMakeLists.txt
```CMake
cmake_minimum_required(VERSION 3.8)

#sets name output log folder:
set(This Outputlog)

# declare what languages (C/C++) to use:
project(${This} C CXX)

# GoogleTest requires at least C++14:
set(CMAKE_C_STANDARD 99)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

#CMake enables testing
enable_testing()

#CMake sets googletest subfolder
add_subdirectory(googletest)

#Headers and Sources that represent our code to be evaluated
set(Headers
    my_source.hpp
)

set(Sources
    my_source.cpp
)

add_library(${This} STATIC ${Sources} ${Headers})

#CMake sets children test subfolder
add_subdirectory(TestFolder)
```
![image](/images/i1.png)

#### 3.: We create our source code files or only export the header file if we already have it. There would be all the code to be tested, be it functions or classes.
![image](/images/i2.png)

#### 4.: We create the test scope:
#### 4.a: We create a subfolder (eg TestFolder) where our test files will be located
#### 4.b: We create a secondary file CMakeLists.txt; We add the following lines:
CMakeLists.txt
```CMake
cmake_minimum_required(VERSION 3.8)

# executable output file
set(This my_output)

set(Sources
    main_test.cpp
)

add_executable(${This} ${Sources})

# Specify libraries or flags to use when linking a given target
target_link_libraries(${This} PUBLIC
    gtest_main
    Outputlog
)

add_test(
    NAME ${This}
    COMMAND ${This}
)
```
![image](/images/i3.png)

#### 4.c: We create a main file where it will execute the test macros
- With the main_test.cpp file completely empty we must compile to prepare the environment so we execute:
- `CTRL + SHIFT + P` (so that there will be a command selector)
- We write: `Developer: Reload Windows`

![image](/images/i4.png)

- Once our VSCODE is restarted we go back to `CTRL + SHIFT + P` to choose which tool we are going to compile our Framework with

![image](/images/i5.png)

Once the build tool is chosen, something like this should appear:

```Bash
[cmake] -- Build files have been written to: C:/WorkspaceFolder/build
```

![image](/images/i6.png)

#### 5.: Once CMake is configured the next step is to compile the entire empty project for that we press: `CTRL + SHIFT + P` and type `CMake: Build`

![image](/images/i7.png)

Until you get the following result

![image](/images/i8.png)

#### 6.: Next step in defining the main_test.cpp file where the macros that execute the tests are defined.

![image](/images/i9.png)

Check that the Intellisense is taking the Include path otherwise something bad is happening.

#### 7.: We write our first simple code to verify that the test environment works properly from here we do the following steps:

#### 7.a: We define a macro of those documented at: http://google.github.io/googletest/ 
A simple way would be in the file main_test.cpp

```cpp
#include <gtest/gtest.h>

TEST(test_suite_name, test_name){
    EXPECT_TRUE(true);
}
```
#### 7.b: We compile using: CTRL + SHIFT + P and write CMake: Build as we saw in point 5. 
With the notification that after compiling appears:
```bash
[build] Build finished with exit code 0
```

#### 7.c: We launch the CMake test `CTRL + SHIFT + P` and type `CMake: Run Tests`

![image](/images/i10.png)

And we verify that the following notification appears:

```bash
[ctest] CTest finished with return code 0
```

In these 7 steps we leave the framework configured to start with what we want to test:

![image](/images/i11.png)

