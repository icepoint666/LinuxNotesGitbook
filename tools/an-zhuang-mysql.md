# 安装mysql

```bash
$ su
$ apt install mysql-server
$ apt install mysql-client
$ apt install libmysqlclient-dev  # 从而可以把mysql/mysql.h添加到include中
```

检查是否安装成功

```bash
# mysql命令
$ mysql

# mysql/mysql.h是否能够找到
$ find /usr/ -name 'mysql.h'
```

