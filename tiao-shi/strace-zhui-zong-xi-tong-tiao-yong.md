# strace——追踪系统调用

因为程序需要执行一些只有操作系统内核才能完成的操作时，从用户态转移到内核态，它需要使用系统调用，所以就需要一些追踪程序来追踪系统调用

更进一步了解：[https://blogs.oracle.com/linux/strace-the-sysadmins-microscope-v2](https://blogs.oracle.com/linux/strace-the-sysadmins-microscope-v2)

 在 Linux 中可以使用[`strace`](http://man7.org/linux/man-pages/man1/strace.1.html)

下面的例子展现来如何使用 `strace` 或 来显示`ls` 执行时，对[`stat`](http://man7.org/linux/man-pages/man2/stat.2.html) 系统调用进行追踪的结果。

```bash
$ sudo strace -e lstat ls -l > /dev/null
lstat("discriminator.py", {st_mode=S_IFREG|0664, st_size=1688, ...}) = 0
lstat("2_shanghaiA_csrnet.sh", {st_mode=S_IFREG|0664, st_size=135, ...}) = 0
lstat("csrnet.py", {st_mode=S_IFREG|0664, st_size=4767, ...}) = 0
lstat("results", {st_mode=S_IFDIR|0775, st_size=4096, ...}) = 0
lstat("3_shanghaiA_JHU_csrnet.sh", {st_mode=S_IFREG|0664, st_size=139, ...}) = 0
lstat("dataset.py", {st_mode=S_IFREG|0664, st_size=43530, ...}) = 0
lstat("test.py", {st_mode=S_IFREG|0664, st_size=1184, ...}) = 0
lstat("scripts", {st_mode=S_IFDIR|0775, st_size=4096, ...}) = 0
lstat("1_shanghaiA_csrnet.sh", {st_mode=S_IFREG|0664, st_size=103, ...}) = 0
lstat("4_shanghaiA_JHU_csrnet.sh", {st_mode=S_IFREG|0664, st_size=107, ...}) = 0
lstat("utils.py", {st_mode=S_IFREG|0664, st_size=3450, ...}) = 0
lstat("visualize.py", {st_mode=S_IFREG|0664, st_size=4580, ...}) = 0
lstat("__pycache__", {st_mode=S_IFDIR|0775, st_size=4096, ...}) = 0
lstat("train.py", {st_mode=S_IFREG|0664, st_size=12349, ...}) = 0
lstat("evaluate.py", {st_mode=S_IFREG|0664, st_size=4444, ...}) = 0
+++ exited with 0 +++
$ sudo strace -e lstat ps > /dev/null # 这个过程没有调用任何系统调用
+++ exited with 0 +++
```



