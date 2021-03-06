

#### 一、目录操作
1. 创建目录：mkdir test1
2. 复制目录下所有文件：cp -r test1/* test2
3. 删除目录：rm -rf test1
4. 查看目录下内容：ls
```
-a 全部，包括隐藏文件
```
5. 切换目录：cd
6. 将/usr下的所有文件和目录移到当前目录下：mv /usr/*  .
7. 查看当前工作目录：pwd
8. 压缩解压：tar
```
-c ：新建打包文件
-t ：查看打包文件的内容含有哪些文件名
-x ：解打包或解压缩的功能，可以搭配-C（大写）指定解压的目录，注意-c,-t,-x不能同时出现在同一条命令中
-j ：通过bzip2的支持进行压缩/解压缩
-z ：通过gzip的支持进行压缩/解压缩
-v ：在压缩/解压缩过程中，将正在处理的文件名显示出来
-f filename ：filename为要处理的文件
-C dir ：指定压缩/解压缩的目录dir
例如：
压缩：tar -jcv -f filename.tar.bz2 要被处理的文件或目录名称
查询：tar -jtv -f filename.tar.bz2
解压：tar -jxv -f filename.tar.bz2 -C 欲解压缩的目录
```

#### 二、文件操作
1. 创建文件：touch file1
2. 复制文件至/testNewFile：cp /file1 /testNewFile 
3. 删除文件：rm file1
4. 查看文件：cat file1 / more file1 / less file1 / tail file1
```
关于tail：从指定点开始将文件写到标准输出。

-f 循环读取
-q 不显示处理信息
-v 显示详细的处理信息
-c<数目> 显示的字节数
-n<行数> 显示行数
--pid=PID 与-f合用,表示在进程ID,PID死掉之后结束. 
-q, --quiet, --silent 从不输出给出文件名的首部 
-s, --sleep-interval=S 与-f合用,表示在每次反复的间隔休眠S秒 

使用tail命令的-f选项可以方便的查阅正在改变的日志文件，tail -f filename会把filename里最尾部的内容显示在屏幕上，并且不但刷新。
```
5. 编辑文件：vim file1
6. 移动文件至/testNewFile：mv file1 /testNewFile

#### 三、进程相关操作
1. ps ：显示运行的进程，还会显示进程的一些信息如pid, cpu和内存使用情况等。
```
-A ：所有的进程均显示出来
-a ：不与terminal有关的所有进程
-u ：有效用户的相关进程
-x ：一般与a参数一起使用，可列出较完整的信息
-l ：较长，较详细地将PID的信息列出
-ef ：显示所有命令，连带命令行
```
2. kill 终止进程：kill -signal PID
```
1：SIGHUP，启动被终止的进程
2：SIGINT，相当于输入ctrl+c，中断一个程序的进行
9：SIGKILL，强制中断一个进程的进行
15：SIGTERM，以正常的结束进程方式来终止进程
17：SIGSTOP，相当于输入ctrl+z，暂停一个进程的进
```

#### 四、网络相关操作
1. ping 用于确定主机与外部连接状态：ping [options] [hostname或IP地址]

2. ssh 用于远程登录上Linux主机：ssh [options] [用户名@]hostname

3. scp 用于远程复制(linux 系统下基于 ssh 登陆进行安全的远程文件拷贝命令)：scp [options] file_source file_target 

4. telnet 用于远程登录操作：telnet [options] [主机]

5. wget 用于远程下载：wget [options] [URL地址]

6. netstat [-acCeFghilMnNoprstuvVwx] [-A<网络类型>] [--ip] 显示网络状态
 查看443端口: netstat -tunlp | grep 443 或 netstat -apn | grep 443

#### 五、权限相关操作
1. chomd 控制文件如何被用户调用：chmod [-cfvR] [--help] [--version] mode file...
```
-c : 若该文件权限确实已经更改，才显示其更改动作
-f : 若该文件权限无法被更改也不要显示错误讯息
-v : 显示权限变更的详细资料
-R : 对目前目录下的所有文件与子目录进行相同的权限变更(即以递回的方式逐个变更)
--help : 显示辅助说明
--version : 显示版本

关于mode：权限设定字串，格式：[ugoa...][[+-=][rwxX]...][,...]， 其中：
u 表示该文件的拥有者，g 表示与该文件的拥有者属于同一个群体(group)者，o 表示其他以外的人，a 表示这三者皆是。
+ 表示增加权限、- 表示取消权限、= 表示唯一设定权限。
r 表示可读取，w 表示可写入，x 表示可执行，X 表示只有当该文件是个子目录或者该文件已经被设定过为可执行。
```
2. chown 改变文件所有者（只有root才有这个权限）：chown [-cfhvR] [--help] [--version] user[:group] file...

```
user : 新的文件拥有者的使用者 ID
group : 新的文件拥有者的使用者组(group)
-c : 显示更改的部分的信息
-f : 忽略错误信息
-h :修复符号链接
-v : 显示详细的处理信息
-R : 处理指定目录以及其子目录下的所有文件
--help : 显示辅助说明
--version : 显示版本
```
3. sudo ：以系统管理者的身份执行指令，也就是说，经由 sudo 所执行的指令就好像是 root 亲自执行。sudo su可切换至root。 

#### 六、搜索操作
1. find：find [PATH] [option] [action]
```
[PATH]：路径，指定搜索范围
[option]：指定匹配的类型，比如用户-user 名字-name 大小-size
```
例如：find / -name config ，即：查找根目录下名字带有config的

2. whereis ： whereis [-bmsu] [BMS 目录名 -f ] 文件名
```
　-b 定位可执行文件。
　-m 定位帮助文件。
　-s 定位源代码文件。
　-u 搜索默认路径下除可执行文件、源代码文件、帮助文件以外的其它文件。
　-B 指定搜索可执行文件的路径。
　-M 指定搜索帮助文件的路径。
　-S 指定搜索源代码文件的路径。
```

3. which：which 可执行文件名称 
which会在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。
```
-n 　指定文件名长度，指定的长度必须大于或等于所有文件中最长的文件名。
-p 　与-n参数相同，但此处的包括了文件的路径。
-w 　指定输出时栏位的宽度。
-V 　显示版本信息
```

#### 七、firewalld防火墙相关
1. 查看firewalld服务状态：systemctl status firewalld （active或者inactive）
2. 查看firewall的状态：firewall-cmd --state
3. 开启、重启、关闭、firewalld.service服务：service firewalld start/restart/stop
4. 查看防火墙规则：firewall-cmd --list-all 
5. 查询端口是否开放：firewall-cmd --query-port=8080/tcp
6. 开放80端口：firewall-cmd --permanent --add-port=80/tcp
7. 移除端口：firewall-cmd --permanent --remove-port=8080/tcp
8. 重启防火墙(修改配置后要重启防火墙)：firewall-cmd --reload

参数解释：
- firwall-cmd：是Linux提供的操作firewall的一个工具；
- --permanent：表示设置为持久；
- --add-port：标识添加的端口；

#### 八、其他
1. 管道 | 
简单来说, Linux 中管道的作用是将上一个命令的输出作为下一个命令的输入, 像 pipe 一样将各个命令串联起来执行。

2. grep过滤：grep [-acinv] [--color=auto] '查找字符串' filename
该命令常用于分析一行的信息，若当中有我们所需要的信息，就将该行显示出来。该命令通常与管道命令一起使用，用于对一些命令的输出进行筛选加工等，比如可以加在ps, tail, cat后面。
```
例如：
（1）过滤得到当前系统中的 ssh 进程信息：ps -aux | grep 'ssh'
（2）递归过滤/var/log/目录中包含 linux 的记录：grep -r 'linux' /var/log/
（3）过滤出 /etc 目录中名字包含 ssh 的目录(不包括子目录)：ls /etc | grep 'ssh'
```

3. clear：清屏

4. ln：为某一个文件在另外一个位置建立一个同步的链接
```
Linux文件系统中，有所谓的链接(link)，我们可以将其视为档案的别名，而链接又可分为两种 : 硬链接(hard link)与软链接(symbolic link)，硬链接的意思是一个档案可以有多个名称，而软链接的方式则是产生一个特殊的档案，该档案的内容是指向另一个档案的位置。硬链接是存在同一个文件系统中，而软链接却可以跨越不同的文件系统。

软链接：
1.软链接，以路径的形式存在。类似于Windows操作系统中的快捷方式
2.软链接可以 跨文件系统 ，硬链接不可以
3.软链接可以对一个不存在的文件名进行链接
4.软链接可以对目录进行链接

硬链接:
1.硬链接，以文件副本的形式存在。但不占用实际空间。
2.不允许给目录创建硬链接
3.硬链接只有在同一个文件系统中才能创建

语法：ln [参数][源文件或目录][目标文件或目录]

必要参数:
-b 删除，覆盖以前建立的链接
-d 允许超级用户制作目录的硬链接
-f 强制执行
-i 交互模式，文件存在则提示用户是否覆盖
-n 把符号链接视为一般目录
-s 软链接(符号链接)
-v 显示详细的处理过程

选择参数:
-S “-S<字尾备份字符串> ”或 “--suffix=<字尾备份字符串>”
-V “-V<备份方式>”或“--version-control=<备份方式>”
```