# Linux性能观察工具一览

![](../.gitbook/assets/1.jpg)

#### **首选vmstat!!**

### **内存监控命令**

**vmstat（很推荐！！显示的信息很全面！！不只是内存，包含CPU，IO）**

* 内存使用
* 虚拟内存交换情况
* IO读写情况
* CPU使用率

  $ vmstat 2 表示每两秒采集一次服务器状态，输出

  $ vmstat 就是直接显示一次采样的数据

```csharp
procs -----------memory----------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff   cache   si   so   bi    bo   in   cs  us sy id wa st
 1  0 382976 4720900 2183488 22734120 0   1   273     8    0    0  12  5 82  1  0
```

* **r** 运行队列线程数
* **b** 阻塞线程数
* **swpd** **free** **buff** **cache** 释义见：free命令——内存，缓存，swap虚拟内存
* **si**  每秒从磁盘读入虚拟内存的大小
* **so**  每秒虚拟内存写入磁盘的大小
* **bi**  块设备每秒接收的块数量，这里的块设备是指系统上所有的磁盘和其他块设备，默认块大小是1024byte，这里机器上显示的273/s，磁盘写入速度差不多273K/s
* **bo** 块设备每秒发送的块数量
* **in** 每秒CPU的中断次数，包括时间中断
* **cs** 每秒上下文切换次数，例如我们调用系统函数，就要进行上下文切换，线程的切换，也要进程上下文切换，这个值要越小越好，太大了，要考虑调低线程或者进程的数目
* **us** 用户CPU时间（百分比%），我曾经在一个做加密解密很频繁的服务器上，可以看到us接近100,r运行队列达到80\(机器在做压力测试，性能表现不佳\)。
* **sy** 系统CPU时间（百分比%），如果太高，表示系统调用时间长，例如是IO操作频繁。
* **id**  空闲 CPU时间，一般来说，id + us + sy = 100, id是空闲CPU使用率，us是用户CPU使用率，sy是系统CPU使用率。
* **wt** 等待IO CPU时间。

### **cpu监控命令**

**top**

* 显示任务进程整体情况
* 显示每个进程的情况
* 显示CPU使用情况
* 显示内存占用情况

![](../.gitbook/assets/wu-biao-ti-%20%285%29.png)

**pidstat**

* PID：进程ID

  %usr：进程在用户空间占用cpu的百分比

  %system：进程在内核空间占用cpu的百分比

  %guest：进程在虚拟机占用cpu的百分比

  %CPU：进程占用cpu的百分比

  CPU：处理进程的cpu编号

  Command：当前进程对应的命令

![](../.gitbook/assets/wu-biao-ti-%20%284%29.png)

**mpstat**

* MultiProcessor Statistics 

  报告CPU的一些统计信息，这些信息存放在/proc/stat文件中。在多CPUs系统里，其不但能查看所有CPU的平均状况信息，而且能够查看特定CPU的信息。

  * **-P {\|ALL}：**表示监控哪个CPU，在\[0,cpu个数-1\]中取值；
  * **internal：**相邻的两次采样的间隔时间；
  * **count：**采样的次数，count只能和delay一起使用；

* 显示所有的CPU信息：mpstat -P ALL 1（间隔为1s）

![](../.gitbook/assets/wu-biao-ti-%20%283%29.png)

* 显示ID为1的CPU信息：mpstat -P 1 1（间隔为1s）

### **IO监控命令**

**iotop**

**iotop命令**是一个用来监视磁盘I/O使用状况的[top](http://man.linuxde.net/top)类工具

**优点：可以统计到每个进程是如何使用IO的**（ [iostat](http://man.linuxde.net/iostat)，nmon等大多数是只能统计到每个设备的读写情况）

#### 选项

```text
-o：只显示有io操作的进程
-b：批量显示，无交互
-d SEC：间隔SEC秒显示一次。
-p PID：监控的进程pid（重要！！）
-u USER：监控的进程用户。
```

**示例：**

```text
$ iotop -p 28123
Total DISK READ :       0.00 B/s | Total DISK WRITE :       0.00 B/s
Actual DISK READ:       0.00 B/s | Actual DISK WRITE:       0.00 B/s
  TID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN     IO>    COMMAND
28123 be/4  uu         0.00 B/s    0.00 B/s  ?unavailable?  python train.py

```



