# less命令——分页阅读

 `less` 为我们创建来一个文件分页器，使我们可以通过翻页的方式浏览较长的文本。

管道后面加less: 分页浏览输出流

```bash
$ ssh myserver 'journalctl | grep sshd' | less
```

less+文件：分页浏览

```bash
$ ssh myserver 'journalctl | grep sshd' > ssh.log
$ less ssh.log
```

### 使用指南

1.全屏导航

* ctrl + F - 向前移动一屏
* ctrl + B - 向后移动一屏
* ctrl + D - 向前移动半屏
* ctrl + U - 向后移动半屏

2.单行导航

* j - 向前移动一行
* k - 向后移动一行

3.其它导航

* G - 移动到最后一行
* g - 移动到第一行
* q - 退出 less 命令

