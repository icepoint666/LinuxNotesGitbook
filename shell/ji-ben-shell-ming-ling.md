# 基本shell命令

### 0. man+命令：查看帮助

### 1. echo输出

想尝试输出空格的字符串，要在行中加转义符，不然可能会Parse成两部分

```bash
$ echo hello\ world
hello world
$ echo $PATH # 输出环境变量
```

### **2. which命令**

输出某个bin的实际位置

```bash
$ which echo
/usr/bin/echo
```

### **3.重定向输入输出流**

在shell中，程序有两个主要的“流”：它们的输入流和输出流。 当程序尝试读取信息时，它们会从输入流中进行读取，当程序打印信息时，它们会将信息输出到输出流中。 通常，一个程序的输入输出流都是您的终端。也就是，您的键盘作为输入，显示器作为输出。

我们也可以重定向这些流！

最简单的重定向是 `< file` 和 `> file`

```bash
$ echo hello > hello.txt # 把输出的hello写进hello.txt
$ cat hello.txt
hello
$ cat < hello.txt # 把文件内容输入到cat中，也就是输出
hello
$ cat hello.txt
$ cat < hello.txt > hello2.txt 
#比较特殊相当于cp，cat不能理解这个重定向，所以不会输出
$ cat hello2.txt
hello
```

 还可以使用 `>>` 来向一个文件追加内容

**tee命令读取标准输入的数据，并将其内容输出成文件**

标准输入就可以来自于**重定向符**或者来自于**管道的输出**

```bash
$ echo 1 | tee file.txt
```

**题目：试图一起输出ls当前目录和ls父目录的结果**

```bash
$ cat < (ls) < (ls ..) # 依靠 < 与 括号 来实现
```

 **括号&lt;\(\)得到的是临时文件而不是STDIN**

`<( CMD )` 会执行 `CMD` 并将结果输出到一个临时文件中，并将 `<( CMD )` 替换成临时文件名。这在我们希望返回值通过文件而不是STDIN传递时很有用。例如， `diff <(ls foo) <(ls bar)` 会显示文件夹 `foo` 和 `bar` 中文件的区别。

### 4.管道\(pipe\)

将两个完全独立的program命令连接起来

 `|`操作符允许我们将一个程序的输出和另外一个程序的输入连接起来

```bash
$ ls -l / | tail -n 1
# ls -l的输出作为tail的输入，显示ls -l尾部最后一行
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
```

**tail命令**

显示 notes.log 文件的最后 10 行，请输入以下命令：

```bash
$ tail notes.log
```

要跟踪名为 notes.log 的文件的增长情况，请输入以下命令：

```bash
$ tail -f notes.log
```

也就是说tail -f filename 会把 filename 文件里的最尾部的内容显示在屏幕上，并且不断刷新

### 5.很实用的快捷编程命令

 `!!` - 完整到上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 `sudo !!`再尝试一次。

```bash
$ mkdir /mnt/new
mkdir: /mnt/new: Permission denied
$ sudo !!
```

 `$_` - 上一条命令的最后一个参数。如果你正在使用的是交互式shell，你可以通过按下 `Esc` 之后键入 . 来获取这个值。

示例：

```bash
$ mkdir test
$ cd $_ # 可以快捷的cd到刚刚创建的test
```

 `$?` - 返回前一个命令的返回值，用来检查上一条命令是否正确执行

返回值0表示正常执行，其他所有非0的返回值都表示有错误发生。

```bash
$ echo "hello" #正确命令
$ echo $?
0
$ true
$ echo $?
0
$ false
$ echo $?
1
$ grep sdasda #错误命令
$ echo $?
1
```

### 6.tree命令

输出当前路径下的文件目录树

### 7.alias命令

alias unalias来重命名，但是不会保存这些

需要修改.bashrc， .zshrc来永久保存

