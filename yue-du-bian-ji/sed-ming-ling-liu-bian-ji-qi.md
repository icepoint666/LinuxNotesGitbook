# sed命令——流编辑器

详细参考：[https://www.cnblogs.com/tureno/articles/6677942.html](https://www.cnblogs.com/tureno/articles/6677942.html)

sed是一款流编辑工具，用来**对文本进行过滤与替换工作**， sed通过输入读取文件内容，但一次仅读取一行内容进行某些指令处理后输出，sed更适合于处理大数据文件。

sed在处理文本文件的时候，会在内存上创建一个模式空间，然后把这个文件的每一行调入模式空间用相应的命令处理，处理完输出；接着处理下一行，直到最后。

**与vim区别：**

**vim需要通知处理文件的哪几行才会去处理，sed默认会处理文件的所有行，除非你告诉它不处理哪几行。**

### sed的基本语法

sed \[选项\] \[定址commands\] \[inputfile\]

关于定址：定址可以是0个、1个、2个；**通知sed去处理文件的哪几行**。

* 0个：没有定址，处理文件的所有行
* 1个：行号，处理行号所在位置的行
* 2个：行号、正则表达式，处理被行号或正则表达式包起来的行

选项：

*  --version 显示sed版本hao
* --help 显示帮助文档
* -n 关闭默认输出，默认将自动打印所有行
* -e 多点编辑，允许多个脚本指令被执行。
* -r 支持扩展正则+ ? \(\) {} \|
* -i **可以修改原文件，慎用！**
* -f 支持使用脚本

命令：

* p打印行
* d 删除行
* s 替换
* n替换第几个匹内容
* w另存为
* a 之后添加一行
* i 当前行之前插入文本
* y 替换匹配内容

### **实际案例讲解**

**1． 参数p: 打印匹配行**

```bash
$ sed -n '2p' /etc/passwd 打印出第2行
$ sed -n '1,3p' /etc/passwd 打印出第1到第3行
$ sed -n '$p' /etc/passwd 打印出最后一行
$ sed -n '/user/p' /etc/passwd 打印出含有user的行
$ sed -n '/\$/p' /etc/passwd 打印出含有$元字符的行，$意为最后一行
$ sed -n '$=' ok.txt    打印总行数
```

**2．参数a和i: 插入文本和附加文本\(插入新行\)**

```bash
$ sed -n '/FTP/p'/etc/passwd #打印出有FTP的行
$ sed '/FTP/a\ 456' /etc/passwd #在含有FTP的行后面新插入一行，内容为456
$ sed '/FTP/i\ 123' /etc/passwd #在含有FTP的行前面新插入一行，内容为123
$ sed '/FTP/i\ "123"' /etc/passwd #在含有FTP的行前面新插入一行，内容为"123″
$ sed '5 a\ 123'   #/etc/passwd 在第5行后插入一新行，内容为123
$ sed '5 i\ "12345″' #/etc/passwd 在第5行前插入一新行，内容为"12345"
```

**3．参数d：删除文本**

删空格

```text
$ sed -i ‘s/[ ]*//g’ ~/$cid.txt
```

删除无内容空行

```text
$ sed ‘/^$/d’ file.conf > file.new.conf
$ sed -i ‘/^$/d’ ~/$cid.txt
```

删第一行

```text
$ sed -i ’1d’ ~/$cid.txt
```

删前两行

```text
$ sed -i ’1,2d’ ~/$cid.txt
```

删除最后一行：

```text
sed -e ‘$d’ file
```

删除最后两行：

```text
sed ‘N;$!P;D’ file
sed ‘N;$!P;$!D;$d’ file
```

删除最后n行：

```text
$vi sed.sh
$!/bin/bash
A=$(sed -n ‘$=’ file)
sed $(($A-n+1)),${A}d file
​
$ sed ‘/user/d’/etc/passwd
```

删除带有string的行\(del include love row head\)

```text
sed -i ‘/^love/d’ file
sed -i ‘/love/d’ file
```

vi删除包含strings前4行，后34行

```text
:/strings/-4,+34d
```

删除配置文件中\#号注释行

```text
sed ‘s#\#.*##’ file.conf > file.new.conf
sed ‘s,\#.*,,’
```

删除配置文件中//号注释行

```text
sed ‘s#//.*##’ file.conf > file.new.conf
sed ‘s@//.*@@’
```

删除由空格和Tab而成的空行

```text
sed ‘/^[[:space:]]*$/d’ file.conf > file.new.conf
sed ‘/^[[:space:]]*$/d’
```

删除尾行的空格

```text
sed -e ‘s/.$//’ file > file.new.conf
```

在尾行添加1个空格

```text
sed ‘s/[0-9]$/& /g’ file > flile.new.conf
```

**4． 参数s：替换文本,替换命令用替换模式替换指定模式，格式为：**

```text
[ a d d r e s s [，address]] s/pattern-to-find/replacement-pattern/[g p w n]
​
$ sed ‘s/user/USER/’/etc/passwd 将第1个user替换成USER,g表明全局替换
$ sed ‘s/user/USER/g’/etc/passwd 将所有user替换成USER
$ sed ‘s/user/#user/’/etc/passwd 将第1个user替换成#user,如用于屏蔽作用
$ sed ‘s/user//’/etc/passwd 将第1个user替换成空
$ sed ‘s/user/&11111111111111/’/etc/passwd 如果要附加或修改一个很长的字符串，可以使用（ &）命令，&命令保存发现模式以便重新调用它，然后把它放在替换字符串里面，这里是把&放前面
$ sed ‘s/user/11111111111111&/’/etc/passwd 这里是将&放后面 在包含字符串test的任意行上，将111替换成222
$ sed ‘/test/s/111/222/g’ sample.txt
```

**5. 快速一行正则表达式命令** 下面是一些一行命令集。（\[ \]表示空格，\[TAB\]表示t a b键）

```text
‘s/\ . $//g’ 删除以句点结尾行
‘-e/abcd/d’ 删除包含a b c d的行
‘s/[ ] [ ] [ ] */[ ]/g’ 删除一个以上空格，用一个空格代替
‘s/^ [ ] [ ] *//g’ 删除行首空格
‘s/\ . [ ] [ ] */[ ]/g’ 删除句点后跟两个或更多空格，代之以一个空格
‘/^ $/d’ 删除空行
‘s/^ .//g’ 删除第一个字符
‘s/COL \ ( . . . \ )//g’ 删除紧跟C O L的后三个字母
‘s/^ \///g’ 从路径中删除第一个\
‘s/[ ]/[TAB]//g’ 删除所有空格并用t a b键替代
‘S/^ [TAB]//g’ 删除行首所有t a b键
‘s/[TAB] *//g’ 删除所有t a b键
```

**6、用sed删除文件中的换行符，杀鸡哪需牛刀？**

```text
tr -d ‘\n’ file
```

真的需要sed

```text
sed -nr ‘ H;
$ {
x;
s/\n//g;
p
}’
```

**7、sed获取ip**

```text
[root@c01 ~]# ifconfig eth0|sed -ne ‘s/^.*addr:\([0-9.]*\).*$/\1/p’
192.168.100.180
​
[root@c01 ~]# ifconfig eth0 | sed -e ‘/inet/!d’ -e ‘s/.*addr://’ -e ‘s/[ ].*//’
192.168.100.180
```

**8、\/转义字符**

```text
echo /usr/local/bin | sed ‘s/\/usr\/local\/bin/\/common\/bin/’
find . -name “*.html” |xargs sed -i ‘s#/canApp/evaluation/html/ index.html#http://www.wallcopper.com/eva/#g’
都可以把当前目录下的.c .h文件中的TAB替换成4个空格。
find . -name “*.[c|h]” | xargs sed -i ‘s/\t/ /g’
```

**9、参数f：**

以下三个命令相等，先在包含字符串test的任意行上，将111替换成222，再将字符f替换成hello

```text
$ sed ‘/test/s/111/222/g ; s/f/hello/g’ sample.txt
$ sed -e ‘/test/s/111/222/g’ -e ‘s/f/hello/g’ sample.txt
```

选项-f: 将要执行的所以编译命令放入在文件中

```text
$ more scher
/test/s/111/222/g
s/f/hello/g’ sample.txt
$ sed -f scher sample.txt
```

**10、参数q： 表示跳离sed。最多与一个地址参数配合**

打印出含有a或b字符的行，一旦遇到数字，即刻停止打印。

```text
$ sed -n -e ‘/[0-9]/q’ -e ‘/[a b]/p’
到china quit
sed ‘china/q’ myfile.txt
```

**11、参数-i后缀:替换并将源文件备份改后缀**

```text
$ sed -i.bak ‘s/123/efg/g’ a.txt 备份源文件
```

**12、参数r:在某行插入文件**

```text
$ sed -i ’2 r readfile.txt’ writefile.txt
```

**13、参数w：读入文件中的内容存入到另一文件中（temp.txt\)。最多与一个地址参数配合。**

将sample.txt文件中含test字符串的数据行，copy至temp.txt档中储存\(原temp.txt中无内容）

```text
$ sed -e ‘/test/w temp.txt’ sample.txt
```

**14、参数y：转换数据中的字符。最多与两个地址参数配合。**

1.将文件中的小写字母替换成大写字母。

```text
$ sed -e ‘y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/’ sample.txt
```

其中前后字符个数必须相同.

**15、参数!:表示不执行函数参数。 将文件中除了important字符串，其余全删除。**

```text
$ sed -e ‘/important/!d’ sample.txt
```

**16、参数=：表示印出资料的行数。最多与两个地址参数配合。 1.印出文件资料并显示行数。**

```text
$ sed -e ‘=’ sample.txt
```

计算总行数

```text
$ sed -n ‘$=’ a.txt
```

**其他案例**

**@1，打印一到三行**

```text
[root@tx3 ~]# cp /etc/passwd /t1
[root@tx3 ~]#  cat -n t1 | sed  -n '1,3p'
    1root:x:0:0:root:/root:/bin/bash
    2bin:x:1:1:bin:/bin:/sbin/nologin
    3daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

**@2，查看passwd文件的lp~halt行**

```text
[root@tx3 ~]#  cat -n t1 |sed -n '/lp/,/halt/p'
    5lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    6sync:x:5:0:sync:/sbin:/bin/sync
    7shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
    8halt:x:7:0:halt:/sbin:/sbin/halt
```

**@3，打印uid是0或1的行**

```text
[root@tx3 ~]# sed -n '/x:[01]:/p' t1
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
```

