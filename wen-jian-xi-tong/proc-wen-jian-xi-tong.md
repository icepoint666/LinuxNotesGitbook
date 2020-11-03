# /proc文件系统

/proc文件系统是一个伪的文件系统，就是说它是一个**实际上不存在的目录**，因而这是一个非常特殊的目录。它**并不存在于某个磁盘上，而是由核心在内存中产生**。这个目录用于**提供关于系统的信息**。

**更详细的/proc官方文档**：[https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/proc.html](https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/proc.html)

1. /proc/x：关于进程x的信息目录，这x是这一进程的标识号。每个进程在 /proc下有一个名为自己进程号的目录。

2. **/proc/cpuinfo**：存放处理器\(cpu\)的信息，如cpu的类型、制造商、 型号和性能等。

3. /proc/devices：当前运行的核心配置的设备驱动的列表。

4. /proc/dma：显示当前使用的dma通道。

5. /proc/filesystems：核心配置的文件系统信息。

6. /proc/interrupts：显示被占用的中断信息和占用者的信息，以及被占用 的数量。

7. /proc/ioports：当前使用的i/o端口。

8. /proc/kcore：系统物理内存映像。与物理内存大小完全一样，然而实际上没有 占用这么多内存；它仅仅是在程序访问它时才被创建。\(注意：除非你把它拷贝到什么地方，否则/proc下没有任何东西占用任何磁盘空间。\)

9. /proc/kmsg：核心输出的消息。也会被送到syslog。

10. /proc/ksyms：核心符号表。

11. /proc/loadavg：系统“平均负载”；3个没有意义的指示器指出系统当前 的工作量。

12. **/proc/meminfo**：各种存储器使用信息，包括物理内存和交换分区 \(swap\)。

13. /proc/modules：存放当前加载了哪些核心模块信息。

14. /proc/net：网络协议状态信息。

15. /proc/self：存放到查看/proc的 程序的进程目录的符号连接。当2个进程查看/proc时，这将会是不同的连接。这主要便于程序得到它自己的进程目录。

16. /proc/stat：系统的不同状态，例如，系统启动后页面发生错误的次数。

17. /proc/uptime：系统启动的时间长度。

18. /proc/version：核心版本。

