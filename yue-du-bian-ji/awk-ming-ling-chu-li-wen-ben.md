# awk命令——处理文本

**awk**：[Linux awk命令详解](https://www.cnblogs.com/ftl1012/p/awk.html)

awk是一个强大的文本分析工具，相对于grep的查找，sed的编辑，awk是处理文本。简单来说awk就是**把文件逐行的读入，以空格为默认分隔符将每行切成一列一列**，切开的部分再进行各种分析处理。

详解：[https://www.cnblogs.com/xudong-bupt/p/3721210.html](https://www.cnblogs.com/xudong-bupt/p/3721210.html)

> 依次处理每一行，并读取里面的每一个字段。

**基本用法**

```bash
# 格式：awk 做什么 文件
awk '{print $0' lan.txt
```

* 上面lan.txt是awk需要处理的文本文件。
* 前面单引号里面有一个大括号，
* 单引号里面就是每一行的处理动作。其中print为打印命令

**示例1：**

```bash
echo 'my name is lanlan' | awk '{print $0}'
# my name is lanlan
```

* 上面代码中，print $0代表当前征行，所以执行结果就是把每一行原样打印出来

awk根据**空格和制表符**，将每一行分成若干段，依次为

```bash
echo 'my name is lanlan'| awk '{print $3}'
# lanlan
```

**示例2：**

使用/etc/passwd文件进行操作，操作

```bash
awk -F ':' '{ print $1 }' /etc/passwd
```

**3 变量**

上面我们说了，可以使用符号 “3代表第一个字段，第二个字段，第三个字段¨G57G为了方便，我们直接使用/etc/passwd文件进行操作，¨G58G∗∗3变量∗∗上面我们说了，可以使用符号“+” 数字的方式表示第几个字段，其实还有一些变量可以直接表示相应的字段。比如 “$NFb” 表示最后一个字段

```text
echo 'my name is lanlan'| awk '{print $NF}'
awk -F ':' '{print NR ") " $1}' demo.txtshe
```

这里出现了双引号，表示原样输出

其他常用的内置变量

* FILENAME：当前文件名
* FS：字段分隔符，默认是空格和制表符。
* RS：行分隔符，用于分割每一行，默认是换行符。
* OFS：输出字段的分隔符，用于打印时分隔字段，默认为空格。
* ORS：输出记录的分隔符，用于打印时分隔记录，默认为换行符。
* OFMT：数字输出的格式，默认为％.6g。

**函数**

既然算是一门语言，函数当然少不了，下面看一波常用的函数

函数toupper\(\)用于将字符转为大写

```bash
awk -F ':' '{ print toupper($1) }' demo.txt
```

可以发现第一个字段输出的时候变成了大写

* tolower\(\)：字符转为小写。
* length\(\)：返回字符串长度。
* substr\(\)：返回子字符串。
* sin\(\)：正弦。
* cos\(\)：余弦。
* sqrt\(\)：平方根。
* rand\(\)：随机数。

**过滤**

通过使用相应的条件，过滤出自己想要的内容

```text
awk '条件 动作' 文件名
```

```bash
$ awk -F ':' '/usr/ {print $1}' demo.txt
root
daemon
bin
sys
```

* 这里/usr/表示只输出包含usr的行

**这个例子输出奇数行**

```bash
# 输出奇数行
$ awk -F ':' 'NR % 2 == 1 {print $1}' demo.txt
root
bin
sync

# 输出第三行以后的行
$ awk -F ':' 'NR >3 {print $1}' demo.txt
sys
sync
```

下面的例子输出第一个字段等于指定值的行。

```bash
$ awk -F ':' '$1 == "root" {print $1}' demo.txt
root

$ awk -F ':' '$1 == "root" || $1 == "bin" {print $1}' demo.txt
root
bin
```

**if语句**  通过if语句编写比较复杂的内容

```bash
$ awk -F ':' '{if ($1 > "m") print $1}' demo.txt
root
sys
sync
```

