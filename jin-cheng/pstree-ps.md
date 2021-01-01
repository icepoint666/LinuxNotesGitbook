# pstree, ps

**查看子进程树pstree**

查看init进程,也就是所有进程的进程树关系：

```bash
$ pstree -up
```

查看某一进程的进程子树：

```bash
$ pstree -p 6648
```

**查看进程**

```text
$ ps -ef                 # 查看所有进程
$ ps -a                  # 查看用户进程
$ ps                     # 查看当前窗口tty进程
```

**查看进程启动及运行时间**

```text
$ ps -eo lstart                        # 启动时间
$ ps -eo etime                         # 运行多长时间
$ ps -eo pid,lstart,etime | grep 5176  # 查看某个进程的pid，启动时间，运行时间
```

