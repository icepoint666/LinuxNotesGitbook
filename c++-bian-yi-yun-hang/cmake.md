# CMake

**参考链接：**

**VSCode:**

* gcc on windows**:** [**https://code.visualstudio.com/docs/cpp/config-mingw**](https://code.visualstudio.com/docs/cpp/config-mingw)
* cmake on Linux: [https://code.visualstudio.com/docs/cpp/cmake-linux](https://code.visualstudio.com/docs/cpp/cmake-linux)

**主要介绍Linux环境下CMake使用**

### 验证环境是否安装

```bash
$ gcc -v
$ make -v
$ cmake --help
```

### 使用CMake编译示例（单目录）

**1.单个目录下，单个源文件**

编写一个`main.cc`文件 

编写一个`CMakeList.txt`文件

```bash
cmake_minimum_required (VERSION 2.8) #指定最低cmake支持版本

project (Demo1)  # 项目名称

add_executable(Demo main.cc)   # 指定生成目标文件为Demo
```

接下来

```bash
$ mkdir build
$ cd build
$ cmake ..
$ make
($ sudo make install可能有时不需要)
```

**2.单个目录下，多个源文件**

```bash
./Demo2
    |
    +--- main.cc
    |
    +--- MathFunctions.cc
    |
    +--- MathFunctions.h

```

```bash
cmake_minimum_required (VERSION 2.8)

project (Demo2)

add_executable(Demo main.cc MathFunctions.cc)
```

**aux\_source\_directory命令**

如果源文件很多，把所有源文件的名字都加进去将是一件烦人的工作。更省事的方法是使用 `aux_source_directory` 命令，该命令会查找指定目录下的所有源文件，然后将结果存进指定变量名

```bash
cmake_minimum_required (VERSION 2.8)

project (Demo2)

# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)

add_executable(Demo ${DIR_SRCS})
```

### 使用CMake编译示例（多目录，重点）

**目录结构**

```bash
./Demo3
    |
    +--- CMakeLists.txt
    |
    +--- main.cc
    |
    +--- math/
          |
          +--- MathFunctions.cc
          |
          +--- MathFunctions.h
```

CMakeLists.**txt**

