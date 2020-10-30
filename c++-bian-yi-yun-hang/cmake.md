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

### CMake添加环境检查

**环境检查**

例要使用一个平台相关的特性的时候。在这个例子中，我们检查系统是否自带 pow 函数。如果带有 pow 函数，就使用它；否则使用我们定义的 power 函数。

**依靠CMake的一些内置宏实现**

```bash
cmake_minimum_required (VERSION 2.8)

project (Demo4)

set (CMAKE_INCLUDE_CURRENT_DIR ON)

# 检查系统是否支持 pow 函数
include (${CMAKE_ROOT}/Modules/CheckFunctionExists.cmake)
check_function_exists (pow HAVE_POW)

# 加入一个配置头文件，用于处理 CMake 对源码的设置
configure_file (
  "${PROJECT_SOURCE_DIR}/config.h.in"
  "${PROJECT_BINARY_DIR}/config.h"
  )

# 是否加入 MathFunctions 库
if (NOT HAVE_POW)
  include_directories ("${PROJECT_SOURCE_DIR}/math")
  add_subdirectory (math)
  set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif (NOT HAVE_POW)

aux_source_directory(. DIR_SRCS)

# 指定生成目标
add_executable(Demo ${DIR_SRCS})
target_link_libraries (Demo  ${EXTRA_LIBS})
```

* **project**的作用： 定义工程名称, 设置几个变量的名字: `PROJECT_NAME, PROJECT_SOURCE_DIR, <PROJECT-NAME>_SOURCE_DIR, PROJECT_BINARY_DIR, <PROJECT-NAME>_BINARY_DIR`
* **include\_directories**的作用：把当前目录\(CMakeLists.txt所在目录\)下的某个目录加入到包含路径

  使用之后，main.cc调用MainFunctions的时候就不需要再`#include "math/MathFunctions.h"`了

  直接`#include "MathFunctions.h`"即可

* **configure\_file** 命令用于加入一个配置头文件 config.h ，这个文件由 CMake 从 [config.h.in](http://config.h.in/) 生成，通过这样的机制，将可以通过预定义一些参数和变量来控制代码的生成
* **set \(CMAKE\_INCLUDE\_CURRENT\_DIR ON\)**  保证生成的build中的头文件，例如config.h能被根目录include到，所以main.cc才能include到config.h

**编写 config.h.in 文件,在CMake根目录**

```bash
// does the platform provide pow function?
#cmakedefine HAVE_POW
```

**目录情况**

![](../.gitbook/assets/wu-biao-ti-%20%281%29.png)

**在代码中使用宏和函数，修改main.cc**

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <config.h>

#ifdef HAVE_POW
  #include <math.h>
#else
  #include <MathFunctions.h>
#endif

int main(int argc, char *argv[])
{
    if (argc < 3){
        printf("Usage: %s base exponent \n", argv[0]);
        return 1;
    }
    double base = atof(argv[1]);
    int exponent = atoi(argv[2]);
    
#ifdef HAVE_POW
    printf("Now we use the standard library. \n");
    double result = pow(base, exponent);
#else
    printf("Now we use our own Math library. \n");
    double result = power(base, exponent);
#endif
    
    printf("%g ^ %d is %g\n", base, exponent, result);
    return 0;
}
```

### CMake安装和测试，生成安装包\(打包\)

* make install 指定安装规则
* make test 添加测试

**定制安装规则**

install主要的含义就是把生成的头文件放到系统/usr/include目录，把库/目标文件放在/usr/bin中

之后执行的时候或者需要动态链接的时候，可以自动从系统目录中寻找

首先先在 math/CMakeLists.txt 文件里添加下面两行：

```bash
# 指定 MathFunctions 库的安装路径
install (TARGETS MathFunctions DESTINATION bin)
install (FILES MathFunctions.h DESTINATION include)
```

指明 MathFunctions 库的安装路径。之后同样修改根目录的 CMakeLists 文件，在末尾添加下面几行：

```bash
# 指定安装路径
install (TARGETS Demo DESTINATION bin)
install (FILES "${PROJECT_BINARY_DIR}/config.h"
         DESTINATION include)
```

* 通过上面的定制，生成的 Demo 文件和 MathFunctions 函数库 libMathFunctions.o 文件将会被复制到 `/usr/local/bin` 中
* 而 MathFunctions.h 和生成的 config.h 文件则会被复制到 `/usr/local/include` 中
* 这里的 `/usr/local/` 是默认安装到的根目录，可以通过修改 `CMAKE_INSTALL_PREFIX` 变量的值来指定这些文件应该拷贝到哪个根目录

```bash
[ehome@xman Demo5]$ sudo make install
[ 50%] Built target MathFunctions
[100%] Built target Demo
Install the project...
-- Install configuration: ""
-- Installing: /usr/local/bin/Demo
-- Installing: /usr/local/include/config.h
-- Installing: /usr/local/bin/libMathFunctions.a
-- Up-to-date: /usr/local/include/MathFunctions.h
[ehome@xman Demo5]$ ls /usr/local/bin
Demo  libMathFunctions.a
[ehome@xman Demo5]$ ls /usr/local/include
config.h  MathFunctions.h
```

#### 为工程添加测试 <a id="&#x4E3A;&#x5DE5;&#x7A0B;&#x6DFB;&#x52A0;&#x6D4B;&#x8BD5;"></a>

 CMake 提供了一个称为 CTest 的测试工具。我们要做的只是在项目根目录的 CMakeLists 文件中调用一系列的 `add_test` 命令

```text
# 启用测试
enable_testing()

# 测试程序是否成功运行
add_test (test_run Demo 5 2)

# 测试帮助信息是否可以正常提示
add_test (test_usage Demo)
set_tests_properties (test_usage
  PROPERTIES PASS_REGULAR_EXPRESSION "Usage: .* base exponent")

# 测试 5 的平方
add_test (test_5_2 Demo 5 2)

set_tests_properties (test_5_2
 PROPERTIES PASS_REGULAR_EXPRESSION "is 25")

# 测试 10 的 5 次方
add_test (test_10_5 Demo 10 5)

set_tests_properties (test_10_5
 PROPERTIES PASS_REGULAR_EXPRESSION "is 100000")

# 测试 2 的 10 次方
add_test (test_2_10 Demo 2 10)

set_tests_properties (test_2_10
 PROPERTIES PASS_REGULAR_EXPRESSION "is 1024")
```

 上面的代码包含了四个测试。第一个测试 `test_run` 用来测试程序是否成功运行并返回 0 值。剩下的三个测试分别用来测试 5 的 平方、10 的 5 次方、2 的 10 次方是否都能得到正确的结果。其中 `PASS_REGULAR_EXPRESSION` 用来测试输出是否包含后面跟着的字符串。

看看测试的结果：

```bash
[ehome@xman Demo5]$ make test
Running tests...
Test project /home/ehome/Documents/programming/C/power/Demo5
    Start 1: test_run
1/4 Test #1: test_run .........................   Passed    0.00 sec
    Start 2: test_5_2
2/4 Test #2: test_5_2 .........................   Passed    0.00 sec
    Start 3: test_10_5
3/4 Test #3: test_10_5 ........................   Passed    0.00 sec
    Start 4: test_2_10
4/4 Test #4: test_2_10 ........................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 4

Total Test time (real) =   0.01 sec
```

如果要测试更多的输入数据，像上面那样一个个写测试用例未免太繁琐。这时可以通过编写宏来实现：

```bash
# 定义一个宏，用来简化测试工作
macro (do_test arg1 arg2 result)
  add_test (test_${arg1}_${arg2} Demo ${arg1} ${arg2})
  set_tests_properties (test_${arg1}_${arg2}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result})
endmacro (do_test)
 
# 使用该宏进行一系列的数据测试
do_test (5 2 "is 25")
do_test (10 5 "is 100000")
do_test (2 10 "is 1024")
```

**生成安装包**

CPack是由 CMake 提供的一个工具，专门用于打包

在顶层的 CMakeLists.txt 文件尾部添加下面几行：

```bash
# 构建一个 CPack 安装包
include (InstallRequiredSystemLibraries)
set (CPACK_RESOURCE_FILE_LICENSE
  "${CMAKE_CURRENT_SOURCE_DIR}/License.txt")
set (CPACK_PACKAGE_VERSION_MAJOR "${Demo_VERSION_MAJOR}")
set (CPACK_PACKAGE_VERSION_MINOR "${Demo_VERSION_MINOR}")
include (CPack)
```

上面的代码做了以下几个工作：

1. 导入 InstallRequiredSystemLibraries 模块，以便之后导入 CPack 模块；
2. 设置一些 CPack 相关变量，包括版权信息和版本信息，其中版本信息用了上一节定义的版本号；
3. 导入 CPack 模块。

 像往常一样构建工程，并执行 `cpack` 命令

### CMake支持gdb调试

类似于g++时编译需要-g

```cpp
set(CMAKE_BUILD_TYPE "Debug")
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
```

### 交互式CMake

详见：[https://www.hahack.com/codes/cmake/\#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BC%96%E8%AF%91%E9%80%89%E9%A1%B9](https://www.hahack.com/codes/cmake/#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BC%96%E8%AF%91%E9%80%89%E9%A1%B9)

### 一个比较完整的CMakeLists.txt

源码：[https://github.com/wzpan/cmake-demo/blob/master/Demo8](https://github.com/wzpan/cmake-demo/blob/master/Demo8)

C**MakeLists.txt**

```bash
cmake_minimum_required (VERSION 2.8)

# 项目信息
project (Demo8)

# 设置版本号，可以在代码中输出版本号
set (Demo_VERSION_MAJOR 1)
set (Demo_VERSION_MINOR 0)

# 设置当前目录，一般也就是build目录可以被cmake根目录include到
set (CMAKE_INCLUDE_CURRENT_DIR ON)

# 检查系统是否支持 pow 函数
include (${CMAKE_ROOT}/Modules/CheckFunctionExists.cmake)
check_function_exists (pow HAVE_POW)

# 加入一个配置头文件，用于处理 CMake 对源码的设置
configure_file (
  "${PROJECT_SOURCE_DIR}/config.h.in"
  "${PROJECT_BINARY_DIR}/config.h"
  )

# 是否加入 MathFunctions 库
if (NOT HAVE_POW)
  include_directories ("${PROJECT_SOURCE_DIR}/math")
  add_subdirectory (math)  
  set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif (NOT HAVE_POW)

# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)

# 指定生成目标
add_executable(Demo ${DIR_SRCS})
target_link_libraries (Demo  ${EXTRA_LIBS})

# 传递FLAGS给C++编译器
set(CMAKE_CXX_COMPILER      "g++" )             # 显示指定使用的C++编译器

set(CMAKE_CXX_FLAGS   "-std=c++11")             # c++11
set(CMAKE_CXX_FLAGS   "-g")                     # 调试信息
set(CMAKE_CXX_FLAGS   "-Wall")                  # 开启所有警告

set(CMAKE_CXX_FLAGS_DEBUG   "-O0" )             # 调试包不优化
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -DNDEBUG " )   # release包优化

# 指定安装路径
install (TARGETS Demo DESTINATION bin)
install (FILES "${PROJECT_BINARY_DIR}/config.h"
         DESTINATION include)

# 启用测试
enable_testing()

# 测试程序是否成功运行
add_test (test_run Demo 5 2)

# 测试帮助信息是否可以正常提示
add_test (test_usage Demo)
set_tests_properties (test_usage
  PROPERTIES PASS_REGULAR_EXPRESSION "Usage: .* base exponent")

# 定义一个宏，用来简化测试工作
macro (do_test arg1 arg2 result)
  add_test (test_${arg1}_${arg2} Demo ${arg1} ${arg2})
  set_tests_properties (test_${arg1}_${arg2}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result})
endmacro (do_test)
 
# 利用 do_test 宏，测试一系列数据
do_test (5 2 "is 25")
do_test (10 5 "is 100000")
do_test (2 10 "is 1024")

# 构建一个 CPack 安装包
include (InstallRequiredSystemLibraries)
set (CPACK_RESOURCE_FILE_LICENSE
  "${CMAKE_CURRENT_SOURCE_DIR}/License.txt")
set (CPACK_PACKAGE_VERSION_MAJOR "${Demo_VERSION_MAJOR}")
set (CPACK_PACKAGE_VERSION_MINOR "${Demo_VERSION_MINOR}")
include (CPack)
```

### **一个CMakeLists最关键的部分**

```bash
cmake_minimum_required (VERSION 3.0)

project(hello)                                  # 给工程起个名字

include_directories(${CMAKE_CURRENT_LIST_DIR}/include)
                                                # 让CMake找到我的头文件 .h
link_directories(${CMAKE_CURRENT_LIST_DIR}/lib)
                                                # 让CMake找到我的库文件 .lib
aux_source_directory(${CMAKE_CURRENT_LIST_DIR}/src ${hello_src})
                                                # 让CMake找到我的源文件 .cc
add_executable(${PROJECT_NAME} ${hello_src})
                                                # 告诉CMake我的构建目标
target_link_libraries(${PROJECT_NAME} util)
                                                # 告诉CMake我要链接哪个库文件
set(CMAKE_CXX_COMPILER      "clang++" )         # 显示指定使用的C++编译器

set(CMAKE_CXX_FLAGS   "-std=c++11")             # c++11
set(CMAKE_CXX_FLAGS   "-g")                     # 调试信息
set(CMAKE_CXX_FLAGS   "-Wall")                  # 开启所有警告

set(CMAKE_CXX_FLAGS_DEBUG   "-O0" )             # 调试包不优化
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -DNDEBUG " )   # release包优化
```

在CMakeLists.txt所在目录，新建build目录，并切换进build进行构建即可. 具体构建方法参见上一篇CMake Hello World的构建。

注意：生成的可执行文件路径会在build/src目录下，如需修改生成位置，请参考CMake变量`EXECUTABLE_OUTPUT_PATH`

