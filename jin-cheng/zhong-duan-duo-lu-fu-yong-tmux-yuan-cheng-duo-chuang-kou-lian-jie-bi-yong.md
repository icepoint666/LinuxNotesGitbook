# tmux\(终端多路复用\)——远程多窗口连接必用

**终端多路复用：terminal multiplexer: tmux**

当您在使用命令行接口时，您通常会希望同时执行多个任务**，**可以想要同时运行您的编辑器，并在终端的另外一侧执行程序。尽管再打开一个新的终端窗口也能达到目的，**使用终端多路复用器**则是一种**更好的办法**。

**分成三级:**

* sessions
  * windows
    * panes

当进入一个tmux，就是开启一个session

```bash
$ tmux #开启一个新的session，就会进入到那个窗口中
$ tmux new -t foobar # 开启一个新的session，并命名
```

像 [`tmux`](http://man7.org/linux/man-pages/man1/tmux.1.html) 这类的终端多路复用器可以允许我们基于面板和标签分割出多个终端窗口，这样您便可以同时与多个 shell 会话进行交互。

不仅如此，终端多路复用使我们可以**分离当前终端会话**并在**将来重新连接**。

这让您操作远端设备时的工作流大大改善，避免了 `nohup` 和其他类似技巧的使用。

`tmux` 的快捷键需要我们掌握，它们都是类似 `<C-b> x` 这样的组合，即需要先按下`Ctrl+b`，松开后再按下 `x`。

* **一些通用命令**

  * list: `tmux list-sessions`,`tmux list-windows`,`tmux list-panes`
  * kill:`tmux kill-session -t 3`, 类似kill-server, kill-session, kill-window

* **会话（一般就一个会话就够了）** - 每个会话都是一个独立的工作区，其中包含一个或多个窗口
  * `tmux` 开始一个新的会话, 使用默认名字：0
  * `tmux new -s NAME` 以指定名称开始一个新的会话
  * `tmux ls` 列出当前所有会话
  * 在 `tmux` 中输入 `<C-b> d` ，将当前会话分离detach
  * `tmux a` 重新连接最后一个会话。
  * `tmux a -t name` 来重新连接一个指定的具体会话
* **窗口（主要用多个窗口）** - 相当于编辑器或是浏览器中的标签页，从视觉上将一个会话分割为多个部分，**都是要在当前会话中找窗口**
  * `<C-b> c` 创建一个新的窗口，使用 `<C-d>`关闭
  * `<C-b> N` 跳转到第 _N_ 个窗口，注意每个窗口都是有编号的
  * `<C-b> p` 切换到前一个窗口
  * `<C-b> n` 切换到下一个窗口
  * `<C-b> ,` 重命名当前窗口
  * `<C-b> w` 列出当前所有窗口
* **面板（分屏）** - 像 vim 中的分屏一样，面板使我们可以在**一个屏幕里显示多个 shell**
  * `<C-b> "` 水平分割
  * `<C-b> %` 垂直n分割
  * `<C-b> <方向>` 切换到指定方向的面板，&lt;方向&gt; 指的是键盘上的方向键
  * `<C-b> z` 切换当前面板的缩放 zoom it
  * `<C-b> [` 开始往回卷动屏幕。您可以按下空格键来开始选择，回车键复制选中的部分
  * `<C-b> <空格>` 在不同的面板排布间切换

类似这样：

![](../.gitbook/assets/wu-biao-ti-1.png)

扩展阅读： [这里](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) 是一份 `tmux` 快速入门教程， [而这一篇](http://linuxcommand.org/lc3_adv_termmux.php) 文章则更加详细，

