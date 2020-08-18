# 资源监控

### **通用监控** 

**htop**

最流行的工具要数 [`htop`](https://hisham.hm/htop/index.php),了，它是 [`top`](http://man7.org/linux/man-pages/man1/top.1.html)的改进版。`htop` 可以显示当前运行进程的多种统计信息。`htop` 有很多选项和快捷键，常见的有：`<F6>` 进程排序、 `t` 显示树状结构和 `h` 打开或折叠线程。 还可以留意一下 [`glances`](https://nicolargo.github.io/glances/) ，它的实现类似但是用户界面更好。如果需要合并测量全部的进程

**dstat**

 [`dstat`](http://dag.wiee.rs/home-made/dstat/) 是也是一个非常好用的工具，它可以实时地计算不同子系统资源的度量数据，例如 I/O、网络、 CPU 利用率、上下文切换等等；

### **I/O 操作** 

**iotop**

 [`iotop`](http://man7.org/linux/man-pages/man8/iotop.8.html) 可以显示实时 I/O 占用信息而且可以非常方便地检查某个进程是否正在执行大量的磁盘读写操作；

### 磁盘

 [`df`](http://man7.org/linux/man-pages/man1/df.1.html) 可以显示每个分区的信息，而 [`du`](http://man7.org/linux/man-pages/man1/du.1.html) 则可以显示当前目录下每个文件的磁盘使用情况（ **d**isk **u**sage）。`-h` 选项可以使命令使用对人类（human）更加友好的格式显示数据；[`ncdu`](https://dev.yorhel.nl/ncdu)是一个交互性更好的 `du` ，它可以让您在不同目录下导航、删除文件和文件夹；

disk usage -h表示 human readable output

```bash
$ du -h videos/
```

 [`ncdu`](https://dev.yorhel.nl/ncdu)是一个交互性更好interactive version的 `du` ，它可以让您在不同目录下导航、删除文件和文件夹；

```bash
$ ncdu
```

### **内存使用** 

**free**

 [`free`](http://man7.org/linux/man-pages/man1/free.1.html) 可以显示系统当前空闲的内存。内存也可以使用 `htop` 这样的工具来显示；

### **lsof命令——打开文件/查看端口监听占用进程**

 [`lsof`](http://man7.org/linux/man-pages/man8/lsof.8.html) 可以列出被进程打开的文件信息。 当我们需要查看某个文件是被哪个进程打开的时候，这个命令非常有用；

**在linux环境下，任何事物都以文件的形式存在**，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。所以如传输控制协议 \(TCP\) 和用户数据报协议 \(UDP\) 套接字等

```bash
$ sudo lsof | grep ":4444 .LISTEN" # 查看监听4444进程的进程信息
```

我们经常会遇到的情况是某个我们希望去监听的端口已经被其他进程占用了。让我们通过进程的PID查找相应的进程。首先执行 `python -m http.server 4444` 启动一个最简单的 web 服务器来监听 `4444` 端口。在另外一个终端中，执行 `lsof | grep LISTEN` 打印出所有监听端口的进程及相应的端口。找到对应的 PID 然后使用 `kill <PID>` 停止该进程。

### **网络连接和配置** 

 [`ss`](http://man7.org/linux/man-pages/man8/ss.8.html) 帮助我们监控网络包的收发情况以及网络接口的显示信息。`ss` 常见的一个使用场景是找到端口被进程占用的信息。如果要显示路由、网络设备和接口信息，您可以使用 [`ip`](http://man7.org/linux/man-pages/man8/ip.8.html) 命令。注意，`netstat` 和 `ifconfig` 这两个命令已经被前面那些工具所代替了。

### **网络使用** 

 [`nethogs`](https://github.com/raboof/nethogs) 和 [`iftop`](http://www.ex-parrot.com/pdw/iftop/) 是非常好的用于对网络占用进行监控的交互式命令行工具。

