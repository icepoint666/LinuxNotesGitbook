# CMake基本使用

**参考链接：**

**VSCode:**

* gcc on windows**:** [**https://code.visualstudio.com/docs/cpp/config-mingw**](https://code.visualstudio.com/docs/cpp/config-mingw)
* cmake on Linux: [https://code.visualstudio.com/docs/cpp/cmake-linux](https://code.visualstudio.com/docs/cpp/cmake-linux)

**主要介绍Linux环境下CMake使用**

* 参考链接：[https://www.hahack.com/codes/cmake/](https://www.hahack.com/codes/cmake/)

### 验证gcc, make, cmake环境是否安装

```bash
$ gcc -v
$ make -v
$ cmake --help
```

### CMake编译示例（单目录）

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

### CMake编译示例（多目录，重点）

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

**源码** 

```cpp
// main.cc
#include <stdio.h>
#include <stdlib.h>
#include "math/MathFunctions.h" //注意：一定是math/MathFunctions.h，代表相对于main.cc的位置，而不是链接库的相对位置

int main(int argc, char *argv[])
{
    if (argc < 3){
        printf("Usage: %s base exponent \n", argv[0]);
        return 1;
    }
    double base = atof(argv[1]);
    int exponent = atoi(argv[2]);
    double result = power(base, exponent);
    printf("%g ^ %d is %g\n", base, exponent, result);
    return 0;
}

// math/MathFunctions.cc
double power(double base, int exponent)
{
    int result = base;
    int i;

    if (exponent == 0) {
        return 1;
    }

    for(i = 1; i < exponent; ++i){
        result = result * base;
    }

    return result;
}
// math/MathFunctions.h
#ifndef MATHFUNCTIONS_H
#define MATHFUNCTIONS_H

extern double power(double base, int exponent); //注意是extern

#endif

```

**CMakeLists.txt有两种写法**

**写法1：不太推荐**

```bash
cmake_minimum_required (VERSION 2.8)

project (Demo3)

aux_source_directory(. DIR_SRCS)
aux_source_directory(math DIR_MATH)
add_library(MathFunctions ${DIR_MATH})

add_executable(Demo main.cc)
target_link_libraries(Demo MathFunctions)

```

* **aux\_source\_directory** 相当于把目录用一个变量名表示出来
* **add\_library** 会把指定目录所有源文件都编译得到目标文件\(静态链接库，LibMathFunctions.a\)，存放在build目录中
* **target\_link\_libraries** 会把指定的目标链接库，自动在整个build的目录中寻找对应名字的.a，然后静态**链接到可执行文件上**

**这种写法会使得math目录中生成文件都生成到build目录中，如果子目录的内容比较多的话，感觉会比较乱，因为生成的东西都混合在math目录中**

**写法2：推荐，用add\_subdirectory**

```bash
cmake_minimum_required (VERSION 2.8)

project (Demo3)

aux_source_directory(. DIR_SRCS)

add_subdirectory(math)

add_executable(Demo main.cc)
target_link_libraries(Demo MathFunctions)

```

* **add\_subdirectory 会自动从指定的目录，也就是到math/目录下执行里面的CMakeLists.txt**

**math目录下的CMakeLists.txt**

```bash
aux_source_directory(. DIR_LIB_SRCS)

add_library (MathFunctions ${DIR_LIB_SRCS})
```

会自动在build文件夹创建一个math子目录，当调用add\_subdirectory里面的CMakeLists.txt执行的生成的东西会保存在math文件夹中，整体会比较有层次感，与本身项目目录一样，**具有层次感**

**生成目录：**

```bash
(base) liangyu@dolphin:~/cc/cmakeTest2/build$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  Demo  Makefile  math

(base) liangyu@dolphin:~/cc/cmakeTest2/build/math$ ls
CMakeFiles  cmake_install.cmake  libMathFunctions.a  Makefile
```

### CMake添加环境检查，版本号

环境检查

### CMake安装和测试，生成安装包\(打包\)

* make install 指定安装规则
* make test 添加测试

### CMake支持gdb调试

### 交互式CMake

详见：[https://www.hahack.com/codes/cmake/\#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BC%96%E8%AF%91%E9%80%89%E9%A1%B9](https://www.hahack.com/codes/cmake/#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BC%96%E8%AF%91%E9%80%89%E9%A1%B9)

