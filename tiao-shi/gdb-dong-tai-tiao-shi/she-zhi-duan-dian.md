# 设置断点

### **为何要设置断点**

在介绍之前，我们首先需要了解，为什么需要设置断点。我们在指定位置设置断点之后，程序运行到该位置将会“暂停”，这个时候我们就可以对程序进行更多的操作，比如查看变量内容，堆栈情况等等，以帮助我们调试程序。

#### **断点设置示例**

断点设置有多种方式，分别应用于不同的场景。借助示例程序进行一一介绍：

```c
//test.c
#include<stdio.h>
void printNum(int a)
{
    printf("printNum\n");
    while(a > 0)
    {
    printf("%d\n",a);
    a--;
    }
}
void printNum2(int a,int num)
{
    printf("printNum\n");
    while(a > num && a>0)
    {
        printf("%d\n",a);
        a--;
    }
}
int div(int a,int b)
{
    printf("a=%d,b=%d\n",a,b);
    int temp = a/b;
    return temp;
}
int main(int argc,char *argv[])
{
    printNum2(12,5);
    printNum(10);
    div(10,0);
    return 0;
}
```

编译后得到可执行文件，`gdb test`在run之前设置断点

### 查看已经存在的断点

使用info breakpoints查看已设置断点，每一个断点都拥有一个断点号

```bash
(gdb)> info breakpoints
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x00000000004005fc in printNum2 at test.c:17
    breakpoint already hit 1 time
2       hw watchpoint  keep y                      a
    breakpoint already hit 1 time
    ignore next 3 hits
```

### **根据行号设置断点**

```bash
(gdb)> b 9  #break 可简写为b
#或者
(gdb)> b test.c:9 #程序运行到第9行会停止
```

### **根据函数名设置断点**

可以将断点设置在函数处：

```bash
(gdb)> b printNum # 程序在调用到printNum函数的时候会断住。
```

### **根据条件设置断点**

假设程序某处发生崩溃，而崩溃的原因怀疑是某个地方出现了非期望的值，那么你就可以在这里断点观察，当出现该非法值时，程序断住。这个时候我们可以借助gdb来设置条件断点，例如：

```bash
(gdb)> break test.c:23 if b==0 # 当在b等于0时，程序将会在第23行断住。
```

**通过condition给断点增加条件，条件触发时才会中断**  
假设上面的断点号为1，那么：

```bash
(gdb)> condition 1 b==0
```

会使得b等于0时，产生断点1。而实际上**可以很方便地用来改变断点产生的条件**

例如，之前设置b==0时产生该断点，那么使用condition可以修改断点产生的条件。

### **根据规则设置断点**

例如需要对所有调用printNum函数都设置断点，可以使用下面的方式：

```bash
(gdb)> rbreak printNum*
```

所有以printNum开头的函数都设置了断点。而下面是对所有函数设置断点：

rbreak用法：rbreak file:regex

```bash
rbreak . 
rbreak test.c:. #对test.c中的所有函数设置断点
rbreak test.c:^print #对以print开头的函数设置断点
```

### **设置临时断点**

假设某处的断点只想生效一次，那么可以设置临时断点，这样断点后面就不复存在了：

```bash
(gdb)> tbreak test.c:l0  #在第10行设置临时断点
```

### **设置执行一定次数后再中断的断点**

假如有某个地方，我们知道可能出错，但是前面30次都没有问题，虽然在该处设置了断点，但是想跳过前面30次，可以使用下面的方式：

```bash
(gdb)> ignore 1 30
```

其中，1是你要忽略的断点号，可以通过info checkpoints查找到，30是需要跳过的次数。这样设置之后，会跳过前面30次。再次通过info breakpoints可以看到：

```text
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x00000000004005e8 in printNum2 at test.c:16
    ignore next 30 hits
```

### **根据表达式值变化产生断点**

有时候我们需要观察某个值或表达式，知道它什么时候发生变化了，这个时候我们可以借助watch命令。例如：

```text
watch a
```

这个时候，让程序继续运行，如果a的值发生变化，则会打印相关内容，如：

```text
Hardware watchpoint 2: a
Old value = 12
New value = 11
```

但是这里要特别注意的是，程序必须运行起来，否则会出现：

```text
No symbol "a" in current context.
```

因为程序没有运行，当前上下文也就没有相关变量信息。

rwatch和awatch同样可以设置观察点前者是当变量值被读时断住，后者是被读或者被改写时断住。

### **禁用或启动断点**

有些断点暂时不想使用，但又不想删除，可以暂时禁用或启用。例如：

```bash
disable  #禁用所有断点
disable bnum #禁用标号为bnum的断点
enable  #启用所有断点
enable bnum #启用标号为bnum的断点
enable delete bnum  #启动标号为bnum的断点，并且在此之后删除该断点
```

### **断点清除**

断点清除主要用到clear和delete命令。常见使用如下：

```bash
clear   #删除当前行所有breakpoints
clear function  #删除函数名为function处的断点
clear filename:function #删除文件filename中函数function处的断点
clear lineNum #删除行号为lineNum处的断点
clear f:lename：lineNum #删除文件filename中行号为lineNum处的断点
delete  #删除所有breakpoints,watchpoints和catchpoints
delete bnum #删除断点号为bnum的断点
```

