# Linux命令的本质是什么——type,which

**四种情况**

* 可执行程序：/usr/bin里面看到文件，编译的二进制文件
* shell内置命令：cd命令就是shell的内置命令
* shell函数，shell脚本中的函数
* alias命令：在其他命令的基础上定义自己的命令

**type + 命令**：识别命令属于上述哪种类型，并显示位置（常常优于which）

```text
$ type type
type is a shell builtin
$ type cp
cp is /bin/cp
```

**which + 命令**：显示可执行程序的位置

```text
$ which ls
/bin/ls
```

