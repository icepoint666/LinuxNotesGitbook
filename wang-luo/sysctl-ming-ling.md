# sysctl命令

**Linux sysctl 修改默认的系统网络参数**

**目的：为了提升**ipv4,ipv6的传输性能

#### 1. Disable the TCP timestamps option for better CPU utilization:

```bash
$ sysctl -w net.ipv4.tcp_timestamps=0
```

#### 2. Enable the TCP selective acks \(SACK\) option for better throughput:

```bash
$ sysctl -w net.ipv4.tcp_sack=1
```

#### 3. Increase the maximum length of processor input queues:

```bash
$ sysctl -w net.core.netdev_max_backlog=250000
```

#### 4. Increase the TCP maximum and default buffer sizes using **`setsockopt()`**

```bash
$ sysctl -w net.core.rmem_max=4194304
$ sysctl -w net.core.wmem_max=4194304
$ sysctl -w net.core.rmem_default=4194304
$ sysctl -w net.core.wmem_default=4194304
$ sysctl -w net.core.optmem_max=4194304
```

#### 5. Increase memory thresholds to prevent packet dropping:

```bash
$ sysctl -w net.ipv4.tcp_rmem="4096 87380 4194304"
$ sysctl -w net.ipv4.tcp_wmem="4096 65536 4194304"
```

#### 6. Enable low latency mode for TCP:

```bash
$ sysctl -w net.ipv4.tcp_low_latency=1 
```

