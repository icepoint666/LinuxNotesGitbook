# coredump——崩溃快照

详情了解：linux coredump调试 - 飞翔的猪的文章 - 知乎 [https://zhuanlan.zhihu.com/p/98700797](https://zhuanlan.zhihu.com/p/98700797)

Coredump叫做核心转储，它是进程运行时在**突然崩溃的那一刻**的一个**内存快照**

操作系统在程序发生异常而异常在进程内部又没有被捕获的情况下，会把进程此刻内存、寄存器状态、运行堆栈等信息转储保存在一个文件里。

该文件也是二进制文件，可以使用gdb、elfdump、objdump等打开



