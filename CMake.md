[TOC]

### 

##来源

[ CMake 入门实战](http://www.hahack.com/codes/cmake/)

##笔记

文件名：CMakeLists.txt

- 单行注释 #
- 命令不区分大小写
- 

###单个源文件

```cmake
#CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

#项目信息   该项目名为 Demo1
project (Demo1)

#制定生成目标   将名为 main.cc 的源文件 编译为 Demo 的可执行文件
add_executable(Demo main.cc)
```

```shell
cmake .     # 在当前目录（包含 CMakeLists.txt的目录）
```

###同一目录多个源文件

```cmake
#CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

#项目信息   该项目名为 Demo2
project (Demo2)

#制定生成目标   将名为 main.cc MathFunctions.cc 的源文件 编译为 Demo 的可执行文件
add_executable(Demo main.cc MathFunctions.cc)
```

- 使用`aux_source_directory(<dir> <variable>) ` 命令，查找指定目录下的 所有源文件，将结果存到制定变量名

```cmake
#CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

#项目信息   该项目名为 Demo2
project (Demo2)
#查找当前目录下的所有源文件，并将结果保存的 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)

#制定生成目标
add_execualbe(Demo ${DIR_SRCS})
```