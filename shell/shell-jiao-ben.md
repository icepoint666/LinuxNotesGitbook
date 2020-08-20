# shell脚本

#### bash脚本其实是shell脚本的升级版，sh有些运行不了的，就要交给bash

#### 本质也算是一样的东西

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

### 3.执行简单的脚本，向脚本传递参数

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

```bash
#!/usr/local/bin/python
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```

**shabang提高可移植性**： 它会利用环境变量中的程序来解析该脚本，这样就提高来您的脚本的可移植性。`env` 会利用我们第一节讲座中介绍过的`PATH` 环境变量来进行定位。 例如，使用了`env`的shebang看上去时这样的`#!/usr/bin/env python`

可能可以用python命令的时候本身已经link了很多个bin文件，它可以指定用哪个bin文件来执行这个脚本



