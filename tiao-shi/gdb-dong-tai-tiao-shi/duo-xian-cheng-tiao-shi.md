# 多线程调试

编程多线程代码，目前使用两个线程进行测试：

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <unistd.h>

using namespace std;

mutex _mutex;
static int total = 0;

//创建一个函数，供多个线程调用
void fun(int num)
{
    while(1)
    {
        sleep(2);
        lock_guard<mutex>lock(_mutex);
        total +=num;
        cout<<"num:"<<num<<endl;
    }
}


int main()
{
    thread t1(fun,3);
    thread t2(fun,5);
    t1.join();
    t2.join();
    return 0;
}
```

编译：

```bash
g++ -std=c++11 testthread.cpp -o testthread -lpthread -g
```

然后进行`GDB`调试界面：

```bash
gdb testthread
```

出现调试界面：

```text
root@iZuf67on1pthsuih96udyfZ:~/GDB/test20200727# gdb test1
GNU gdb (Ubuntu 7.11.1-0ubuntu1~16.5) 7.11.1
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
root@iZuf67on1pthsuih96udyfZ:~/GDB/test20200730# ls
testthread  testthread.cpp
root@iZuf67on1pthsuih96udyfZ:~/GDB/test20200730# gdb testthread
GNU gdb (Ubuntu 7.11.1-0ubuntu1~16.5) 7.11.1
```

打断点，运行程序

```bash
(gdb) l 17
12      void fun(int num)
13      {
14          while(1)
15          {
16              sleep(2);
17              lock_guard<mutex>lock(_mutex);
18              total +=num;
19              cout<<"num:"<<num<<endl;
20          }
21      }
(gdb) b 17
Breakpoint 1 at 0x40117b: file testthread.cpp, line 17.
(gdb) run
Starting program: /root/GDB/test20200730/testthread
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[New Thread 0x7ffff6f4e700 (LWP 15195)]
[New Thread 0x7ffff674d700 (LWP 15196)]
[Switching to Thread 0x7ffff6f4e700 (LWP 15195)]

Thread 2 "testthread" hit Breakpoint 1, fun (num=3) at testthread.cpp:17
17              lock_guard<mutex>lock(_mutex);
(gdb)
```

查看下线程：

```bash
(gdb) info threads
  Id   Target Id         Frame
  1    Thread 0x7ffff7fe3740 (LWP 15191) "testthread" 0x00007ffff7bc298d in pthread_join (threadid=140737336633088, thread_return=0x0) at pthread_join.c:90
* 2    Thread 0x7ffff6f4e700 (LWP 15195) "testthread" fun (num=3) at testthread.cpp:17
  3    Thread 0x7ffff674d700 (LWP 15196) "testthread" fun (num=5) at testthread.cpp:17
(gdb)

```

说明出现两条线程，目前执行的是线程ID是`2`

我们可以使用`GDB`命令去切换线程

```bash
(gdb) thread 3
[Switching to thread 3 (Thread 0x7ffff674d700 (LWP 15196))]
#0  fun (num=5) at testthread.cpp:17
17              lock_guard<mutex>lock(_mutex);
```

我们很自然的回发现，线程都会在我们断点处停止，而且我们线程也已经切换完成；

我们一步一步的看会出现什么现象：

```bash
(gdb) thread 3
[Switching to thread 3 (Thread 0x7ffff674d700 (LWP 15196))]
#0  fun (num=5) at testthread.cpp:17
17              lock_guard<mutex>lock(_mutex);
(gdb) n
num:3

Thread 3 "testthread" hit Breakpoint 1, fun (num=5) at testthread.cpp:17
17              lock_guard<mutex>lock(_mutex);
(gdb) n
[Switching to Thread 0x7ffff6f4e700 (LWP 15195)]

Thread 2 "testthread" hit Breakpoint 1, fun (num=3) at testthread.cpp:17
17              lock_guard<mutex>lock(_mutex);
(gdb) n
num:5
18              total +=num;
(gdb) n
19              cout<<"num:"<<num<<endl;
```

从调试信息可以观察到，我们一步步调试的时候，另外一个线程也会出现在调试信息中

这里我们使用命令，我们只使用单条线程进行调试操作

```bash
(gdb) set scheduler-locking on
(gdb) 
(gdb) info threads
  Id   Target Id         Frame 
  1    Thread 0x7ffff7fe3740 (LWP 15278) "testthread" 0x00007ffff7bc298d in pthread_join (threadid=140737336633088, thread_return=0x0) at pthread_join.c:90
* 2    Thread 0x7ffff6f4e700 (LWP 15279) "testthread" fun (num=3) at testthread.cpp:17
  3    Thread 0x7ffff674d700 (LWP 15280) "testthread" fun (num=5) at testthread.cpp:17
(gdb) n
18              total +=num;
(gdb) p total
$1 = 0
(gdb) n
19              cout<<"num:"<<num<<endl;
(gdb) p total
$2 = 3
(gdb) n
num:3
17              lock_guard<mutex>lock(_mutex);
(gdb) n
14          while(1)
(gdb) n
16              sleep(2);
(gdb) n

Thread 2 "testthread" hit Breakpoint 1, fun (num=3) at testthread.cpp:17
17              lock_guard<mutex>lock(_mutex);
(gdb) n
18              total +=num;
(gdb) p total
$3 = 3
(gdb) n
19              cout<<"num:"<<num<<endl;
(gdb) p total
$4 = 6
(gdb) 
```

通过调试信息可以知道，我们发现了，设置了单个线程执行的时候，只有当前线程在执行，我们执行到加锁的上一句，切换一下线程，再试下，看一下会不会出现我们想要的结果：

```bash
(gdb) thread 3
[Switching to thread 3 (Thread 0x7ffff674d700 (LWP 15280))]
#0  fun (num=5) at testthread.cpp:17
17              lock_guard<mutex>lock(_mutex);
(gdb) n

Thread 3 "testthread" hit Breakpoint 1, fun (num=5) at testthread.cpp:17
17              lock_guard<mutex>lock(_mutex);
(gdb) n
18              total +=num;
(gdb) p total
$5 = 9
(gdb) n
19              cout<<"num:"<<num<<endl;
(gdb) n
num:5
17              lock_guard<mutex>lock(_mutex);
(gdb) n
14          while(1)
(gdb) n
16              sleep(2);
(gdb) n

Thread 3 "testthread" hit Breakpoint 1, fun (num=5) at testthread.cpp:17
17              lock_guard<mutex>lock(_mutex);
(gdb) n
18              total +=num;
(gdb) p total
$6 = 14
(gdb) 
```

