# 用户相关命令

### **ubuntu 新建用户并挂载到指定的目录**

ubuntu新建用户有两个命令，**adduser** 和 **useradd**：

1、**adduser比较方便（推荐）**

直接使用命令：sudo adduser “用户名”，

```text
$ sudo adduser liangyu
```

然后设置密码，一路回车就OK了，

并且系统默认为新建的用户在/home目录下建立以用户名为名字的家目录，并且默认加入sudo用户组，自动设置shell，总之，很方便；

2、useradd，使用不太方便，需要自己设置家目录，需要自己把用户加入sudo用户组，如果不加入sudo用户组的话，sudo命令不能用，还需要自己设置shell等等。

**useradd默认shell是`/bin/sh`**，这个与`/bin/bash`还是有很大差别的，`/bin/sh`不能左右以及上下提示，也没有tab和高亮

可以观察`/etc/passwd` 通过useradd创建的用户对应的就是`/bin/sh`，所以我们需要手动修改成`/bin/bash`

但是有些时候，我们就是希望不要把家目录放到默认的/home/下，想放到别的硬盘下，这时候的useradd就显得很有用了，先看一下useradd可跟的参数，

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

**3. 删除用户 userdel** 

```bash
$ sudo userdel -r []
```

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

