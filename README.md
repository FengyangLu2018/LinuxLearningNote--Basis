# LinuxLearningNote--Basis
Linux基础知识学习，包括文件系统、Shell脚本和用户管理三个部分。


# 一、 文件系统
这一部分的主题是文件系统，主要涉及文件权限、目录管理、磁盘管理和文件压缩四个部分的内容。

## 1 文件权限
### 1.1 所有用户和所属组
Linux中的文件依据用户与本文件的关系呈现出不同的被管理权限，因此用户与文件的关系在文件权限管理中非常重要。用户与文件的关系可以分为分类，分别是文件所有者、文件所属用户组成员和其他用户。这三类用户对文件的管理权限在文件属性中分别设置。此外，root用户对所有文件都享有最高权限。
### 1.2 权限管理系统
在bash中输入命令`ls -al`，可以看到当前目录下所有文件的详细信息列表。
- 这个列表每行有7列，分别是权限、连接数、所有者、所属组、文件容量、修改日期和文件名。
- 文件权限属性共有10个字符组成，第1字符代表文件类型。d目录，－普通文件，l连接文件，b存储设备，c串口设备。
- 2－4字符代表所有者权限，5－7代表所属组权限，8－10代表其他用户的权限。每一组的三个字符分别代表可读、可写、可执行这三个权限。
### 1.3 如何修改文件权限
#### 1.3.1 改变文件所属组
`chgrp [-R] groupname filename`
-R代表递归处理，如果改变目录的所属组，该目录下所有的文件所属组均发生改变。
#### 1.3.2 改变文件所有者
`chown [-R] username filename`
-R同上。
#### 1.3.3 改变文件权限
1.数字类型命令 `chmod [-R] xyz filename` r:4,w:2,x:1
2.符号类型命令 
+ `chmod [-R] u=rwx,go=rx filename` u:文件所有者,g:所属组,o:其他用户,a:所有用户
+ `chmod [-R] a+w filename` 给所有用户都加上写入权限
### 1.4 目录与文件的权限意义
对目录而言，权限r代表用户可以使用`ls`命令读出此目录下文件的信息；权限w代表用户可以使用`mv`、`rm`、`cp`等命令对此目录下文件进行更改；权限x代表着用户可以使用`cd`进入此目录把其作为工作目录。
### 1.5 文件种类与拓展名
#### 1.5.1 文件种类
+ 一般文件，属性第一字符为－。
   + 纯文本文件（ASCII）
   + 二进制文件（binary）
   + 数据格式文件（data）
+ 目录，属性第一字符为d。
+ 连接文件，属性第一字符为l。
+ 设备文件
   + 块设备文件，属性第一字符为b。 
   + 字符设备文件，属性第一字符为c。
+ 套接字，属性第一字符为s。
+ 管道，属性第一字符为p。
#### 1.5.2 文件扩展名
+ Linux系统的文件是没有扩展名的，能不能执行取决于文件的属性
+ 单一文件或目录最长为255个字符
+ 完整路径最长为4096个字符
+ 文件名小数点开头代表隐藏文件

## 2 文件与目录管理
### 2.1 Linux目录配置标准：FHS
FHS针对目录树架构仅定义出三层目录下应该放置什么数据，分别是/，/usr和/var。
#### 2.1.1 / 根目录
以下是FHS规定的根目录下需存在的子目录：
+ /bin 单用户维护模式下还能被操作的命令，如cat，chmod，chown，date
+ /boot 开机使用的文件，包括内核文件及开机菜单与开机所需配置文件
+ /dev 设备接口文件
+ /etc 系统主要的配置文件，如账号密码文件、各种服务的初始化文件
+ /home 用户主文件夹所在目录
+ /lib 开机时会用到的函数库
+ /media 可删除的设备，软盘、光盘、DVD等设备暂时挂载在这里
+ /mnt 与/media相似
+ /opt 第三方软件放置的目录，如KDE桌面管理系统
+ /root 系统管理员的主文件夹，最好与根目录放在同一分区
+ /sbin 开机过程中需要的，包含开机、修复、还原系统所需的命令，如fdisk，fsck，ifconfig，init，mkfs等
+ /srv 一些网络服务启动后所需取用的数据目录，如www服务需要的网页数据
+ /tmp 一般用户或正在执行的程序暂时放置文件的地方，需定时清理
下面目录几个尽管没有被FHS所定义，也很重要：
+ /lost+found 当文件系统发生错误时，将一些丢失的片段放置在这个目录下，这个目录通常会在分区的最顶层
+ /proc 虚拟文件系统，数据放置在内存中，不占磁盘空间，记录内核、进程、外部设备的状态和网络的状态
+ /sys 虚拟文件系统，记录目前已加载的内核模块与内核检测到的硬件设备信息
根目录与开机、还原、系统修复等操作相关。根目录下与开机过程有关的目录不能与根目录放到不同分区，包括/etc,/bin/dev/lib和/sbin。根目录的分区应该越小越好，且应用程序所安装的软件最好不要和根目录放在同一分区内。
#### 2.1.2 /usr UNIX Software Resource
按照FHS的定义，/usr放置的数据属于可分享且不可变动的，中文名称为UNIX操作系统软件资源，类似windows系统的C:\windows\和c:\program files\的综合体。/usr的建议子目录如下：
+ /usr/bin/ 绝大部分用户可使用的与开机过程无关的命令
+ /usr/include/ 头文件与包含文件
+ /usr/lib/ 各应用软件的函数库、目标文件，以及不被一般用户惯用的执行文件或脚本
+ /usr/local/ 系统管理员在本机自行安装自己下载的软件（如果想安装新版软件又不想删除旧版，可以将新版软件安装在此目录下，此目录也有bin，etc，include，lib等子目录）
+ /usr/sbin/ 非系统正常运行所需的系统命令，如daemon
+ /usr/share/ 共享文件，几乎都是文本文件
  + /usr/share/man 在线帮助文件
  + /usr/share/doc 软件杂项的文件说明
  + /usr/share/zoneinfo 与时区有关的时区文件
+ /usr/src/ 源码，内核的源码放置在/usr/src/linux/目录下
+ /usr/X11R6/ X Window11R6重要数据放置的目录
#### 2.1.3 /var variable
如果/usr是安装时会占用较大的硬盘容量，那么/var是在系统运行后才会渐渐占用硬盘容量的目录，其常见子目录如下：
+ /var/cache/ 应用程序运行过程中产生的一些暂存文件
+ /var/lib/ 应用程序执行过程中需要使用的数据文件放置的目录
+ /var/lock/ 设备或文件上锁信息
+ /var/log/ 登录文件
+ var/mail/ 电子邮件
+ var/run/ 某些程序或服务启动后会将它们的PID放置在这个目录下
+ var/spool/ 队列数据，即排队等待其他程序使用的数据，这些数据在使用后通常都会被删除
### 2.2 目录与路径
#### 2.2.1 相对路径与绝对路径
+ 绝对路径由根目录写起，相对路径由当前路径写起
+ .代表当前位置路径，..代表当前位置的上级位置路径，-代表前一位置路径，~代表当前用户的工作目录路径，~abc代表用户abc的工作目录路径
#### 2.2.2 目录的相关操作
+ 切换目录 `cd` change directory
+ 显示目前所在目录 `pwd [-P]` print working directory，-P：获取正确的目录名称，而不是以连接文件的路径来显示
+ 新建目录 `mkdir [-mp] dirname` -p：根据dirname依序创建多层目录，-m 711：给予新建目录711的权限
+ 删除“空”目录 `rmdir [-p] dirname` -p：根据dirname连同上层的空目录一起删除
+ 删除目录及其下所有内容 `rm -r dirname`
#### 2.2.3 $PATH
+ 如果一个命令所在的路径在$PATH变量中，执行该命令仅需打命令名，无需打命令的路径，如cd，ls等
+ 不同用户默认的PATH不同
+ 用户可以修改PATH，`PATH="$PATH":/dirname`
+ 如果PATH中能够找到两个名字相同但路径不同的命令，默认执行先被查询的目录下的命令
### 2.3 文件与目录管理
#### 2.3.1 查看文件与目录ls
+ -a 全部文件连同隐藏文件一起列出来
+ -d 仅列出目录本身，不列出目录内的文件数据
+ -F 根据文件、目录等信息给予附加数据结构，如下：
  + *代表可执行文件
  + /代表目录
  + =代表socket文件
  + |代表FIFO文件
+ -h 将文件容量以Mb，kb等单位列出
+ -i 列出inode号码
+ -l 列出长数据串，包含文件的属性与权限等数据
+ -n 列出UID与GID，而非用户和和用户组的名称
+ -r 将排序结果反向输出，无论按名称还是按容量
+ -R 连同子目录内容一并列出
+ -S 以文件容量排序，而不是文件名排序
+ -t 依时间排序，而不是文件名
+ --color={never，always，auto} 是否根据文件特性给予颜色显示
+ --full-time 以完整时间模式输出
+ --time={atime，ctime} 输出访问时间或权限属性时间，而非内容更改时间
#### 2.3.2 复制文件或目录
+ cp [options] source destination
  + -a 相当于-pdr，综合p、d、r的效果，使目标文件与源文件内容和属性都完全相同（但需执行者有修改文件相关属性的权限）
  + -d 若源文件为连接文件，则复制连接文件属性而非文件本身属性
  + -f 强制，若目标文件已经存在且无法打开，则删除后再试
  + -i 若目标文件已存在，在覆盖时会先询问
  + -l 进行硬链接的连接文件创建，而非复制文件本身
  + -p 连同文件属性一起复制过去，而非使用默认属性，备份常用
  + -r 递归持续复制，用于目录的复制行为
  + -s 复制为软连接文件
  + -u 若目标文件比源文件旧才更新目标文件
+ cp [options] source1 source2 source3 ... directory 将多个源文件复制到目标目录下
#### 2.3.3 移除文件或目录
rm [-fir] file/directory
+ -f force，强制执行，忽略不存在的文件，不出现警告信息
+ -i 互动模式，删除前会询问
+ -r 递归删除，用于删除某个目录下所有内容
#### 2.3.4 移动文件与目录或更名
+ mv [-fiu] source destination
  + -f 若目标文件已存在，不询问直接覆盖
  + -i 若目标文件已存在，先询问
  + -u 目标文件比源文件旧，才覆盖更新
+ mv [options] source1 source2 source3 ... directory 将多个源文件移动到目标目录下
#### 2.3.5 取得路径的文件名和目录名
+ basename path 得到文件名
+ dirname path 得到目录名
### 2.4 文件内容查阅
#### 2.4.1 直接查看文件内容
+ cat [-AbEnTv] filename 正向列示
  + -A 相当于-vET，可列出一些特殊字符
  + -b 列出行号，仅针对非空白行做行号显示
  + -E 将结尾的断行字符$显示出来
  + -n 打印出行号，连同空白行也会有行号
  + -T 将[Tab]按键以^I显示出来
  + -v 
+ tac filename
+ nl [-bnw] filename
  + -b
  + -n
  + -w
