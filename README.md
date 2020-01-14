# LinuxLearningNote--Basis
Linux基础知识学习，包括文件系统、Shell脚本和用户管理三个部分。

## 一、 文件系统
这一部分的主题是文件系统，主要涉及文件权限、目录管理、磁盘管理和文件压缩四个部分的内容。
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


