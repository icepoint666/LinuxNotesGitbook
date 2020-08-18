# journalctl日志工具

输出所有日志记录

```text
 $ sudo journalctl
```

找出包含某个信息的系统日志

```bash
# 它会找到所有包含intel(区分大小写)的系统日志。
$ sudo journalctl | grep -i intel
```

查看日志文件所占空间

```bash
$ sudo journalctl --disk-usage
Archived and active journals take up 920.0M on disk.
```

查看指定时间段的日志

利用 --since 与 --until 选项设定时间段，二者分别负责指定给定时间之前与之后的日志记录。时间值可以使用多种格式，比如下面的格式：

```text
YYYY-MM-DD HH:MM:SS
```

如果我们要查询 2018 年 3 月 26 日下午 8:20 之后的日志：

```text
$ journalctl --since "2018-03-26 20:20:00"
```

如果以上格式中的某些组成部分未进行填写，系统会直接进行默认填充。例如，如果日期部分未填写，则会直接显示当前日期。如果时间部分未填写，则缺省使用 "00:00:00"\(午夜\)。秒字段亦可留空，默认值为 "00"，比如下面的命令：

```text
$ journalctl --since "2018-03-26" --until "2018-03-26 03:00"
```



