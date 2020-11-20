# shell脚本

#### bash脚本其实是shell脚本的升级版，sh有些运行不了的，就要交给bash

#### 本质也算是一样的东西

shell编程100例：[https://zhuanlan.zhihu.com/p/84529054](https://zhuanlan.zhihu.com/p/84529054) （用于参考）

shell脚本面试问题70个：[https://zhuanlan.zhihu.com/p/83711089](https://zhuanlan.zhihu.com/p/83711089)（用于参考）

### 1.空格是command

#### 给variable赋值的时候不要在中间加入空格，会认为空格是一个命令

```bash
$ foo=bar #正确
$ foo = bar #错误
```

### 2.双引号与单引号的不同

```bash
$ foo=bar
$ echo "value is $foo"
value is bar
$ echo 'value is $foo'
value is $foo
```

### 3.向脚本传递参数

```bash
mcd () {
    mkdir -p "$1"
    cd "$1"
}

>>$ source mcd.sh #执行

>>$ ./show.sh file1.txt #包含参数$0与参数$1
```

这里 `$1` 是脚本的第一个参数。与其他脚本语言不同到是，bash使用了很多特殊到变量来表示参数、错误代码和相关变量。

* `$0` - 脚本名
* `$1` 到 `$9` - 脚本到参数。 `$1` 是第一个参数，依此类推。
* `$@` - 所有参数
* `$#` - 参数个数
* `$$` - process id

### 4.退出码搭配 `&&` \(与操作符\) 和 `||` \(或操作符\)使用，用来进行条件判断

决定是否执行其他程序。同一行的多个命令可以用 `;` 分隔。程序 `true` 的返回码永远是`0`，`false` 的返回码永远是`1`。让我们看几个例子

这里的true或者false可以表示一个执行语句的返回码

```bash
$ false || echo "Oops, fail"
Oops, fail

$ true || echo "Will not be printed"

$ true && echo "Things went well"
Things went well

$ false && echo "Will not be printed"

$ false ; echo "This will always run"
This will always run

$ echo "hello"
$ $_ && echo "Things went well" # 执行成功就输出成功信息
$ $_ || echo "Things fail" # 执行失败就输出错误信息
```

### 5.储存变量

```bash
$ echo $(pwd)
$ echo "We are in $(pwd)"
We are in /home/icep
```

### 6./dev/null

 `/dev/null`（或称空设备）在类Unix系统中是一个特殊的设备文件，它丢弃一切写入其中的数据（但报告写入操作成功）

**有两个作用：**

```bash
 $ grep foobar $file > /dev/null 2> /dev/null
```

* 1.grep foobar $file &gt; /dev/null
  * 将本身会输出的内容重定向到null，因为并不太想输出这些信息
* 2.Specifying **2**&gt;/**dev**/**null** will filter out the errors so that they will not be output to your console，会把error过滤掉并忽略，2就是error discriptor

### 7.for循环语句 if else语句

* for \_ in ; do ... done
* -ne表示not equal
* -e表示equal

```bash
#!/bin/bash
for file in $@; do
    grep foobar $file > /dev/null 2> /dev/null
    # 如果模式没有找到，则grep退出状态为 1
    # 我们将标准输出流和标准错误流重定向到Null，因为我们并不关心这些信息
    if [[ $? -ne 0 ]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done
```

### 8.脚本第一行shebang行指定执行文件

shell知道去用python解释器而不是shell命令来运行这段脚本，是因为脚本的开头第一行的[shebang](https://en.wikipedia.org/wiki/Shebang_%28Unix%29)。

**指明当前脚本运行的解释器路径**

\#!/bin/sh是指此脚本使用/bin/sh来解释执行，\#!是特殊的表示符，其后面根的是此解释此脚本的shell的路径。脚本的内容是由解释器解释的，我们可以用各种各样的解释器来写对应的脚本。 比如说/bin/csh脚本，/bin/perl脚本，/bin/awk脚本，/bin/sed脚本，甚至/bin/echo等等。

```bash
#!/usr/local/bin/python
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```

**shabang提高可移植性**： 它会利用环境变量中的程序来解析该脚本，这样就提高来您的脚本的可移植性。`env` 会利用我们第一节讲座中介绍过的`PATH` 环境变量来进行定位。 例如，使用了`env`的shebang看上去时这样的`#!/usr/bin/env python`

可能可以用python命令的时候本身已经link了很多个bin文件，它可以指定用哪个bin文件来执行这个脚本

**\#!/bin/sh与\#!/bin/bash区别**

GNU/Linux操作系统中的/bin/sh本是bash \(Bourne-Again Shell\) 的符号链接， 但鉴于bash过于复杂，有人把bash从NetBSD移植到Linux并更名为dash \(Debian Almquist Shell\)， 并建议将/bin/sh指向它，以获得更快的脚本执行速度。Dash Shell 比Bash Shell小的多，符合POSIX标准 Ubuntu继承了Debian，所以从Ubuntu 6.10开始默认是Dash Shell。

应该说，/bin/sh与/bin/bash虽然大体上没什么区别，但仍存在不同的标准。 标记为\#!/bin/sh的脚本不应使用任何POSIX没有规定的特性 \(如let等命令, 但\#!/bin/bash可以\)

### 9. set -e

```bash
#!/bin/bash

set -e

command 1
command 2
...

exit 0

```

你写的**每个脚本都应该在文件开头加上set -e**

**这句语句告诉bash如果任何语句的执行结果不是true则应该退出**。 这样的好处是防止错误像滚雪球般变大导致一个致命的错误，而这些错误本应该在之前就被处理掉。如果要增加可读性，可以使用set -o errexit，它的作用与set -e相同。

### 0.shell脚本实战样例

**1.批量修改文件名（find, mv, &gt;）**

```bash
#!/bin/bash
##查找txt文件
find /123 -type f -name "*.txt" > /tmp/txt.list
##批量修改文件名
for f in `cat /tmp/txt.list`
do
    mv $f $f.temp
done
```

**2.处理文本**

请将当前目录中demo.txt**第二行第三列**数据**输出到**demo2.txt中

```bash
cat demo.txt | awk 'NR==2{print $3}' > demo2.txt 
```

3.**查看占用内存最大的进程的PID和VSZ**

```bash
ps -aux | sort -k5nr | awk 'BEGIN{print "PID VSZ"}{print $2, $5}' | awk 'NR<3'
```

**4.统计内存使用**

```bash
#! /bin/bash
# author:xiaolan
sum=0
for mem in `ps aux |awk '{print $6}' |grep -v 'RSS' `
do
    sum=$[$sum+$mem]
done
echo "The total memory is $sum""k"
```

**5.计算本地用户数**

```bash
$ cat /etc/passwd | wc -l
```

