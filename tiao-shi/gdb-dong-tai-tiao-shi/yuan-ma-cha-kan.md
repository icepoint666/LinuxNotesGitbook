# 源码查看

调试的时候往往需要一边查看代码，一边调试，如果已经开始了调试，而查看源码或者编辑源码却要另外打开一个窗口，那未免显得太麻烦。gdb内部也支持查看代码的功能

 介绍如何在GDB调试模式下**查看源码或对源码进行编辑**。

我们先准备一些源码，分别是main.c：

```c
//main.c
#include<stdio.h>
#include"test.h"
int main(void)
{
    printf("it will print from 5 to 1\n");
    printNum(5);
    printf("print end\n");

    printf("it will print 1 to 5\n");
    printNum1(5);
    printf("print end\n");
    return 0;
}
```

头文件test.h：

```c
#ifndef _TEST_H
#define _TEST_H
#include<stdio.h>
void printNum(int n);
void printNum1(int n);
#endif
```

以及test.c:

```c
#include"test.h"
void printNum(int n)
{
    if( n < 0)
        return;
    while(n > 0)
    {
        printf("%d\n",n);
        n--;
    }
}

void printNum1(int n)
{
    if( n < 0)
        return;
    int i = 1;
    while(i <= n)
    {
        printf("%d\n",i);
        i++;
    }
}
```

编译运行：

```bash
$ gcc -g  -o main  main.c test.c
$ chmod +x main
$ ./main
it will print from 5 to 1
5
4
3
2
1
print end
it will print 1 to 5
1
2
3
4
5
print end
```

程序功能比较简单，用来打印5到1的数以及1到5的数

### list命令——列出源码

* 指定行
* 指定函数
* 指定某文件的行或者函数

```bash
$ gdb main
(gdb)> l
1    //main.c
2    #include<stdio.h>
3    #include"test.h"
4    int main(void)
5    {
6        printf("it will print from 5 to 1\n");
7        printNum(5);
8        printf("print end\n");
9    
10        printf("it will print 1 to 5\n");
(gdb)>
```

直接输入l可从第一行开始显示源码，继续输入l，可列出后面的源码。后面也可以跟上+或者-，分别表示要列出上一次列出源码的后面部分或者前面部分。

#### **列出指定行附近源码**

l后面可以跟行号，表明要列出附近的源码：

```bash
(gdb)> l 9
4    int main(void)
5    {
6        printf("it will print from 5 to 1\n");
7        printNum(5);
8        printf("print end\n");
9    
10        printf("it will print 1 to 5\n");
11        printNum1(5);
12        printf("print end\n");
13        return 0;
```

#### **列出指定函数附近的源码**

这个很容易理解，而使用也很简单，l后面跟函数名即可，例如：

```bash
(gdb)> l printNum
1    #include"test.h"
2    void printNum(int n)
3    {
4        if( n < 0)
5            return;
6        while(n > 0)
7        {
8            printf("%d\n",n);
9            n--;
10        }
```

在这里，l后面跟上函数名printNum，它便列出了printNum函数附近的源码。

#### **设置源码一次列出行数**

不知道你有没有发现，在列出函数源码的时候，它并没有列全，因为l每次只显示10行，那么有没有方法每次列出更多呢？  
我们可以通过listsize属性来设置，例如设置每次列出20行：

```bash
(gdb)> set listsize 20
(gdb)> show listsize
Number of source lines gdb will list by default is 20.
```

这样每次就会列出20行，当然也可以设置为0或者unlimited，这样设置之后，列出就没有限制了，但源码如果较长，查看将会不便。

#### **列出指定行之间的源码**

例如，要列出3到15行之间的源码：

```bash
(gdb)> l 3,15
3    {
4        if( n < 0)
5            return;
6        while(n > 0)
7        {
8            printf("%d\n",n);
9            n--;
10        }
11    }
12    
13    void printNum1(int n)
14    {
15        if( n < 0)
```

启始行和结束行号之间用逗号隔开。两者之一也可以省略，例如：

```bash
(gdb)> list 3,
3    {
4        if( n < 0)
5            return;
6        while(n > 0)
7        {
8            printf("%d\n",n);
9            n--;
10        }
11    }
12    
```

省略结束行的时候，它列出从开始行开始，到指定大小行结束

#### **列出指定文件的源码**

前面执行l命令时，默认列出main.c的源码，如果想要看指定文件的源码呢？

因此可以使用：

```bash
(gdb)> l test.c:1
1    #include"test.h"
2    void printNum(int n)
3    {
4        if( n < 0)
5            return;
6        while(n > 0)
7        {
8            printf("%d\n",n);
9            n--;
10        }
(gdb)
```

来查看指定文件指定行，或者指定文件指定函数：

```bash
(gdb)> l test.c:printNum1
9            n--;
10        }
11    }
12    
13    void printNum1(int n)
14    {
15        if( n < 0)
16            return;
17        int i = 1;
18        while(i <= n)
(gdb) 
```

或者指定文件指定行之间：

```bash
(gdb)> l test.c:1,test.c:3
1    #include"test.h"
2    void printNum(int n)
3    {
(gdb)>
```

### 指定/更换源码路径

在查看源码之前，首先要确保我们的程序能够关联到源码，一般来说，我们在自己的机器上加上-g参数编译完之后，使用gdb都能查看到源码，但是如果源码被移走了

例如，我现在将main.c移动到当前的temp目录下，再执行l命令：

```bash
(gdb)> l
1    main.c: No such file or directory.
(gdb)> 
```

它就会提示找不到源码文件了，那么怎么办呢？  
我们可以使用**dir命令指定源码路径**，例如：

```bash
(gdb)> dir ./temp
Source directories searched: /home/hyb/workspaces/gdb/sourceCode/./temp:$cdir:$cwd
```

这个时候它就能找到源码路径了。我这里使用的是相对路径，保险起见，你也可以使用绝对路径。

**set substitute-path更换源码目录**

可以使用set substitute-path from to将原来的路径替换为新的路径，那么我们如何知道原来的源码路径是什么呢？借助readelf命令可以知道：

```bash
$ readelf main -p .debug_str
  [     0]  long unsigned int
  [    12]  short int
  [    1c]  /home/hyb/workspaces/gdb/sourceCode
  [    40]  main.c
（显示部分内容）
```

main为你将要调试的程序名，这里我们可以看到原来的路径，那么我们现在替换掉它：

```bash
(gdb)> set substitute-path /home/hyb/workspaces/gdb/sourceCode /home/hyb/workspaces/gdb/sourceCode/temp
(gdb)> show substitute-path
List of all source path substitution rules:
  `/home/hyb/workspaces/gdb/sourceCode' -> `/home/hyb/workspaces/gdb/sourceCode/temp'.
(gdb)
```

设置完成后，可以通过show substitute-path来查看设置结果。这样它也能在正确的路径查找源码啦。

需要注意的是，这里**对路径做了字符串替换**，那么如果你有多个路径，可以做多个替换。甚至可以对指定文件路径进行替换。

最后你也可以通过unset substitute-path \[path\]取消替换。

