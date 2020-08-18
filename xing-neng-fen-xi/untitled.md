# time命令——计时

python的Import time模块就可以计时，但是这个时间是real time

也就是从开始到运行结束的时间，可能会造成误导，因为您的电脑可能也在同时运行其他进程，也可能在此期间发生了等待。

### time命令：真实时间real time，用户时间user time，系统时间sys time

* 真实时间 - 从程序开始到结束流失掉到真实时间，包括其他进程到执行时间以及阻塞消耗的时间（例如等待 I/O或网络）；
* _User_ - CPU 执行用户代码所花费的时间；
* _Sys_ - CPU 执行系统内核代码所花费的时间。

通常来说，用户时间+系统时间代表了您的进程所消耗的实际 CPU时间

例如，试着执行一个用于发起 HTTP 请求的命令并在其前面添加 [`time`](http://man7.org/linux/man-pages/man1/time.1.html) 前缀。网络不好的情况下您可能会看到下面的输出结果。请求花费了 2s 才完成，主要事件是在等待网络发过来的消息，但是进程仅花费了 15ms 的 CPU 用户时间和 12ms 的 CPU 内核时间。

```bash
$ time curl https://missing.csail.mit.edu &> /dev/null
real    0m2.561s
user    0m0.015s
sys     0m0.012s
```

