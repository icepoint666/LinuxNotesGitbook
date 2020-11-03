# gcc/g++基本使用, 其它选项

### **编译文件**

```bash
$ g++ main.cpp -o a.out # 导出可执行文件名称为a.out
$ g++ main.cpp -g  # 输出可以调试的信息
$ g++ --std=c11 main.cpp # c++11编译
$ g++ --std=c11 main.cpp -g -o a.out
```

同时有Class.cpp Class.h main.cpp，其中main.cpp包含了"Class.h"头文件，需要一起编译

不然会报错：

```bash
/usr/lib/gcc/x86_64-linux-gnu/7/../../../x86_64-linux-gnu/Scrt1.o: In function `_start':
(.text+0x20): undefined reference to `main'
collect2: error: ld returned 1 exit status
```

编译方法：

```bash
$ g++ main.cpp Class.cpp -o a.out
```

### g++指令其他选项

**link** 

**-l**（小写） 用来**指定程序要链接的库**，**-l**参数紧接着就是库名

**-L**（大写） **跟着的是库文件所在的目录名**。比如我们把libtest.so放在/aaa/bbb/ccc目录下，那链接参数就是**-L** /aaa/bbb/ccc

 **放在/lib和/usr/lib和/usr/local/lib里的库直接用-l参数就能链接了**

**include**

**-i** \(小写\) 用来指定程序要寻找的头文件

**-I** \(大写i\) 指定为第一个寻找头文件的目录

 **gcc -o hello hello.c -I /home/hello/include -L /home/hello/lib -lworld**

上面这句表示在编译hello.c时：

-I /home/hello/include表示将/home/hello/include目录作为第一个寻找头文件的目录

寻找的顺序是：**/home/hello/include--&gt;/usr/include--&gt;/usr/local/include** 

**define \(增加宏定义）**

\*\*\*\*

