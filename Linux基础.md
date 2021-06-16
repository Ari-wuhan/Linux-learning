# 一、初入Linux

## 忘记root密码怎么办

1.重启系统

按3秒钟向下的方向键，不让它进入系统，移动光标到第一行，按字母E编辑，进入另外一个界面

2.进入紧急模式

将光标移动到 "ro"，把"ro"修改为:"rw init=/sysroot/bin/sh"

然后同时按住Ctrl +x，系统就进入了紧急模式

3.修改root密码

先切换到原始系统chroot /sysroot

然后修改密码passwd

```
chroot /sysroot
passwd
LANG=en//解决乱码问题
```

让修改的密码生效:

```
touch ./autorelabel
```

同时按住Ctrl +D，然后输入reboot重启系统

## 快捷键

Ctrl + c：终止当前命令

Ctrl +l:	清屏

Ctrl +a:	让光标移动到命令的最前面

Ctrl +e:	让光标移动到命令的最后面

Ctrl +z：暂停当前进程

## 设置网络

>   把机器设置为NAT模式

## 查看IP地址:

```
ip add
```

![截屏2021-06-16 上午10.40.03的副本](/Users/hanwu/Desktop/截屏2021-06-16 上午10.40.03的副本.png)

第一个是127.0.0.1，本地回环地址，lo是网卡名

第二个是192.168.187.2，是自动获取的IP地址，ens33时是卡名

## 测试是否联网

```
pip -c 4 www.baidu.com
```

![image-20210616104653288](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616104653288.png)

ping通了，显示已经可以上网

## 手动设置IP地址

## 配置语言支持

查看系统支持的语言环境

```
localectl list-locales | egrep "zh_CN|en_US"
```

设置语言环境（中文）:

```
localectl set-locale LANG="zh_CN.UTF-8"
```

下次登录生效

## 查询帮助文档-----man命令

```
man [命令]
```

由于查看命令的帮助文档

如:

```
man ls
```

## Linux系统目录结果

- bin：存放最常用的命令
- /boot:启动Linux的核心文件，包括连接文件和镜像文件
- /dev：存放Linux的外部设备
- /etc：存放系统管理所需的配置文件和子目录（系统配置，不能随便改）
- /home：用户的家目录
- /lib与/ib64:存放系统最基本的动态链接共享库（共享）
- /media：把识别的设备挂载到该目录下
- /mnt:让用户挂在别到文件系统，如把光驱到/mnt上，然后进入查看内容
- /opt:安装软件的目录
- /proc：虚拟目录，是系统内存的映射，可用来获取系统信息
- /root:系统管理员的家目录（超级用户）
- /run:存放服务的pid
- /sbin:存放超级用户（系统管理员）的系统管理程序
- /srv:存放服务启动之后需要提取的数据
- /sys:存放去硬件驱动程序相关的命令
- /usr:非常重要的m目录，存放用户的应用程序和文件
- /usr/bin:存放系统用户使用的应用程序
- /usr/sbin:存放超级用户使用的比较高级的管理程序和系统守护程序
- /usr/src:存放内核源代码
- /var:不断扩张经常修改的动态目录，包括各种日志文件或pid文件

## 正确关机、重启

如果要关机，要确保当前系统中没有其他用户在登录系统

可以用who查看是否与其他人在登录

10分钟后关机

```
shutdown -h 10
```

立即关机

```
shutdown -h now
```

设定关机之间，如9:00关机:

```
shutdown -h 9:00
```

重启:

```
reboot
```

立即重启:

```
shutdown -r now
```

关闭系统:

```
halt
```

# 二、Linux目录文件和目录管理

## 路径命令

### cd命令

切换目录，cd跟目录名，会切换到对应目录下

cd直接返回根目录

"."表示当前所在目录

".."表示上一句的目录

pwd表示显示当前所在目录

<img src="/Users/hanwu/Library/Application Support/typora-user-images/image-20210616121448435.png" alt="image-20210616121448435"  />

### which命令

which命令用于查找某个命令的绝对路径

```
which mkdir
```

![image-20210616123620246](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616123620246.png)

### mdkir命令

mkdir用于创建目录

```
mkdir -p  /tmp/test/123
```

"-p"表示递归创建多级目录

![image-20210616121951989](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616121951989.png)

也可以直接查看指定目录的属性

```
ls -ld /tmp/test
```

![image-20210616122226247](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616122226247.png)

### rmdir命令

rmdir用于删除空目录

>    但是只能删除目录，不能删除文件

![image-20210616122526846](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616122526846.png)

### rm命令

-r:  删除目录

![image-20210616122844521](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616122844521.png)

-f：强制删除，既是没有文件，也会强制执行

![image-20210616122954508](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616122954508.png)

删除目录时，一定要加上r:

![image-20210616123406072](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616123406072.png)

## 环境变量PATH

为什么我们使用命令时，只是使用命令，而没有使用它们的绝对路径呢？为什么命令在哪里都可以用呢？

这时环境变量PATH在起作用

试打印$PATH:

```
echo $PATH
```

![截屏2021-06-16 下午12.39.00](/Users/hanwu/Desktop/截屏2021-06-16 下午12.39.00.png)

将/root这个路径加到%PATH:

```
PATH = $PATH/root
```

![image-20210616124318552](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616124318552.png)

### cp命令

cp是copy的缩写，即复制

```
cp [选项]来源文件    目的文件
```

-r ：

>   复制一个目录，必须加上r，否则不能复制类似于rm命令

![image-20210616135216053](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616135216053.png)

“-i"：安全选项，当遇到一个已经存在的文件时，会询问是否覆盖

![image-20210616135650381](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616135650381.png)

### mv命令

mv是move的缩写，即移动

```
mv [选项][源目标文件][目标文件或目录]
```

当目标文件/目录不存在时，会将源目标文件重命名:

![image-20210616140435682](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616140435682.png)

如果目标目录存在，就会把目录移动到指定目录里

![截屏2021-06-16 下午2.07.16](/Users/hanwu/Desktop/截屏2021-06-16 下午2.07.16.png)

## 与文档相关的命令

### cat命令

cat命令是将文件的内容显示在屏幕上

-n 显示行号

-A 显示隐藏内容



![image-20210616141320706](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616141320706.png)

###  tac命令

cat的逆序显示

![截屏2021-06-16 下午2.14.47](/Users/hanwu/Desktop/截屏2021-06-16 下午2.14.47.png)

### more命令

当文件内容太多时可以使用more，向上向下翻屏幕

空格下一行，Q推出

![image-20210616141719988](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616141719988.png)

### less命令

less命令比more有更多的功能

输入/。然后输入想查找的字符串（向下搜索）。然后回车，就会自动查找想要的字符串，可以按N显示下一个

输入的“？”，则是向上搜索

### head命令

head命令默认显示前10行

![image-20210616142237272](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616142237272.png)

当然也可以指定行数:

![image-20210616142347429](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616142347429.png)

### tail命令

是head的逆序显示，即显示最后10行

![image-20210616142516434](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616142516434.png)

指定行数：

![image-20210616142630881](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616142630881.png)

## 文件的所有者和所属者

每一个目录和文件都会有一个所有者和所属者，还有一个其他用户

![截屏2021-06-16 下午2.30.49](/Users/hanwu/Desktop/截屏2021-06-16 下午2.30.49.png)

其中的两个root分别是所有者和所属者

## Linux文件属性

用ls -l目录显示目录下的文件时，共显示了9列命令（用空格划分）

第一列：表示文件类型

>   "-":表示普通文件
>
>   "d":表示目录文件
>
>   "c"：表示设备文件
>
>   "s"：表示套接字文件
>
>   "l"：表示链接文件

![image-20210616143830788](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616143830788.png)

文件类型后面紧跟着9位：

分别为所有者权限，所属组权限，和其他用户的权限

"r"代表可读，"w"代表可写,"x"代表可执行

对于一个目录来讲，打开这个目录即为执行这个目录

任何一个用户必须要有x权限才能打开并查看文件内容

如

```
-rwxr-xr--
```

表示普通文件，所有者可读可写可执行，所属组可读可执行，其他用户只可读

第二列：然后是文件占有的节点

第三列：文件的所有者

第四列：文件的所属组

第五列：文件的大小

第六七八列：表示最后一次修改时间，月日以及具体时间

第九列：文件名

## 更改文件的权限

### chgrp命令

chgrp(change group的缩写)，改变文件的所属组

```
chgrp [组名] [文件名]
```

![image-20210616145314365](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616145314365.png)

还可以更改目录的所属组：

![image-20210616145547439](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616145547439.png)

### chown命令

chown(change owner缩写)，用于更改文件的所有者

-R:用于递归更改，递归目录上的文件所有者全部更改

```
chown [-R] 账户名  文件名
```

![image-20210616150100426](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616150100426.png)

### chmod命令

chmod(change mode简写)，改变用户对文件/目录的读、写和执行权限

为方便文件的更改，用数字代替rwx，即二进制，有位1无为0

如rwx即111，对应7

r-x即101，对应5

```
chmod -R xyz 文件名
```

![image-20210616150616352](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616150616352.png)

>   如果某个目录，只想自己看到，而不想让其他人看到，可以设置为740，rwxr-----

除了数字，还指出用字母rwx的形式来设置

>   u,g,o分别代表所有者，所属组和其他人

即user,grup,others

![截屏2021-06-16 下午3.12.31](/Users/hanwu/Desktop/截屏2021-06-16 下午3.12.31.png)

### umask命令

umask命令用于改变文件的默认权限

默认情况下，目录默认权限为755，普通文件默认权限为644

```
umask ***(三个数字)
```

查看umask:

![image-20210616151657571](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616151657571.png)

umsak预设值为0022

umask代表的含义是：

>   即666（文件）-   umask=实际权限
>
>   777（目录）-      umask   =实际权限

![image-20210616152331934](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616152331934.png)

umask的含义是获得实际权限要用666（文件）或者777（目录）减去的权限

一般情况，root预设的umask权限为0022

一般用户的预设umask权限为002

![image-20210616152624645](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616152624645.png)

![image-20210616152641804](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616152641804.png)

### 更改文件的特殊属性

#### chatter命令

chatter表示为文件增加不同的属性权限

```
chatter [+-=][Asaci][文件或目录名]
```

A：atime将不可更改

s:数据同步写入磁盘

a:只能追加，不能删除

c:自动压缩文件，读取时自动解压

i:不能删除、不能重命名、设定链接、不能写入以及新增数据

常用的为a和i:

![截屏2021-06-16 下午4.30.22](/Users/hanwu/Desktop/截屏2021-06-16 下午4.30.22.png)

#### lsattr命令

lsattr命令用于读取文件或者目录的特殊权限

```
lsattr [-aR] [文件/目录名]
```

“a"：列出隐藏文件

"R"：连同子目录一起列出

![image-20210616164302310](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616164302310.png)

#### set uid,set gid,sticky bit

set uid:使其具有文件所有者权限，（只针对二进制可执行文件）

set gid:使其具有文件所属组权限（目录或文件）

sticky bit:防止删除

![image-20210616165010153](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616165010153.png)

passw，显示的是rws而不是rwx，这个 s就是设置的特殊权限，这里是s--，是4.这里是4755

/tmp/，显示的是rwt,这个t就是设置的特殊权限,--t是1,这里是1777

添加set uid：u+s

添加gid: u+s

添加sticky bit :o+t

s:表示set uid或sset gid

t表示sticky bit

![截屏2021-06-16 下午4.56.36](/Users/hanwu/Desktop/截屏2021-06-16 下午4.56.36.png)

## 在Linyx下搜索文件

### which搜索绝对路径

![image-20210616165857156](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616165857156.png)

### whereis命令查找文件

```
whereis [-bms] [filename]
```

-b:只查找二进制文件

-m:只查找帮助文件

-s：只查找源代码文件

![image-20210616170201716](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616170201716.png)

### locate查找文件

![截屏2021-06-16 下午5.06.09](/Users/hanwu/Desktop/截屏2021-06-16 下午5.06.09.png)

locate命令会找出含有关键字的所有文件

很菜

### find命令查找文件

```
find [路径] [参数]
```

-atime +n/-n:

​	访问时间大于或小于n天的文件

​	atime是在执行或者读取文件时更改的

-ctime +n/-n:

​	写入、修改inode属性的时间大于/小于n天的文件

​	ctime是在对文件进行操作时随inode的内容的更改而更改的

​	inode（索引结点）用来存放档案及目录的基本信息

-mtime +n/-n:

​	写入时间大于/小于n天的文件

​	mtime是在写入文件时更改的

​	

![截屏2021-06-16 下午5.11.11](/Users/hanwu/Desktop/截屏2021-06-16 下午5.11.11.png)

-1表示一天之内

+10表示10天以上

- stat获取文件的atime,ctime,mtime信息

![image-20210616171600765](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616171600765.png)

-name filename：指定文件名查找

-type filetypes:  指定文件类型查找

![image-20210616171740871](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616171740871.png)

![image-20210616171825120](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616171825120.png)

## Linux文件系统简介

Windows格式化磁盘时，会指定格式FAT,NTFS等

而Linux文件系统格式为ext3,ext4,xfs

早期的Linux使用ext2

CentOS7,8默认支持xfs

CentOS6默认支持ext4

### ext3

ext3是直接从ext2发展过来的

带有日志功能，可以追逐记录文件的变化，并把变化写入日志

ext3已经非常稳定，可靠，完全兼容ext2

### ext4

ext4又比ext3性能好很多

最明显的是ext4支持的最大系统文件和当单个文件比ext3大很多

### xfs

xfs支持的量级要比ext4大很多

索引CentOS7,8默认支持它也是必然的

### EeiserFS

是发行版的文件系统

## Linux文件类型

### 常见的文件类型

#### 普通文件--"-"

一般类型文件，可分为二进行和纯二进制

#### 目录

类似于Windows下的文件夹

#### 链接文件----“l"

类似于Windows下的快捷方式

#### 设备

与系统周边相关的一些文件

一种是块设备，如硬盘-----"d"

一种是字符设备,如键盘，鼠标-----"c"

### Linux文件后缀名

#### .sh-----shell脚本

#### tar.gz-----压缩包

#### my.cnf----配置文件

#### test.zip代表一个压缩文件

### Linux的链接文件

两种：硬链接和软链接。二者本质区别在于inode

#### 硬链接

不能垮文件系统，不能链接目录

#### 软链接

原文件的操作和软链接文件没有影响

#### ln命令建立硬链接

```
ln [来源文件] [目的文件]
```

#### ln命令建立软链接

```
ln -s [来源文件] [目的文件]
```

![截屏2021-06-16 下午5.43.02](/Users/hanwu/Desktop/截屏2021-06-16 下午5.43.02.png)

硬链接不会复制数据，额外占用磁盘空间，但是不能选目录做链接

![截屏2021-06-16 下午5.45.29](/Users/hanwu/Desktop/截屏2021-06-16 下午5.45.29.png)

![image-20210616174635125](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616174635125.png)



如果删除的原文件，就无法读取软链接

但是软链接可以简介目录

![image-20210616174926519](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616174926519.png)

# 三、Linux系统用户与用户组管理

## 认识/etc/passwd与/etc/shadow

### /etc/passwd

![image-20210616190401105](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616190401105.png)

![image-20210616190841772](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616190841772.png)

/etc/passwd 可以分成7个字段：

第一个字段

​	用户名

第二个字段

​	存放改用户的扣篮，用x代替，确保安全（加密）

第三个字段

​	uid，用户标识号

​	root 是0,普通用户从1000开始

​	取值范围为0-65535

第四个字段

​	gid,组标识符,普通用户从1000开始

第五个字段

​	注释说明，无意义

第六个字段

​	用户的家目录

第七个字段

​	用户的shell（进程）

​	用户登录后要启动一个进程，用来将用户下达的指令传给内核，这就是shell	

​	Linux的shell有多种，sh,bash,csh,tcsh等

​	CentIOS的进程就是bash

### /etc/shadow

![image-20210616191321609](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616191321609.png)

也是分为9个字段：

第一个字段

​			用户名

第二个字段

​			用户密码，是该用户的真正密码（加密），为防止被窃取，该文件属性为000

![截屏2021-06-16 下午7.15.49](/Users/hanwu/Desktop/截屏2021-06-16 下午7.15.49.png)

第三个字段

​		上次更改密码的时间，用的是时间戳

第四个字段

​		要过多少天才可以更改密码，默认为0，即不受限制

第五个字段

​		密码多少天后到期，即多少天后必须修改密码，默认为99999

第六个字段

​		密码到期前的警告期限，默认为7，即警告密码在7天后到期

第七个字段

​		账户失效期限，过了期限，账户将被锁定

第八个字段

​		账号的生命周期，也是用的时间戳

第九个字段

​		做保留用，无意义

## 用户与用户组管理

### groupadd新增用户组

```
groupadd [-g UID]  groupname
```

![image-20210616192452760](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616192452760.png)

![image-20210616192740274](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616192740274.png)

### groupdel删除用户组

```
groupdel groupname
```

![image-20210616192926525](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616192926525.png)

### userad增加用户

``` 
useradd [-u UID] [-g GID] [-d HOME] [-M] [-s]
```

-u	:设置UID

-g	:设置GID

-d	:设置用户的家目录

-M	:表示不建立家目录

-s	:自定义shell

![image-20210616193413542](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616193413542.png)

### userdel删除用户

```
userdel [-r] username
```

r	:删除用户是一并删除用户的家目录

![image-20210616194112593](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616194112593.png)

## 用户密码管理

### passwd命令

passwd用于设置密码

```
passwd [username]
```

![image-20210616194320247](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616194320247.png)

改登录的用户密码，可以不用输用户名

![image-20210616194408832](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616194408832.png)

### mkpasswd

mkpasswd用于生成密码

不过在使用前要安装一个expect包

```
yum install expect
```

```
mkpasswd
```



## 用户身份切换

先创建一个用户

```
useradd test
```

并且设置密码

```
passwd test
```



### su命令

```
su [-] username
```

加上-,相当于登录改用户

![截屏2021-06-16 下午7.52.39](/Users/hanwu/Desktop/截屏2021-06-16 下午7.52.39.png)

### sudo命令

默认情况下，只有toor用户才能使用sudo命令

普通用户要想使用root命令，要预先设定

使用visudo编辑相关的配置文件/etc/sudoers

先要安装visudo

```
yum install -y sudo
```

然后启动visudo

```
visudo
```

![image-20210616200922002](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616200922002.png)



用"/all"找到这一行在，在下面加入一行,让test拥有sudo权限

test    		ALL=(ALL)	ALL



![image-20210616201120288](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616201120288.png)



和vim的编辑方式一样

从左到右边：

第一段：

test表示一个用户1，用于指定哪个用户拥有test权限

第二段：

左边的ALL是指所有主机，右边的ALL是指获取那个用户的身份

第三段：

设定用户可以使用的sudo命令有哪些

验证test是否具有sudo权限:

![image-20210616201355014](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616201355014.png)

为了避免每增加一个用户就写入一行

可以这样设置:

把%wheel ALL=(ALL)ALL前面的#去掉

即让wheel这个组的所有用户都获取sudo权限

接下来只需要把想要sudo权限的用户加入wheel中即可

![image-20210616201745753](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616201745753.png)



#### 让用户不输入密码就能切换到root用户：

进入

```
visudo
```

在文件的最后写入3行代码：

```
User_Alias USER_SU = test, test1, han
Cmnd_Alias SU =/usr/bin/su
USER_SU ALL=(ALL) NOPASSWD: SU
```



第一行设置一个user别名

第二行设置一个命令别名

第三行让他们获取权限





### 不允许远程登录Linux

修改配置文件/etc/ssh/sshd_config

在文件中查找PermitRootLogin yes

并修改为PermitRootLogin no

保存文件后，重启sshd服务

```
systemctl restart sshd.service
```





# 四、磁盘管理

## 查看磁盘或者命令的容量

### df命令

df(disk filesystem)用于查看已挂载的磁盘的总容量、使用容量、剩余容量等，默认以KB为单位

```
df
```

![image-20210616203954499](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616203954499.png)

其中

/,/boot是在安装i系统时划分出来的

/dev,/dev/shm为内存分区，大小默认为内存大小的一半



i：

​	表示查看inode的使用状况

![截屏2021-06-16 下午8.45.30](/Users/hanwu/Desktop/截屏2021-06-16 下午8.45.30.png)	

h:

​	表示使用合适的单位显示数据

​	![image-20210616204621977](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616204621977.png)

k,m:

​	分别表示以KB,MB斯安叔数据单位

![image-20210616204652766](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616204652766.png)



最后一列为挂载点



### du命令

du(disk message)命令用于查看某个目录或文件所占的大小

```
du [abckmsh] [文件或目录名]
```

a:

​	列出所有的文件和目录

![image-20210616205205934](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616205205934.png)

b:	

​	已B为单位输出

![image-20210616205231832](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616205231832.png)

k:

​	以kb为单位输出

![image-20210616205251419](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616205251419.png)

m:

​	以mb为单位输出

![image-20210616205314688](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616205314688.png)

h:

​	由系统自动调节输出单位

![image-20210616205334422](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616205334422.png)

c：

​	最近进行总加

![image-20210616205354782](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616205354782.png)

s:

​	只列出总和

![image-20210616205415096](/Users/hanwu/Library/Application Support/typora-user-images/image-20210616205415096.png)

## 磁盘的分区和格式化

### 增加虚拟磁盘

### fdisk命令

## 格式化磁盘分区

### mkfs命令

### e2label命令

## 挂载/卸载磁盘

## mount命令

## /etc/fatab配置文件

## blkid命令

### umount命令

## 建立一个swap文件增加虚拟内存