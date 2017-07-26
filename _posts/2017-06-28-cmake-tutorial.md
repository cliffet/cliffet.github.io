---
layout: post
title: CMake tutorial
category: interest
tags: [dev tool]
---

## Step 1
CMakeLists.txt 가 필요한 파일의 전부  

```cmake
cmake_minimum_required (VERSION 2.6)
project (Tutorial)
add_executable(Tutorial tutorial.cxx) 
```
* CMakeLists.txt에서 명령들은 대소문자 가리지 않음  
* `cmake_minimum_required (???)`
* `project (이름)`
* `add_executable(프로젝트명 파일)`

```cmake
cmake_minimum_required (VERSION 2.6)
project (Tutorial)

# The version number
set (Tutorial_VERSION_MAJOR 1)
set (Tutorial_VERSION_MINOR 0)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file (
  "${PROJECT_SOURCE_FIR}/TutorialConfig.h.in"
  "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  )
  
# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
include_directories("${PROJECT_BINARY_DIR}")

# add the executable
add_executable(Tutorial tutorial.cxx)
```
* `set (변수명 값)`
* `configure_file(입력파일 출력파일)`
* `include_directories (경로)`

## Step 2
```cmake
cmake_minimum_required (VERSION 2.6)
project (Tutorial)

# should we use our own lib functions?
option (USE_MYLIB
  "Use tutorial provided lib implementation" ON)

# The version number
set (Tutorial_VERSION_MAJOR 1)
set (Tutorial_VERSION_MINOR 0)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file (
  "${PROJECT_SOURCE_FIR}/TutorialConfig.h.in"
  "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  )
  
# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
include_directories("${PROJECT_BINARY_DIR}")
include_directories("${PROJECT_SOURCE_DIR}/SomeLibrary")
add_subdirectory (SomeLibrary)

# add the SomeLibrary?
if (USE_MYLIB)
  include_directories ("${PROJECT_SOURCE_DIR}/SomeLibrary")
  add_subdirectory (SomeLibrary)
  set (EXTRA_LIBS ${EXTRA_LIBS} SomeLibrary)
endif (USE_MYLIB)

# add the executable
add_executable(Tutorial tutorial.cxx)
target_link_libraries (Tutorial ${EXTRA_LIBS})
```
* SomeLibrary를 라이브러리가 위치한 서브디렉토리
* `add_subdirectory (이름)`
* `target_link_libraries (프로젝트 이름)`
* `option(이름 설명 값)`
* USE_MYLIB의 설정은 TutorialConfig.h.in파일에 `#cmakedefine USE_MYLIB` 를 추가함으로써 선택할 수 있다

