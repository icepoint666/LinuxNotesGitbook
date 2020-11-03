# /etc文件系统

**/etc目录包含各种系统配置文件，下面说明其中的一些。其中包括了用户信息文件/etc/passwd**

1. /etc/rc或/etc/rc.d或/etc/rc?.d：启动、或改变运行级时运行的脚本或脚本的目录。

2. **/etc/passwd**：用户信息文件，其中的域给出了用户名、真实姓名、用户起始目录、加密口令和用户的其他信息。

4. /etc/fstab：指定启动时需要自动安装的文件系统列表。也包括用swapon -a启用的swap区的信息。

5. /etc/group：类似/etc/passwd ，但说明的不是用户信息而是组的信息。包括组的各种数据。

10. /etc/mtab：当前安装的文件系统列表。由脚本\(scritp\)初始化，并由 mount命令自动更新。当需要一个当前安装的文件系统的列表时使用\(例如df命令\)。

14. /etc/profile 、/etc/csh.login、/etc/csh.cshrc：登录或启动时bourne或cshells执行的文件。这允许系统管理员为所有用户建立全局缺省环境。

