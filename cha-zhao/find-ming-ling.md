# find命令——查找文件



查找某目录下file数目

```bash
$ find faces/ -type c | wc -l
```

查找某目录下file并输出

```bash
$ find faces/ -type c -print
```

查找某目录下directory数目

```bash
$ find faces/ -type d | wc -l
```

查找并删除不正确的文件，也就是size为0的文件

```bash
$ find faces/ -size 0    -type f -delete
```

查找并删除大小小于40kb的文件

```bash
$ find faces/ -size -40k -type f -delete # 小于40k
$ find faces/ -size +40k -type f -delete # 大于40k
```

查找某种后缀的文件

```bash
$ find faces/ -name '*.png' -print # -print表示输出查找的结果 
$ find faces/ -iname '.*' -print # -iname不区分大小写，这个正则表达式可以查找隐藏文件
$ find faces/ ! -name '*.png' -print # ！表示否定参数，排除所指定到的模式
```

