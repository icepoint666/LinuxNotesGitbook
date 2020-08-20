# shell、bash 和 zsh 等词的区分

### **shell 概念**

shell 是一个命令行解释器，顾名思义就是机器外面的一层壳，用于人机交互，只要是人与电脑之间交互的接口，就可以称为 shell。表现为其作用是用户输入一条命令，shell 就立即解释执行一条。不局限于系统、语言等概念、操作方式和表现方式等。 比如我们平时在黑框框里输入命令，叫 command-line interface \(CLI\)；在屏幕上点点点，叫graphical user interface \(GUI\)

### **Interactive 和 Non-interactive**

Interactive，如果你打开 terminal，在里面输入 bash 代码，回车得到输出，你就是在运行一个 Interactive shell，它的特征是可以让用户输入，然后直接把输出打到界面上；如果你运行一个包含了若干行的 shell 脚本，这些 shell 代码就运行在Non-interactive shell 中。

常见的 shell 解释器有 sh、bash这两种，其他的 ksh、csh 和 zsh 等是不常见的。

**Mac OS 中默认安装了以上所有类型**，Windows 需要自行安装，Linux 更不用说了。

就像上面说的，只要一门语言有解释器，就可以作为 shell 使用。比如Java 有第三方解释器 Jshell，PHP有 PHP Shell。如果你用过 windows，那你对 **cmd 这个词一定不陌生，它是 windows shell**，官方名称叫做 command interpreter。

**一般bash是sh的更加进化的版本，如果使用sh运行会报错，那么就最好使用bash**

### **bash**

Bash 是最常见的 shell，Mac 中默认 shell 就是 bash。

* login shell： 是指登录系统后所获得的顶层 shell，比如最常用的 ssh 登录，登录完后得到一个 login shell
* non-login shell：如果已经登录了桌面电脑，打开 terminal 进入的 shell 就是 Non-login shell

**唤起 bash shell 时**加载的不同文件：login shell 加载 ~/.bash\_profile ，而non-login shell 加载 ~/.bashrc 。

### **zsh**

很多人的 mac 中会使用 zsh 而不是 bash，一大半是因为 oh-my-zsh 这个配置集，它兼容 bash，还有自动补全等好用的功能。zsh 的配置文件~/.zshrc

