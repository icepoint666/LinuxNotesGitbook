# gcc/g++链接 静态库 动态库

### 静态库

 `linux`环境下是以`.a`结尾的，而`windows`环境下是以`.lib`结尾的

特点：可执行文件中包含了库代码的一份完整拷贝

显然静态链接的缺点就是：

* ①冗余拷贝
* ②而且如果一个lib更新了，还需要重新去静态链接整个项目（编译时链接）

### 创建静态库

 写源文件

```bash
aLibTest.h aLibTest.cpp main.cpp
```

通过 `g++ -c xxx.cpp` 生成目标文件 先编译生成`.o`链接文件

```bash
$ g++ aLibTest.cpp -c 
$ ls
aLibTest.cpp  aLibTest.h  aLibTest.o  main.cpp
```

归档，生成静态库

```bash
$ ar cr libaLibTest.a aLibTest.o
$ ls
aLibTest.cpp  aLibTest.h  aLibTest.o  libaLibTest.a  main.cpp
```

链接

```bash
$ g++ main.cpp -L. -laLibTest
$ ./a.out 
```

### 动态库

 `Linux`下动态库文件的文件名形如 `libxxx.so`

使用动态库的优点是系统只需载入一次动态库，不同的程序可以得到内存中相同的动态库的副本

优点：节省了内存

### 创建动态库

编写源文件

将一个或几个源文件编译链接，生成共享库， `-fPIC` 创建与地址无关的编译程序（pic，position independent code），是为了能够在多个应用程序间共享。

```bash
$ g++ -fPIC -c aLibTest.cpp 
$ ls
aLibTest.cpp  aLibTest.h  aLibTest.o  main.cpp
```

生成动态库（链接器选项-shared）

```bash
$ g++ -shared -o libaLibTest.so aLibTest.o
$ ls
aLibTest.cpp  aLibTest.h  aLibTest.o  libaLibTest.so  main.cpp
```

直接运行./main还不行，如何让系统能够找到它：

* 编辑/etc/ld.so.conf文件，加入库文件所在目录的路径
* 运行ldconfig ，该命令会重建/etc/ld.so.cache文件
* 如果安装在/lib或者/usr/lib下，那么ld默认能够找到，无需其他操作。
* 如果安装在其他目录，需要将其添加到/etc/ld.so.cache文件中，步骤如下：
  * 当系统加载可执行代码时候，能够知道其所依赖的库的名字，但是还需要知道绝对路径。此时就需要系统动态载入器\(dynamic linker/loader\)。
  * 对于elf格式的可执行程序，是由ld-linux.so\*来完成的，它先后搜索elf文件的 DT\_RPATH段—环境变量LD\_LIBRARY\_PATH—/etc/ld.so.cache文件列表—/lib/,/usr/lib 目录找到库文件后将其载入内存。

我们直接将我们生成的文件拷贝至`/usr/lib`下，运行./main即可

