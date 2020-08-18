# 调试器，pdb，pyflakes

### **调试器**

**调试器**是一种可以允许我们和正在执行的程序进行交互的程序，它可以做到：

* 当到达某一行时将程序暂停；
* 一次一条指令地逐步执行程序；
* 程序崩溃后查看变量的值；
* 满足特定条件是暂停程序；
* 其他高级功能。

如果debugging的时候不断使用print其实会比较累

### pdb 动态调试

很多编程语言都有自己的调试器。Python 的调试器是[`pdb`](https://docs.python.org/3/library/pdb.html).

允许我们和正在执行的程序进行交互调试

```bash
$ python -m ipdb app.py
```

下面对`pdb` 支持对命令进行简单对介绍：

* **l**\(ist\) - 显示当前行附近的11行或继续执行之前的显示；
* **s**\(tep\) - 执行当前行，并在第一个可能的地方停止
* **n**\(ext\) - 继续执行直到当前函数的下一条语句或者 return 语句；
* **b**\(reak\) - 设置断点（基于传入对参数）；
* **p**\(rint\) - 在当前上下文对表达式求值并打印结果。还有一个命令是**pp** ，它使用 [`pprint`](https://docs.python.org/3/library/pprint.html) 打印；
* **r**\(eturn\) - 继续执行直到当前函数返回；
* **q**\(uit\) - 退出调试器。

 因为 Python 是一种解释型语言，所以我们可以通过 `pdb` shell 执行命令。 [`ipdb`](https://pypi.org/project/ipdb/) 是一种增强型的 `pdb` ，它使用[`IPython`](https://ipython.org/) 作为 REPL并开启了 tab 补全、语法高亮、更好的回溯和更好的内省，同时还保留了`pdb` 模块相同的接口。

### pyflakes 静态分析

 有些问题是您不需要执行代码就能发现的。例如，仔细观察一段代码，您就能发现某个循环变量覆盖了某个已经存在的变量或函数名；或是有个变量在被读取之前并没有被定义。 这种情况下 [静态分析](https://en.wikipedia.org/wiki/Static_program_analysis) 工具就可以帮我们找到问题。

可以帮助检查代码错误，有哪些变量**重定义**，或者有哪些变量**定义未使用**，或者**未定义**

```bash
$ pyflakes app.py
```

