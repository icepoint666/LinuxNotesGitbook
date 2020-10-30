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

### 单文件使用CMake编译示例

编写一个`main.cc`文件 

编写一个`CMakeList.txt`文件

saaasasas

```bash
cmake_minimum_required (VERSION 2.8) #指定最低cmake支持版本

# 项目信息
project (Demo1)  # 项目名称

# 指定生成目标
add_executable(Demo main.cc)   # 指定生成目标文件为Demo
```



