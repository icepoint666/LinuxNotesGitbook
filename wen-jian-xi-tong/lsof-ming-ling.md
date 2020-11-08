# lsof命令

#### lsof 一切皆文件：lsof（list open files）是一个查看当前系统文件的工具。在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。

**很强大！**

### **lsof**打开的文件可以是

1. 普通文件
2. 目录
3. 网络文件系统的文件
4. 字符或设备文件
5. \(函数\)共享库
6. 管道，命名管道
7. 符号链接
8. 网络文件（例如：NFS file、网络socket，unix域名socket）
9. 还有其它类型的文件

### **命令参数**

* -a 列出打开文件存在的进程
* -c&lt;进程名&gt; 列出指定进程所打开的文件
* -g 列出GID号进程详情
* -d&lt;文件号&gt; 列出占用该文件号的进程
* +d&lt;目录&gt; 列出目录下被打开的文件
* +D&lt;目录&gt; 递归列出目录下被打开的文件
* -n&lt;目录&gt; 列出使用NFS的文件
* -i&lt;条件&gt; 列出符合条件的进程。（4、6、协议、:端口、 @ip ）
* -p&lt;进程号&gt; 列出指定进程号所打开的文件
* -u 列出UID号进程详情
* -h 显示帮助信息
* -v 显示版本信息

### 最基本输出释义

#### 无任何参数输出释义

```bash
$lsof| more
COMMAND     PID      USER   FD      TYPE             DEVICE SIZE/OFF       NODE NAME
init          1      root  cwd       DIR              253,0     4096          2 /
init          1      root  rtd       DIR              253,0     4096          2 /
init          1      root  txt       REG              253,0   150352    1310795 /sbin/init
init          1      root  mem       REG              253,0    65928    5505054 /lib64/libnss_files-2.12.so
init          1      root  mem       REG              253,0  1918016    5521405 /lib64/libc-2.12.so
init          1      root  mem       REG              253,0    93224    5521440 /lib64/libgcc_s-4.4.6-20120305.so.1
init          1      root  mem       REG              253,0    47064    5521407 /lib64/librt-2.12.so
init          1      root  mem       REG              253,0   145720    5521406 /lib64/libpthread-2.12.so
...
```

说明：

**lsof输出各列信息的意义如下：**

* COMMAND：进程的名称
* PID：进程标识符
* PPID：父进程标识符（需要指定-R参数）
* USER：进程所有者
* PGID：进程所属组
* FD：文件描述符，应用程序通过文件描述符识别该文件。如cwd、txt等:
* TYPE：文件类型，如DIR、REG等，常见的文件类型:

  ```text
  （1）DIR：表示目录
  （2）CHR：表示字符类型
  （3）BLK：块设备类型
  （4）UNIX： UNIX 域套接字
  （5）FIFO：先进先出 (FIFO) 队列
  （6）IPv4：网际协议 (IP) 套接字
  ```

* DEVICE：指定磁盘的名称
* SIZE：文件的大小
* NODE：索引节点（文件在磁盘上的标识）
* NAME：打开文件的确切名称

### 实例1：查找某个文件相关的进程

```bash
$lsof /bin/bash
COMMAND     PID USER  FD   TYPE DEVICE SIZE/OFF    NODE NAME
mysqld_sa  2169 root txt    REG  253,0   938736 4587562 /bin/bash
ksmtuned   2334 root txt    REG  253,0   938736 4587562 /bin/bash
bash      20121 root txt    REG  253,0   938736 4587562 /bin/bash
```

### 实例2：列出某个用户打开的文件信息（很强大！）

```bash
$lsof -u username
```

### 实例3：列出某个程序进程所打开的文件信息

```bash
$lsof -c mysql
```

### 实例4：通过某个进程号显示该进程打开的文件

```bash
$lsof -p 11968
```

### 实例5：列出谁在使用某个端口

```c
root@gamma:/mnt/Lab/sds# lsof -i :22
COMMAND   PID    USER   FD   TYPE     DEVICE SIZE/OFF NODE NAME
sshd     1393    root    3u  IPv4  780833956      0t0  TCP *:ssh (LISTEN)
sshd     1393    root    4u  IPv6  780833958      0t0  TCP *:ssh (LISTEN)
sshd    23676    root    3u  IPv4 3094777270      0t0  TCP gamma:ssh->125.216.241.132:65072 (ESTABLISHED)
sshd    23757   sdsds    3u  IPv4 3094777270      0t0  TCP gamma:ssh->125.216.241.132:65072 (ESTABLISHED)
sshd    29167    root    3u  IPv4 3110800396      0t0  TCP gamma:ssh->125.216.241.78:58436 (ESTABLISHED)
sshd    29257      as    3u  IPv4 3110800396      0t0  TCP gamma:ssh->125.216.241.78:58436 (ESTABLISHED)
sshd    29837    root    3u  IPv4 3113496191      0t0  TCP gamma:ssh->125.216.249.142:51191 (ESTABLISHED)
sshd    29846    root    3u  IPv4 3113306646      0t0  TCP gamma:ssh->125.216.249.142:51196 (ESTABLISHED)
sshd    30010      me    3u  IPv4 3113496191      0t0  TCP gamma:ssh->125.216.249.142:51191 (ESTABLISHED)
sshd    30040      me    3u  IPv4 3113306646      0t0  TCP gamma:ssh->125.216.249.142:51196 (ESTABLISHED)

```

### 实例6：列出所有的网络连接

```text
$lsof -i
```

### 实例7：列出所有tcp 网络连接信息

```text
$lsof -i tcp

$lsof -n -i tcp
COMMAND     PID  USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
svnserve  11552 weber    3u  IPv4 3799399      0t0  TCP *:svn (LISTEN)
redis-ser 25501 weber    4u  IPv4  113150      0t0  TCP 127.0.0.1:6379 (LISTEN)
```

### 实例8：根据文件描述列出对应的文件信息

```bash
$lsof -d description(like 2)
```

示例:

```bash
$lsof -d 3 | grep PARSER1
tail      6499 tde    3r   REG    253,3   4514722     417798 /opt/applog/open/log/HOSTPARSER1_ERROR_141217.log.001
```

说明： 

* 0表示标准输入
* 1表示标准输出
* 2表示标准错误
* 从而可知：所以大多数应用程序所打开的文件的 FD 都是从 3 开始

