# webbench

### webbench

安装：

```bash
$ git clone https://github.com/EZLippi/WebBench.git
$ sudo apt-get install ctags
$ cd Webbench
$ make
# make & make install 编译安装到usr/local/bin
```

使用

```bash
$ ./webbench -c 1000 -t 50 http://125.216.241.205:8080/ 

-f	--force	             不需要等待服务器响应
-r	--reload	发送重新加载请求
-t	--time   	运行多长时间，单位：秒"
-p	--proxy server:port	使用代理服务器来发送请求
-c	--clients	创建多少个客户端，默认1个"
-9	--http09	使用 HTTP/0.9
-1	--http10	使用 HTTP/1.0 协议
-2	--http11	使用 HTTP/1.1 协议
--get	使用 GET请求方法
--head	使用 HEAD请求方法
--options	使用 OPTIONS请求方法
--trace	使用 TRACE请求方法
```



