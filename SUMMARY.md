# Table of contents

* [Linux操作](README.md)

## shell

* [基本shell命令](shell/ji-ben-shell-ming-ling.md)
* [shell脚本](shell/shell-jiao-ben.md)
* [shell、bash 和 zsh 等词的区分](shell/shellbash-he-zsh-deng-ci-de-qu-fen.md)
* [使用管道进行数据整理（管道/过滤器模式）](shell/shi-yong-guan-dao-jin-hang-shu-ju-zheng-li-guan-dao-guo-lv-qi-mo-shi.md)
* [xargs命令——配合管道利器](shell/xargs-ming-ling-pei-he-guan-dao-li-qi.md)
* [配置文件——.bashrc等，ln命令](shell/pei-zhi-wen-jian-.bashrc-deng-ln-ming-ling.md)

## Tools

* [Useful Tools](tools/useful-tools.md)
* [guake命令界面](tools/guake-ming-ling-jie-mian.md)
* [journalctl日志工具](tools/journalctl-ri-zhi-gong-ju.md)
* [ctrl+r快速搜索history里命令关键字段](tools/ctrl+r-kuai-su-sou-suo-history-li-ming-ling-guan-jian-zi-duan.md)

## 查找

* [find命令——查找文件](cha-zhao/find-ming-ling.md)
* [locate命令——根据名字定位文件](cha-zhao/locate-ming-ling-gen-ju-ming-zi-ding-wei-wen-jian.md)
* [grep命令——查找字段](cha-zhao/grep-ming-ling-cha-zhao-zi-duan.md)
* [rg命令——查找代码（侧重）](cha-zhao/rg-ming-ling-cha-zhao-dai-ma-ce-zhong.md)
* [fzf命令——interactive查找字段](cha-zhao/fzf-ming-ling-interactive-cha-zhao-zi-duan.md)
* [利用grep查找history历史记录](cha-zhao/li-yong-grep-cha-zhao-history-li-shi-ji-lu.md)

## 阅读，编辑

* [less命令——分页阅读](yue-du-bian-ji/less-ming-ling-fen-ye-yue-du.md)
* [sed命令——流编辑器](yue-du-bian-ji/sed-ming-ling-liu-bian-ji-qi.md)
* [awk命令——处理文本](yue-du-bian-ji/awk-ming-ling-chu-li-wen-ben.md)

## 进程

* [Linux通过signal机制控制进程——kill命令，bp命令，jobs命令](jin-cheng/linux-tong-guo-signal-ji-zhi-kong-zhi-jin-cheng-kill-ming-ling-bp-ming-ling-jobs-ming-ling.md)
* [tmux\(终端多路复用\)——远程多窗口连接必用](jin-cheng/zhong-duan-duo-lu-fu-yong-tmux-yuan-cheng-duo-chuang-kou-lian-jie-bi-yong.md)
* [pstree, ps](jin-cheng/pstree-ps.md)

## c++编译运行

* [gcc/g++基本使用, 其它选项](c++-bian-yi-yun-hang/g++-ji-ben-shi-yong.md)
* [gcc/g++链接 静态库 动态库](c++-bian-yi-yun-hang/g++-lian-jie-jing-tai-ku-dong-tai-ku.md)
* [CMake基本使用](c++-bian-yi-yun-hang/cmake.md)
* [CMakeLists.txt最关键的部分](c++-bian-yi-yun-hang/cmakelists.txt-zui-guan-jian-de-bu-fen.md)

## 调试

* [调试器，pdb，pyflakes](tiao-shi/untitled.md)
* [gdb——动态调试](tiao-shi/gdb-dong-tai-tiao-shi/README.md)
  * [启动调试](tiao-shi/gdb-dong-tai-tiao-shi/qi-dong-tiao-shi.md)
  * [设置断点](tiao-shi/gdb-dong-tai-tiao-shi/she-zhi-duan-dian.md)
  * [变量查看](tiao-shi/gdb-dong-tai-tiao-shi/bian-liang-cha-kan.md)
  * [源码查看](tiao-shi/gdb-dong-tai-tiao-shi/yuan-ma-cha-kan.md)
  * [单步调试](tiao-shi/gdb-dong-tai-tiao-shi/dan-bu-tiao-shi.md)
  * [多线程调试](tiao-shi/gdb-dong-tai-tiao-shi/duo-xian-cheng-tiao-shi.md)
* [gprof——性能分析：运行时长和调用](tiao-shi/untitled-1.md)
* [valgrind——内存检测](tiao-shi/valgrind.md)
* [strace——追踪系统调用](tiao-shi/strace-zhui-zong-xi-tong-tiao-yong.md)
* [coredump——崩溃快照](tiao-shi/coredump-beng-kui-kuai-zhao.md)
* [log工具](tiao-shi/log-gong-ju.md)

## 性能分析

* [Linux性能观察工具一览](xing-neng-fen-xi/linux-xing-neng-guan-cha-gong-ju-yi-lan.md)
* [time命令——计时](xing-neng-fen-xi/untitled.md)
* [CPU性能分析](xing-neng-fen-xi/cpu-xing-neng-fen-xi/README.md)
  * [CPU相关性能排查，优化](xing-neng-fen-xi/cpu-xing-neng-fen-xi/cpu-xiang-guan-xing-neng-pai-cha-you-hua.md)
* [内存性能分析](xing-neng-fen-xi/nei-cun-xing-neng-fen-xi.md)
* [压力测试](xing-neng-fen-xi/ya-li-ce-shi/README.md)
  * [ab\(Apache Bench\)](xing-neng-fen-xi/ya-li-ce-shi/ab-apache-bench.md)
* [perf命令——事件分析](xing-neng-fen-xi/perf-ming-ling-shi-jian-fen-xi.md)
* [火焰图——可视化perf](xing-neng-fen-xi/huo-yan-tu-han-shu-tiao-yong-hao-shi.md)
* [资源监控](xing-neng-fen-xi/zi-yuan-jian-kong.md)

## 网络

* [Untitled](wang-luo/untitled.md)

## 用户

* [用户相关命令](yong-hu/yong-hu-xiang-guan-ming-ling.md)
* [文件权限与文件所有](yong-hu/wen-jian-quan-xian-yu-wen-jian-suo-you.md)

## 硬件

* [硬件相关命令](ying-jian/ying-jian-xiang-guan-ming-ling.md)

## 文件系统

* [Linux各个目录作用](wen-jian-xi-tong/linux-ge-ge-mu-lu-zuo-yong.md)
* [/etc文件系统](wen-jian-xi-tong/etc-wen-jian-xi-tong.md)
* [/proc文件系统](wen-jian-xi-tong/proc-wen-jian-xi-tong.md)
* [/var文件系统](wen-jian-xi-tong/var-wen-jian-xi-tong.md)

