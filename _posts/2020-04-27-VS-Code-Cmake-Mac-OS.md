---
title: VS Code + Cmake + Mac OS
date: 2020-04-27 19:11:45 +0800
categories: [踩坑总结, 环境配置]
tags: [tips]
---
学校邮箱被ban，jetbrain家的软件都无法使用。Mac也不能用宇宙第一IDE，无奈写C/C++项目只能白手起家。
## 总工程概览
```
Project:
│ 
├── bin                  可执行文件夹 
│   └── test             测试文件夹
├── build                cmake缓存目录 
├── include              头文件目录
│   └── utils.h
├── make                 bash脚本
├── readme.md            本文
├── src                  源文件目录
│   ├── main.cpp     
│   └── utils.cpp    
└── CMakeLists.txt       主目录 CMakeLists
```
## 新建工程目录
新建工程目录, 用VS Code打开, 随意命名为 CppTest
执行下列命令, 构建工程的基本框架:
```bash
mkdir .vscode bin build include src  
cd ../src
touch main.cpp utils.cpp
cd ..
touch include/utils.h
```
创建VS Code配置文件（可省）
```
cd ../.vscode
touch c_cpp_properties.json launch.json settings.json tasks.json
```

## 安装Cmake
```bash
brew install cmake
cmake --version
```
cmake 所需的主要文件就是 CMakeLists.txt, 我们可以通过编写CMakeLists.txt 调用 gcc 等编译器, 省去了每次输入冗长命令的麻烦。

## CmakeLists.txt
写入以下内容：
```xml
CMAKE_MINIMUM_REQUIRED(VERSION 2.8) # cmake最低版本要求

PROJECT(CppTemplate)    # 工程名 CppTemplate

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11") # 添加c++11标准支持

SET(CMAKE_C_COMPILER "/usr/bin/gcc") # 默认c编译器
SET(CMAKE_CXX_COMPILER "/usr/bin/g++") # 默认c++编译器

SET(CMAKE_BUILD_TYPE "Debug")  # Debug模式 选项: Release Debug MinSizeRel RelWithDebInfo

SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g2 -ggdb")  # debug模式下 gdb相关选项
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall") # release模式下 gdb相关选项

# set(CMAKE_VERBOSE_MAKEFILE ON) # 开启调试 出现问题时开启

# enable_testing() # 打开测试

add_definitions(-DDEBUG) # 相当于代码中 #define DEBUG

# add_subdirectory(test) # 添加test子目录

SET(EXECUTABLE_OUTPUT_PATH "${PROJECT_SOURCE_DIR}/bin") # 可执行文件输出目录

INCLUDE_DIRECTORIES("${PROJECT_SOURCE_DIR}/include") # 头文件包含目录

# 这段代码可以区分操作系统
MESSAGE("Identifying the OS...")
if(WIN32)
  MESSAGE("This is Windows.")
elseif(APPLE)
  MESSAGE("This is MacOS.")
elseif(UNIX)
  MESSAGE("This is Linux.")
endif()
# 这段代码可以区分操作系统

AUX_SOURCE_DIRECTORY(src DIR_SRCS) # 添加源代码文件夹, 自动扫描所有文件

add_executable(chess  # 输出名为chess的可执行文件
   ${DIR_SRCS}
)
# 也可以这么写
# add_executable(chess  # 输出名为chess的可执行文件
#    ./src/main.cpp
#    ./src/utils.cpp
# )
```
## 编写主程序/头文件
省略
## 编译执行
```bash
cd build 
cmake ..  #生成Makefile
make   #编译
```
如果一切正常, bin目录下就会多出一个chess可执行文件，并执行
```bash
cd ..
./bin/chess
```
如果出错也没关系, 可以取消掉 CMakeLists.txt 的 # set(CMAKE_VERBOSE_MAKEFILE ON) 注释, 并运行:
```
cd build
cmake ..>cmake.out
```
就可以打开 build 目录下的 cmake.out 查看调试信息
## 参考
1. https://zhuanlan.zhihu.com/p/45528705