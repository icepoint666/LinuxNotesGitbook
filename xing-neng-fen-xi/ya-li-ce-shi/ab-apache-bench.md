# ab\(Apache Bench\)

官方链接：[https://www.tutorialspoint.com/apache\_bench/index.htm](https://www.tutorialspoint.com/apache_bench/index.htm)

是一个http server的压测工具

ab命令可以指定很多参数，来指定发送http请求的不同报头

 **-n**  即requests，用于指定压力测试总共的执行次数。

 **-c** 即concurrency，用于指定压力测试的并发数。

**-t** 即timelimit，等待响应的最大时间\(单位：秒\)。

 **-T** 即content-type，用于设置Content-Type请求头信息，例如：application/x-www-form-urlencoded，默认值为text/plain。

 **-i** 使用HEAD请求代替GET请求。

 **-C** 添加cookie信息，例如："Apache=1234"\(可以重复该参数选项以添加多个\)。

 **-H** 添加任意的请求头，例如："Accept-Encoding: gzip"，请求头将会添加在现有的多个请求头之后\(可以重复该参数选项以添加多个\)。

 **-k** 使用HTTP的KeepAlive特性

### 压测示例

```bash
$ ab -n 1000 -c 100 localhost/index.php
```

URL为**localhost/index.php**的页面进行压力测试。测试总次数为1000，并发数为100\(相当于100个用户同时访问，他们总共访问1000次\)

输出结果：

```bash
This is ApacheBench, Version 2.3 <$Revision: 655654 $>

Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/

Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)

Completed 100 requests

Completed 200 requests

Completed 300 requests

Completed 400 requests

Completed 500 requests

Completed 600 requests

Completed 700 requests

Completed 800 requests

Completed 900 requests

Completed 1000 requests

Finished 1000 requests

Server Software:        Apache/2.2.25 (服务器软件名称及版本信息)

Server Hostname:        localhost (服务器主机名)

Server Port:            80 (服务器端口)

Document Path:          /index.php (供测试的URL路径)

Document Length:        10 bytes (供测试的URL返回的文档大小)

Concurrency Level:      100 (并发数)

Time taken for tests:  0.247 seconds (压力测试消耗的总时间)

Complete requests:      1000 (压力测试的总次数)

Failed requests:        0 (失败的请求数)

Write errors:          0 (网络连接写入错误数)

Total transferred:      198000 bytes (传输的总数据量)

HTML transferred:      10000 bytes (HTML文档的总数据量)

Requests per second:    4048.34 [#/sec] (mean) (平均每秒的请求数)

Time per request:      24.701 [ms] (mean) (所有并发用户(这里是100)都请求一次的平均时间)

Time per request:      0.247 [ms] (mean, across all concurrent requests) (单个用户请求一次的平均时间)

Transfer rate:          782.78 [Kbytes/sec] received (传输速率，单位：KB/s)
```

### Epoll压测性能分析示例

来自于[https://github.com/yedf/handy/blob/master/raw-examples/epoll.cc](https://link.zhihu.com/?target=https%3A//github.com/yedf/handy/blob/master/raw-examples/epoll.cc)的代码

针对容易触发LT开关EPOLLOUT事件的情景（让服务器返回1M大小的数据，字节长度1048576，返回了一个这么长的字符串）

测试的结果显示ET的性能稍好，详情如下：

LT 启动命令

```bash
$ g++ epoll.cc -o epoll
$ ./epoll a
```

ET启动命令 

```bash
$ g++ epoll-et.cc -o epoll-et
$ ./epoll-et a 
```

ab 命令：

```bash
$ ab -n 1000 -k 127.0.0.1/ # 因为代码里服务器listen端口都是0.0.0.0:80
# 127.0.0.1 环回发给本地，发送1000个请求
# -k 使用HTTP的KeepAlive特性
```

LT 结果：Requests per second: 42.56 \[\#/sec\] \(mean\) 

ET 结果：Requests per second: 48.55 \[\#/sec\] \(mean\)

当我把服务器返回的数据大小改为48576时，开关EPOLLOUT更加频繁，性能的差异更大 

ab 命令：

```bash
$ ab -n 5000 -k 127.0.0.1/ 
```

LT 结果：Requests per second: 745.30 \[\#/sec\] \(mean\) 

ET 结果：Requests per second: 927.56 \[\#/sec\] \(mean\)

