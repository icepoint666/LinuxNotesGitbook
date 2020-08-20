# 硬件相关命令

**系统**

```bash
$ uname -a               # 查看内核/操作系统/CPU信息
$ head -n 1 /etc/issue   # 查看操作系统版本
$ cat /proc/cpuinfo      # 查看CPU信息
$ hostname               # 查看计算机名
$ lspci -tv              # 列出所有PCI设备（yum install pciutils）
$ lsusb -tv              # 列出所有USB设备
$ lsmod                  # 列出加载的内核模块
$ env                    # 查看环境变量  
```

**资源**

```bash
$ free -m                # 查看内存使用量和交换区使用量
$ df -h                  # 查看各分区使用情况
$ du -sh <目录名>         # 查看指定目录的大小
$ du -h                  # 查看当前目录的大小
$ grep MemTotal /proc/meminfo   # 查看内存总量
$ grep MemFree /proc/meminfo    # 查看空闲内存量
$ uptime                 # 查看系统运行时间、用户数、负载
$ cat /proc/loadavg      # 查看系统负载
```

**磁盘和分区**

```bash
$ mount | column -t      # 查看挂接的分区状态
$ fdisk -l               # 查看所有分区
$ swapon -s              # 查看所有交换分区
$ hdparm -i /dev/hda     # 查看磁盘参数(仅适用于IDE设备)
$ dmesg | grep IDE       # 查看启动时IDE设备检测状况
```

**查看网卡型号**

```bash
$ lspci | grep Ethernet  
```

**查看内存和cpu最直接最常用的命令**

```bash
$ free
$ cat /proc/cpuinfo   
```

**查看硬盘信息（查看硬盘型号和硬盘序列号）**

```bash
$ df -h  
```

