# CMakeLists.txt最关键的部分

### **一个CMakeLists最关键的部分**

```bash
cmake_minimum_required (VERSION 3.0)

project(hello)                                  # 给工程起个名字

include_directories(${CMAKE_CURRENT_LIST_DIR}/include)
                                                # 指定头文件所在目录，让CMake找到我的头文件 .h
link_directories(${CMAKE_CURRENT_LIST_DIR}/lib) # 指定链接库文件所在目录，让CMake找到我的库文件 .lib
aux_source_directory(${CMAKE_CURRENT_LIST_DIR}/src ${hello_src})
                                                # 指定要编译的源文件所在目录，让CMake找到我的源文件 .cc
add_executable(${PROJECT_NAME} ${hello_src})
                                                # 告诉CMake我的构建目标
target_link_libraries(${PROJECT_NAME} util)
                                                # 指定要链接哪个具体的库文件（不同于前面目录，这里是文件名）
set(CMAKE_CXX_COMPILER      "clang++" )         # 显示指定使用的C++编译器

set(CMAKE_CXX_FLAGS   "-std=c++11")             # c++11
set(CMAKE_CXX_FLAGS   "-g")                     # 调试信息
set(CMAKE_CXX_FLAGS   "-Wall")                  # 开启所有警告

set(CMAKE_CXX_FLAGS_DEBUG   "-O0" )             # 调试包不优化
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -DNDEBUG " )   # release包优化
```

在CMakeLists.txt所在目录，新建build目录，并切换进build进行构建即可. 具体构建方法参见上一篇CMake Hello World的构建。

注意：生成的可执行文件路径会在build/src目录下，如需修改生成位置，请参考CMake变量`EXECUTABLE_OUTPUT_PATH`

