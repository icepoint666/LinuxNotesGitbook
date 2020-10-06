# g++基本使用

**编译文件**

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

