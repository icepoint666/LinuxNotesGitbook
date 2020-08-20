# 用户相关命令

### **ubuntu 新建用户并挂载到指定的目录**

ubuntu新建用户有两个命令，**adduser** 和 **useradd**：

1、其中adduser比较方便，直接使用命令：sudo adduser “用户名”，

```text
$ sudo adduser liangyu
```

然后设置密码，一路回车就OK了，

并且系统默认为新建的用户在/home目录下建立以用户名为名字的家目录，并且默认加入sudo用户组，自动设置shell，总之，很方便；

2、useradd，使用不太方便，需要自己设置家目录，需要自己把用户加入sudo用户组，如果不加入sudo用户组的话，sudo命令不能用，还需要自己设置shell等等，但是有些时候，我们就是希望不要把家目录放到默认的/home/下，想放到别的硬盘下，这时候的useradd就显得很有用了，先看一下useradd可跟的参数，

```text
$ useradd  -d  /mnt/lab/username   -m -s  /bin/bash  username
```

 //这样建立的用户并没有加入到sudo用户组；

这样新建用户username的家目录就会在/mnt/lab/username，使用的shell是/bin/bash。

然后再使用命令：

```text
$ passwd username
```

设置密码就可以了。

### **用户相关命令**

```bash
$ w                      # 查看活动用户
$ who                    # 查看活动用户 (同上)
$ who -a                 # 查看所有活动/非活动用户
$ id <用户名>            # 查看指定用户信息
$ last                   # 查看用户登录日志
$ cut -d: -f1 /etc/passwd   # 查看系统所有用户
$ cut -d: -f1 /etc/group    # 查看系统所有组
$ crontab -l             # 查看当前用户的计划任务
```

