# rg命令——查找代码（侧重）

grep出现了很多替代品：包括 [ack](https://beyondgrep.com/), [ag](https://github.com/ggreer/the_silver_searcher) 和 [rg](https://github.com/BurntSushi/ripgrep)。它们都特别好用，但是功能也都差不多，我比较常用的是 ripgrep \(`rg`\) ，因为它速度快，而且用法非常符合直觉。

```bash
# 查找所有python文件中使用了requests 库的文件
$ rg -t py 'import requests'
# 查找在指定的目录下所有Python文件
$ rg "import requests" -t py ~/icep/
# 查找所有没有写 shebang 的shell文件（包含隐藏文件）-u是不忽略隐藏文件
$ rg -u --files-without-match "^#\!" -t sh
# 查找所有的foo字符串，并打印其之后的5行
$ rg foo -A 5
# --stats打印匹配的统计信息（匹配的行和文件的数量）
$ rg "import requests" -t py --stats ~/icep/
```

