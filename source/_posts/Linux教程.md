---
title: Linux笔记
date: 2021-06-05 19:26:47
tag: 操作系统
---

## Shell基础

### Shell脚本通配符

```shell
*	*python*	匹配任意含有python的字符
?	linu?	匹配linu+任意字符
[]	[123456]	匹配1,2,3,4,5,6中的任意字符
..	表示字符之间，可以是英语字符也可以是数字	a..e	表示a,b,c,d,e		1..5	表示1,2,3,4,5
{}	表示一个范围	如{1..10}表示1，2，3，4，5，6，7，8，9，10，touch {1..10}.txt,则会创建1.txt，2.txt，...，10.txt十个文件
!	表示排除	[!abc]	表示不是a,b,c之中的任意字符
^	在一行的开头匹配字符串	^b	表示那些以b开头的字符串的那些行
$	在一行的末尾匹配字符串	ubuntu$	表示以ubuntu结尾的字符串
'' ""	包含字符串解释为普通字符串，包括空格、/、\、$ 等特殊符号
``	反引号包含的字符串解释为命令
#	表示注释
$	表示取变量值
$#	统计个数
\	转义字符
|	分隔两个管道命令，表示或者
;	分隔多个命令，多个命令依次执行
&	a&b	a,b同时运行	a&	表示后台执行a命令
&&	连接多个命令	a&&b	a执行成功后运行b
```

### Shell中的变量

```shell
#用户变量
变量名=变量值	#注意=两边不要有多余空格，如果变量值中有空格或者Tab则需要使用''或""

#环境变量
在终端中输入echo $环境变量名，显示当前的环境变量值
	#如：echo $(data)
```

### Shell脚本

```shell
#if语句
if 条件
then
	语句
elif 条件
then
	语句
else
	语句
fi

#case语句，类似switch
case 表达方式 in
	模式1)
		语句
		;;
	模式2)
		语句
		;;
	...
	*)
		语句
esac

#for循环结构
## 第一种
for 变量名 in 变量列表 #变量列表可以用{1..10}这样的形式，类似python中的range(0,10)
do
	语句
done
## 第二种
for (变量名=初值;变量名<=终值;变量名++)
do
	语句
done

#while语句
while 条件
do
	语句
done

#until 条件
do
	语句
done
```

### Shell脚本的运行

```shell
bash 或者 sh	脚本名称.sh	运行一个Shell脚本
```

### Shell帮助

```shell
help 命令名	#查看shell内置命令的帮助信息
which 命令名	#查看外部命令的路径
man 命令名		#查看外部命令的帮助信息
xargs 占位参数命令 #xargs -I {} 声明{}是一个占位符，如：cat 1.txt | xargs -I {} cp {} test	这里的{}就是占位符，读取出1.txt中的内容会放到cp {} test这段的{}位置替换
```

## 文件管理

### 常见文件夹

```shell
/home	#用户的主目录，每一个用户名下的文件夹中对应着其的个人文件
/etc	#系统的配置文件，一些软件的配置文件
/var	#杂烩文件夹，存放一些日志信息，登录信息，授权信息，权限机制
/usr	#User Resource用户资源文件夹，存放用户程序和文件，类似Windows的program files
/dev	#Device设备文件夹，存放外部设备，用户挂载的U盘等
/media	#媒体文件夹，系统将自动识别的设备，如光驱等，默认挂载到/media/对应用户名 文件目录下面
/mnt	#用户临时挂载的其他文件系统，如网盘
/bin	#Binary文件夹，存放系统命令和绝大部分应用程序的可执行文件
/proc	#虚拟文件目录，是系统内存的映射，可通过它来访问一些系统信息，如/proc/version 查看内核信息
/boot	#启动Linux的一些核心文件，一些链接文件和镜像文件
/srv	#一些服务启动后需要提取的数据，如FTP服务器软件会创建/srv/ftp文件夹，并存放一些数据
/lib	#系统的动态链接库文件夹，类型Windows中的DLL文件夹
/root	#系统管理员的主目录
/sbin	#系统管理员所使用的系统管理程序
```

### 文件管理中常使用的命令

```shell
tree	显示目录树形结构
	-d 显示目录名称而非内容	
	-D 列出文件或目录的更改时间		
	-L Level，显示到多少层		
	-f 在每个文件或目录之前，显示完整的相对路径
	-F 执行文件后面加上*，目录后面加上/，软连接后面加上@，用于区分结果类型

pwd	#print working directory	打印当前工作目录的绝对路径

ls	列出目录和文件
	-F 同tree的F参数效果一致
	-l 即long listing format，相当于显示详细信息
	-a 即all,显示所有文件和目录（以.开头的文件为隐藏文件）
	-h 即humman-readable，人性化阅读方式，使用适当的单位显示文件大小
	-d 即dirctory，仅显示目录，不显示内容
	-R 即recursive，递归列出子目录

cd	改变当前的工作路径

gedit	用图形化界面编辑文件，类似Windows的notepad

>、>>	输出重定向	>表示覆盖文件，>>表示追加
<、<<	输入重定向

find	文件查找
	-perm 按文件权限
	-name 按文件名称
	-size 按文件大小，+100M表示大于100MB的文件，+100k表示大于100kB的文件，-100k表示小于100kB的文件，-100c表示小于100B的文件
	-type 按文件类型，f表示文件，d表示目录
	-mtime 按修改时间，以天为单位，-1表示修改时间小于一天，+1表示修改时间大于1天
	#用法：find 目录 参数选项 查找值
	#例子：find ~ -perm 444 -type d 在~目录下查找出所有权限是444的目录
	
mkdir	创建文件目录（文件夹）
	-p	递归创建文件夹	如：mkdir 333/222/111	创建了333文件夹，里面是222文件夹，里面是111文件夹
rmdir	删除空文件目录（文件夹），必须是里面没有内容的空文件夹才行
rm	#重量级删除文件或文件夹命令
	-r 即recursive，递归删除目录下的子目录和文件
	-f 即force，强制删除且不询问是否删除
	-i 即interactive，交互，每个要删除的文件都会询问是否删除
	
cp	复制文件和目录
	-r 即recursive，递归复制子目录和子文件
	-f 即force，强制复制粘贴，如果目标文件或目录存在，不提示，直接覆盖
	-i 即interactive，询问是否覆盖
	-b 即backup，备份，如果目标文件或者目录已经存在，则在同名情况下名字后面加上~
	-t 指定复制到的路径

mv	移动或重命名文件和目录
	-f 强制移动，直接覆盖
	-i 询问是否覆盖
	-b 同名时名字后面加上~
	-t 指定移动目录，在移动多个文件或目录时使用，-t 后要紧跟指定移动到的目录，如：mv a b c.txt -t d 将a目录,b目录,c.txt移动到d目录中
	
ln	文件链接，包括软链接和硬链接，软链接相当于Windows中的快捷方式，硬链接相当于多一个文件名指向同一块内存空间，目录无法创建硬链接，删除源文件后软链接失效，硬链接仍然拥有源文件的数据
	#用法：
	创建软链接: ln -s 目标文件路径 软链接的名字
	创建硬链接: ln 目标文件路径 硬链接的名字
```

### 文件内容查看与分析命令

```shell
touch	创建空文件

cat		查看文件，合并文件：cat a b > c	将a,b文件用覆盖方式合并到c文件中

#分页显示文件内容
more	分页显示文件内容基础班，Q键退出，空格键向下翻页，回车键向下移动一行
less	more的升级版，在more的基础上，增加了B键向上翻页，方向键移动页面

head	显示文件开头的内容
	-n 接数字，表示显示开头的多少行

tail	显示文件末尾的内容
	-n 同head的-n参数

echo	标准输出，常用来查看变量值，或{}表示的范围，如：echo {1..10}终端会显示1，2，3，4，5，6，7，8，9，10

awk		文本分析命令
	-F 指定分隔符，可以是一个字符串或一个正则表达式
	#用法
	awk '{[正则表达式] 动作}'文件名
	#例子：
	awk '/要查找的内容/' 1.txt 如：awk '/12/' 1.txt 查找1.txt中12所在的行
	awk -F '' '$1 ~ /1/' 1.txt 在1.txt中查找第一段为1的行
	awk -F ' ' '{print $4}' /var/log/auth.log | tail -n3 以空格作为分割，打印前第4段内容，$0表示打印全部段，使用管道将输出作为tail的输入，打印最后3行
	awk -F '' '{print $1}' 1.txt 对于每一行，每一个字符都会被拆成一段，打印每行
	awk -F '' '{print $1"*"$2}' 1.txt 打印前两端并用*号分割，注意用""，否则会和''混淆
	#注意：awk中的$1这种相当于变量，可以用于关系运算符的，如：awk -F '' '$1>$2' 1.txt 打印1.txt中第一段大于第二段的行


sort	对文件内容进行排序（默认为升序排序），按每行的第一个字符排序，第一个字符相同，则比较第二个字符，以此类推
	-r 降序排列，也可以叫反转顺序
	-u 去重排列
	-o 输出排序结果到指定文件中
	-h 按大小顺序排列，从小到大（不是按数字第一位比较排列） 
	#例子：du -h . | sort -h 按文件的大小，从小到大排序

grep	文件内容查找
	-i 即ignore，忽略字符大小写

meld	文本差异比较软件，带GUI界面

wc		统计文件内容
	-l 只显示行数
	-w 只显示字数（单词数）
	-c 只显示字节数
```

### 文件和目录的权限管理

```shell
使用ll或者ls -l来查看文件的权限情况，一般是三组值对应三类用户，第一个组是文件所有者权限值，第二个组是文件所属组权限值，第三个是其他用户权限值
如：rwxrw-rw-，rwx是文件所有者的权限情况，rw-是文件所属组的权限情况，最后一个rw-是其他用户的权限情况

#字母表示法
读取（r），写入（w），执行（x），对于目录来说，执行权限就是用户能否进入目录，读取权限就是用户能否列出目录中的内容，写入权限就是用户能否在该目录下操作子文件和子目录
#数字表示法
权限代号rwx，用3bit表示，
第一位为执行权限：--x，表示001
第二位为写入权限：-w-，表示010
第三位为读取权限：r--，表示100
把每一位上的值加和就是权限情况，如：rwx=111即7，具有读取，写入，执行权限，0表示不具有任何权限

chmod	更改文件权限
	u 即user，文件所有者
	g 即group，文件所属组
	o 即other，其他用户
	a 即all，所有用户
	+ 增加权限，默认为所有用户
	- 减少权限，默认为所有用户
	-R 递归设置目录的权限，包括子目录和子文件
	r 读取权限
	w 写入权限
	x 执行权限
	#列子：
	chmod 777 1.txt 1.txt文件所有用户获取读取，写入，执行权限；chmod 644 1.txt 1.txt所有者获取读取，写入权限，所属组和其他用户获取读取权限
	chmod a+x 1.txt 1.txt文件所有用户都获取执行权限；chmod a+rwx 1.txt 1.txt文件所有用户都获取读取，写入，执行权限；chmod og-x 1.txt 文件所属组和其他用户都去掉执行权限

chown	修改文件所有权，通常需要超级管理员权限
	-R 即recursive，
	#用法：chown 用户:用户组 文件名或目录名
	#例子：
	用ls -l 文件名或目录名 查看对应文件或目录的所有者和所属组
	sudo chown root:root 1.txt 将1.txt文件的所有者改为root，所属组改为root组

chgrp	修改文件所属组，通常需要超级管理员权限
	#用法：chgrp 用户组名 文件名
	
umask	权限掩码，相当于预设权限，如目录的默认权限为777，文件的默认权限为666，权限掩码一般默认为0002，则在该目录下创建一个目录A，则目录A的权限为777-002=775，创建一个文件B，则文件B的权限为666-002=664
	#用法：umask 三位八进制数	即可设置权限掩码，如：umask 022 则权限掩码设置为022
```

### 文件压缩与解压

```shell
gzip	将文件压缩成一个.gz压缩文件
	-d 即decompress，解压文件，不带参数d就是压缩
	-f 即force，强制解压文件，
	-r 即recursive，递归处理，将该目录下的文件和目录同样处理
	-t 即test，测试压缩文件是否正确
	-v 即verbose，显示执行过程，相当于显示详细信息
	-数字 数字是介于1-9之间的值，表示压缩率，-9最佳压缩率，-1最快/最低压缩率
	-h 显示简洁的帮助提示
	#用法：gzip 参数 文件名

bzip2	将文件压缩成一个.bz2的压缩文件，压缩效果比gzip好些
	#参数通gzip

tar		归档压缩文件，即tape archive，将文件或目录打包成一个.tar文件，然后再使用gzip或者bzip2对.tar文件进行压缩
	-c 即--create，创建新的归档文件
	-x 即--extract，解压归档文件
	-v 即--verbose，显示执行过程
	-t 即--list，查看文件内容
	-r 即--append，追加文件到归档文件中
	-u 即--update，更新原压缩包中的文件
	-f 即--file，该参数需要归档文件名
	-z 使用gzip压缩
	-j 使用bzip2压缩，效率更高，压缩包更小
	-P 即--preserve-permission，保留文件权限
	-C 即--directory，将当前目录改变为指定目录，相当于指定一个解压路径
	#用法：tar 参数 归档文件名 文件或目录
	#例子：
	tar -cvjf 1.tar 1.txt 将1.txt归档并压缩为1.tar，注意f参数必须在参数的最后一个位置
	tar -xvjf 1.tar -C test	将1.tar解压到test目录中
	#注意：用bzip2压缩的只能用bzip2解压，用gzip解压会报错
	
zip		压缩
	-r 即--recurse-paths，递归压缩
	-o 即oldest，更新压缩文件至最新
	-d 即delete，删除指定文件
	-P 即password，设置密码，大小写敏感
	#用法：zip 参数 压缩后文件名 文件或目录
	#例子：zip -P 123 1.zip 1.txt 将1.txt压缩为1.zip，并设置密码为123
	#注意：压缩时，如果是对目录进行压缩则需要加上-r参数否则会失败，设置密码时P参数放在最后

unzip	解压
	-o 即overwrite，不提示覆盖文件
	-d 指定解压目录
	-P 使用解压密码
	#用法：unzip 参数 解压到的目录 压缩文件名	解压到的目录不填则默认为当前路径，若路径不存在则会自动创建目录
	#例子：unzip -od test -P 123 1.zip 使用123解压1.zip到test目录中
	#注意：压缩文件名要放在最后面

rar		压缩和解压
	-r 即recurse，递归压缩子文件和目录
	-w 即work，指定工作目录
	-p 即password，设置密码，大小写敏感
	a 即add，加入压缩包，当指定压缩包存在时，为追加到压缩包中，且可以单独给某个文件设置密码
	e 即extract，提取压缩文件到当前路径，但不创建子目录
	x 即extract，从压缩包中提取文件，包含全路径
	d 即delete，从压缩包中删除文件
	#用法：rar 参数 压缩后文件名 文件或目录
	#例子：rar a 1.rar 1.txt 将1.txt压缩为1.rar	rar a 1.rar 2.txt -p 将2.txt以追加的方式添加到1.rar中，且为压缩包中的2.txt设置密码
	#例子：rar x 1.rar	将1.rar解压到当前目录下	rar x 1.rar -w test 将1.rar解压到test中，注意test需要提前创建
```

## 用户和用户组管理

### 用户和组

```shell
UID：用户ID，UID为0表示超级用户，1-999为系统预留给系统虚拟用户的ID，超级用户创建的用户ID从1000开始

GID：组ID，Linux中的组有基本组（私有组）和附加组（公开组），一个用户只能属于一个私有组，但可以加入多个公开组，创建用户时默认创建同名的组

root：超级用户，也称根用户，UID为0，在系统中拥有最高权限，可以操作任何文件，执行任何命令，默认情况下只能在本地登录，一般不运行直接使用超级用户登录系统

虚拟用户：即程序用户，UID为1-999，默认情况不能登录系统，与进程相关，如daemon、bin、sys、mail、ftp都是虚拟用户，daemon用户由系统的守护进程创建

普通用户：UID为1000开始，由系统管理员创建，可以登录系统，能够管理自身文件，可以临时使用超级用户权限

#用户和组的配置文件
/etc/passwd	用户账户信息文件
	#格式：每一行都是一个用户的账号信息，包括7个字段，用":"分隔，即用户名:密码域:UID:GID:注释信息:主目录:命令解释器，密码域为x表示密码已经被映射到影子文件中
/etc/shadow	用户密码影子文件
	#格式：每一行都是一个用户的密码设置信息，包括9个字段，用":"分隔，即用户名:加密后的密码:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:保留字段，加密后的密码由三个部分组成，用"$"分隔，即$加密算法序号$加盐值$加密后的密码，0表示DES，1表示MD5哈希算法，2表示Blowfish，5表示SHA-256哈希算法，6表示SHA-512哈希算法，如果密码是!则表示没有设置密码，如果是*表示不会用该用户登录，
	#注意：千万别试图在shadow文件中用户的密码前面加上"!"来试图绕过密码，可能会导致无法登录！！！
/etc/group	组账户信息
	#格式：每一行都是一个组的账户信息，包括4个字段，用":"分隔，即组名:组密码域:GID:成员清单
/etc/gshadow	组密码影子文件
	#格式：每一个行都是一个组的密码，包括4个字段，用":"分隔，即组名:组密码:GID:成员清单，密码解释同/etc/shadow文件
```

### 用户管理

```shell
id	查看用户账户信息
	#用法：id 用户名

sudo 使用超级管理员权限，一般要求输入管理员密码
	#用法：sudo 用使用的命令

su	切换用户
	#用法：su 用户名	且换到对应的用户下，需要输入密码

exit 退出
	#用法：exit	退回到原用户，或者直接退出登录，退出终端

useradd	添加用户
	-m 如果用户主目录不存在则自动创建，理解成使用了mkdir来创建对应用户的主目录
	-s 指定用户登录的Shell环境，默认为/bin/sh
	-d 指定用户登录的主目录
	-u 指定用户账户的UID，必须唯一
	-r 创建一个系统账户
	-p 即password指定用户密码，使用该参数，其他用户可见
	-c 即comment，指定备注信息，使用引号将其括起来，保存在/etc/passwd文件的备注栏上
	-e 即expired，指定失效时间，默认为永久有效，格式为MM\DD\YY
	-f 指定不活动时间，即密码过期后，账户被彻底禁用前的天数，0表示立即禁用，-1表示禁用这个功能
	-G 指定用户所属的基本组或GID，但该组必须存在
	-g 指定用户所属的附庸组
	#注意：一般要使用到管理员权限
	#用法：useradd 参数 参数值 用户名

passwd	设置用户密码
	-d 即delete，删除指定用户密码
	-e 即expire，强制指定用户密码过期
	-l 即lock，锁定用户账户
	-u 即unlock，解锁指定用户账户
	#用法：passwd 参数 参数值 用户名，没有参数表示给用户创建或更改密码

usermod	修改用户属性
	-L 即lock，锁定用户
	-U 即unlock，解锁用户
	-G 附加组名
	-a 即append，将用户附加到附加组中，需要和-G参数配合使用
	-u 修改指定用户UID
	-d 修改用户登录的主目录，紧跟指定的主目录路径
	-l 即login，修改用户名 -l 新用户名 原用户名
	#用法：sudo usermod 参数 参数值 用户名
	#例子：sudo usermod -aG sudo user1 将user1添加到sudo组中，使其可以使用sudo获取超级管理员权限

chage	修改用户密码有效期
	-l 即list，列出密码有效期信息
	-m 两次修改密码的最小间隔天数，0表示随时都可以修改
	-M 密码有效期的最大天数
	-E 指定密码过期时间，0表示立刻过期，-1表示永不过期
	#用法：chage 参数 参数值 用户名

userdel	删除用户
	-r 即recursive，递归删除用户主目录下的文件和目录
	-f 强制删除，即使用户当前登录了系统
	
groupadd 添加用户组
	-g 指定用户组GID
	-f 如果GID已经被占用，则取消创建，如果组已经存在则该参数失效
	-r 创建系统用户组，GID小于1000
	#用法：同useradd

groupmod 修改用户组属性
	-g 修改GID
	-n 即name，修改组名
	#用法：同usermod
	
gpasswd	设置用户组密码
	-a 即append，添加用户到用户组，紧跟用户名
	-d 即delete，删除用户组中的用户，紧跟用户名
	-r 删除用户组密码
	-A 指定用户组管理员，可以任命其他组的用户作为本组的管理员
	-M 指定组成员
	#用法：gpasswd 参数 参数值 用户组名 没有参数时意味着给该组设置密码或修改密码
	
groupdel 删除用户组
	#用法:groupdel 用户组名

chpasswd	批量修改用户密码
	#从系统的标准输入读入用户名和密码，对已经存在的用户修改其密码，可使用管道符
	#例子：echo 用户名:密码 | chpasswd
	#批量添加用户的shell脚本
	#!/bin/bash
	for i in {1..10}
	do
		useradd user$i -g testgroup
		echo user$i:123456 | chpasswd
	done
	#批量删除用户的shell脚本
	#!/bin/bash
	for i in {1..10}
	do
		userdel user$i
	done
	
#其他
1.使用awk查看非系统用户：sudo awk -F ':' '$3>=1000' /etc/passwd
2.直接修改/etc/passwd、/etc/shadow、/etc/group文件中对应的用户名，就可以达到修改某个用户名的目的，（usermod -l只是修改的登录用户名）
3.修改用户主目录直接使用usermod -d即可
4.john破解系统用户密码（一般密码复杂一点就没法破解）
```

## 进程管理

### 进程概述

```shell
OS基础：
一，进程概念
	1.进程是程序的一次执行
	2.进程是一个程序及其数据在处理机上顺序执行时所发生的活动
	3.进程是具有独立功能的程序在一个数据集合上运行的过程，它是系统进行资源分配和调度的一个独立单位（有线程的系统中进程是资源分配的单位，线程是系统调度的单位）
二，进程状态
	1.就绪状态，除cpu外，其他资源分配完成，获取cpu即可运行
	2.运行状态，正在处理机上运行
	3.阻塞状态，又称睡眠状态，等待状态
三，进程和程序的区别
	1.进程是一个动态的概念，程序是一个静态的概念。程序是指令的有序集合，没有执行含义。进程强调执行过程，动态地创建，并被调度执行后消亡。
	2.进程具有并行特征（并发性、独立性、异步性），程序没有
	3.不同进程可以包含同一程序，只要对应的数据集不同。同一程序执行过程中也可以产生多个进程
四，进程优先级
	Linux中进程优先级为一个整数，且优先级高者优先
五，进程层次
	Linux中运行进程创建进程，创建进程的进程为父进程，被创建的为子进程，依次类推，形成进程“家族”
```

### 进程状态查看

```shell
ps	即process status，查看进程状态
	a 即all，显示一个终端的所有进程信息
	-A 同上
	-a 显示同一终端下的所有程序
	-u 即user，显示指定用户的进程信息
	-x 显示没有控制终端的进程信息
	-e 显示所有进程
	-f 即format，使用全格式列表显示进程详细信息
	-l 即long format，使用长格式列表显示进程详细信息
	#注意：ps命令参数带-和不带输出有时有区别
	#输出字段含义：
		F	即flag，标志
		S	有时是STAT，最小状态表示（单字符），D：不可中断，R：运行，S：中断（阻塞），T：停止，Z：僵死（僵尸进程）
		UID	进程创建者的UID或名字
		PID	即Process ID，进程ID号
		PPID	即Parent Process ID，父进程ID号
		%CPU	即CPU占比
		%MEM	即内存占比
		RSS		内存中页（一页4KB）的数量
		VSZ		占用虚拟内存大小
		PRI	即Privilege，进程优先级
		STIME 	即Start time，进程开始执行的时间
		NI	即niceness值，表示进程优先级的修正值
		ADDR	
		SZ	内存大小
		WCHAN	睡眠状态进程的核函数地址，运行状态的进程将显示“-”
		TTY		进程所在终端的设备号
		TIME	进程已经运行的时间
		CMD		运行此进程的命令名、进程名称
		#注：PRI（new） = PRI（old）+ NI
	#用法：ps 参数 参数值 [参数]
	#例子：
		1.查看所有进程信息: ps -aux 显示字段最多, ps -ef 显示字段其次, ps -e显示字段最少
		2.查看某一个用户的进程信息: ps -u root 显示简洁, ps -u root -l 显示详细，也可以使用ps -aux | grep root 这样搭配管道符匹配出root用户的进程

pgrep	查看指定进程的PID
	#用法：pgrep 进程名
	#例子：pgrep sshd	查看SSH服务器的守护进程sshd，也可以使用ps -aux | grep sshd 或者 sudo ss -antp | grep sshd	查看sshd进程信息

uptime 系统平均负载统计命令
	-p 即petty，用优美的格式显示（也没好看多少）
	#用法：uptime 参数
	
top 动态实时监控进程
	-d 指定自动刷新的时间间隔，单位秒，默认3秒
	-p 即PID，仅监控用PID指定的进程
	-u 即user，仅监控指定用户的进程
	-b 批处理
	-c 即complete，显示进程的完整名称
	#用法：top [参数 [参数值]]
	#例子
		1. top 实时监控所有进程刷新时间3s
		2. top -d 10 实时监控，并设置刷新时间为10s
	#注意：进入实时监控后，上下键可以移动，PgDn和PgUp可翻页，按Q离开
	#显示详情
		第一行，系统信息：系统时间、up 当前系统持续运行时间、当前登录用户数目、load average表示每1min、5min、15min系统的负载大小
		第二行，进程信息：系统总进程数目、当前运行进程数目、当前处于睡眠状态的进程数目、当前处于终止状态的进程数目、当前的僵尸进程数目
		第三行，CPU状态信息：us为用户空间占用CPU的百分比、sy表示内核空间、ni表示改变过优先级的进程、id表示空闲CPU、wa表示IO等待、hi表示硬件中断、si表示软中断、st表示CPU使用内部虚拟机运行任务的时间
		第四行，内存信息：物理内存总大小、空闲内存大小、已占用内存大小、缓存的内存总量（单位为：MB）
		第五行，交换区信息：交换区总大小、空闲交换区大小、已占用交换区大小、缓存的交换区总量（单位为：MB）
		第六行，空
		第七行，各进程状态信息字段：进程号、所属用户、PR进程优先级、NI niceness值、VIRT进程使用的虚拟内存总量（单位为：KB）、RES进程使用的未被交换的物理内存大小（单位为：KB）、SHR共享内存大小（单位为：KB）、TIME+进程使用的CPU时间总计（单位为：1/100s）、CMD运行此进程的命令名或进程名称
		
pstree	查看进程树
	-a 显示进程的命令行参数
	-p 显示进程的线程PID
	-s 显示所有父进程
	-n 根据进程ID排序，默认是根据进程名排序
	-h 即highlight，高亮显示当前进程和所有父进程
	-l 长格式显示
	-g 显示进程组ID号
	-t 隐藏线程，只显示进程
	#用法：pstree 参数 参数值	参数值可以是PID，不写则会显示所有的进程组成的进程树
```

### 进程状态控制

```shell
& 后台启动进程，一般用于命令要运行一会，但当前不想因为命令的执行阻塞终端时，使用后，终端不会被阻塞，命令仍然在执行
	#用法：命令 参数 参数值 &

nice	即niceness，用于调整进程优先级，范围是-20~19，-20为最高优先级，19为最低优先级，程序默认的PRI为80，niceness值继承于其父进程，默认niceness值为0
	-n 指定运行时的niceness值
	#例子：sudo nice -n -20 命令 
	#注意：nice命令需要使用超级管理员权限

renice	调整运行进程的优先级
	-n	指定数值（可要可不要，直接写要改变的优先级就行）
	#用法：sudo renice [参数] 优先级数值 PID

kill	终止进程
	-9	强制终止
	#用法：kill [参数] 进程PID

killall	指定进程名终止进程
	-I 即Ignore，忽略大小写
	-i 即interface，交互模式，终止进程前询问用户
	-e 进程名精确匹配
	-u 终止指定用户名的所有进程
	-9 强制终止进程
	-s 发送信号
	-r 采用正则表达式
	#用法：killall [参数 [参数值]] 进程名/用户名

time	统计进程运行时间，在执行的命令前面加上time，当执行结束时，会显示一共执行了多少时间
	#用法：time 命令名/运行程序名

nohup	将进程脱离终端运行，常常和&后台命令配合使用，nohup会自动将命令的输出重定向到nohup.out中
	#用法：nohup 命令 &

	
```

### 任务查看与管理

```shell
jobs	查看任务状态，一般是使用&的后台进程和使用Ctrl+Z挂起的进程
	-l 列出所有活动任务
	#用法：jobs -l
	#如：
	makima@makima-ubuntu-pc:~$ jobs -l
	[1]+  6024 停止                  ping www.baidu.com		
	#注意：[1]中的1就是该任务的序号，该序号在fg和bg中使用
	#注意：后台进程无法被挂起

fg		通过指定的任务序号将任务移到前台执行
	#用法：fg 任务序号
	#注意：是任务序号不是进程号PID

bg		通过指定的任务序号将任务（当前被挂起的进程）移到后台执行
	#用法：bg 任务序号
	#注意：是任务序号不是进程号PID，如果进程有输出且输出没有被重定向，则还是会输出到当前终端

fuser	列出进程和任务使用的本地或者远程文件的进程号，相当于查看当前进程都占用了哪些文件，一般用来查看哪些进程在占用某个tcp或者udp端口
	-a 显示所有未使用的文件
	-k 终止访问指定文件的所有进程
	-i 终止前询问用户
	-l 列出所有已知的信号名称
	-m 即mount，挂载点
	-n 命名空间，常指网络协议
	-u 在每一个进程后显示所属的用户名
	-v 即verbose，显示执行过程
	#用法：fuser 参数 参数值 文件名/设备名/网络套接字
	#例子：sudo fuser -vn tcp 22 显示tcp的22端口的使用详情

at		定时任务，一次性执行，按Ctrl+D结束at命令的输入状态
	#用法:
		1.指定具体日期：格式为：HH:MM yyyy-mm-dd 或mm/dd/yy或dd.mm.yy 如：at 08:00 2023-03-06 或 at 08:00 03/06/23 或 at 08:00 06.03.23
		2.当天24h制，格式为：HH:MM，如果当前时间已经过去，则推迟到明天那时 如：at 12:25
		3.当天12h制，格式为：HH:MMAM或HH:MMPM 如：at 8:00AM 或 at 9:00PM
		4.相对时间，格式为：now +n minutes或hours或days 如：at now +2 minutes 
	#注意：在输入状态时功能符号得是英文状态，如()、""、''这些
	#其他
		1.atq 查看当前已有的定时任务
		2.atrm 序号 按序号删除当前的定时任务

crontab	设置周期任务，crond是Linux系统执行周期性任务的守护进程，它会每分钟检查是否有需要运行的任务
	-l 显示某个用户的crontab文件内容，如果不指定则默显示当前用户的crontab
	-u 指定用户来设置crontab服务
	-i 删除用户的crontab时，询问
	-r 从/var/spool/cron目录中删除某个用户的crontab文件，如果不指定用户则默认删除当前用户的
	-e 编辑用户的crontab，默认打开当前用户
	#字段含义：每一段用空格隔开，一行代表一个任务，*表示全选，如m位置写*则表示符合要求的每一分钟都要执行，*/n表示将其每n个分为一组执行一次，相当于步长为n
		m	分钟，0~59的整数
		h	小时，0~23的整数
		dom	表示日期，0~31的整数
		mon	表示月份，1~12的整数
		dow	表示星期几，0~7的整数，0或7代表星期日
		command	需要执行的命令，可以是系统命令也可以是脚本，路径必须为绝对路径
	#用法：
		1.crontab -e 打开并编辑当前用户的crontab文件
		2.sudo service cron reload && sudo service cron rstart 重新加载用户的crontab文件，并重启cron服务
		3.sudo service cron stop 终止cron服务
	#例子：
		0 */3 * * * ps -ef | grep java | grep -v grep | awk '{print $2}' | xargs kill -9 && cd ~/service_Run && java -jar awtrix.jar 
		#解释：每隔3小时，执行一次终止java进程，并重新运行awtrix.jar
```

### 特别

```shell
screen	创建视窗（screen终端）使命令或进程在关闭本终端后仍然可以继续执行，且下次登录时可以回到之前创建的视窗
    -list	列出当前创建的screen终端信息
    -ls		同-list
    -h<行数> 指定screen终端的缓冲区行数
    -x		恢复之前离线的screen终端
    -m		即使目前已在screen终端中，仍强制建立新的screen终端
    -p 		windows	如果命名窗口存在，则预选该窗口
    -r 		重新连接到指定的screen终端，紧跟screen_id
    -R		先试图恢复离线的screen终端，若找不到，则建立新的screen终端
    -t 		即title，设置标题（窗口名称）
    -U		screen终端使用UTF-8编码
    -wipe 　检查目前所有的screen终端，并删除已经无法使用的screen终端
    -v		打印screen版本信息，如：“Screen version 4.08.00 (GNU) 05-Feb-20”
    #用法：screen 参数 参数值
    #例子：
    	1.screen vim test.txt 创建screen终端，并执行vim test.txt
    	2.screen -ls 列出当前系统中存在的screen终端，每段第一组数字就是screen_id
    	3.screen -r screen_id 恢复到指定screen_id的screen终端中
    	4.screen	直接创建一个新的screen终端
```



## 磁盘管理

### 磁盘基础概念

```shell
#Linux中一切皆文件
	1.设备文件：Linux将硬件设备看作一个文件，称为设备文件。内核为每一个硬件设备都在/dev目录下创建对应的设备文件，设备文件关联驱动程序，通过访问设备文件就可以访问对应的设备
	2.设备标志：设备类型、主设备号、次设备号。主设备号与驱动程序一一对应，次设备号用来区分同一类型的不同设备
	3.设备分类：字符（char）设备，块（block）设备。OS基础：字符设备按字符方式顺序访问（如：打印机），块设备按块方式随机访问（如：硬盘，U盘）

#磁盘分区表
	1.磁盘分区：系统对磁盘物理设备的逻辑划分
	2.创建文件系统流程：分区->格式化（建立文件系统）->储存数据
	3.分区表类型：MBR（Master Boot Record）主引导记录最多支持4个主分区，最大容量为2TB；GPT（Globally Unique Identifier Partition Table）最多支持128个主分区，最大容量可以超过2TB。（多数情况下通常使用MBR分区表）
	
#磁盘分区命名规则
	1.格式：磁盘设备接口前缀 + 设备编号 + 分区编号。接口前缀IDE接口使用hd表示，SCSI、SATA、SAS、USB接口用sd表示；设备编号用小写字母从a开始编号；Linux为每个磁盘的分区分配有1~16分区编号，即分区号，主分区和扩展分区占用1~4号，逻辑分区占用5~16
	2.注意：每块磁盘只能划分一块扩展分区，且扩展分区创建后不能直接使用，需要先在扩展分区内创建逻辑分区（数量没有限制），逻辑分区实际上就是扩展分区内创建的分区

#Linux文件系统
	文件系统格式：Linux中文件系统主要使用ext2、ext3、ext4等，ext即Extented File System（扩展文件系统）。目前Ubuntu默认采用ext4（支持1EB文件系统，16TB单个文件），Linux也支持NTFS、vfat（FAT32）、ISO9660，Linux特有的Linux Native（就是根"/"分区）和Linux Swap分区
	
```

### 磁盘分区管理

```shell
#查看磁盘分区情况	ls -l /dev/sd*
	makima@makima-ubuntu-pc:~$ ls -lh /dev/sd*
	brw-rw---- 1 root disk 8, 0  3月  6 15:29 /dev/sda
	brw-rw---- 1 root disk 8, 1  3月  6 15:29 /dev/sda1
	brw-rw---- 1 root disk 8, 2  3月  6 15:29 /dev/sda2
	brw-rw---- 1 root disk 8, 5  3月  6 15:29 /dev/sda5
	#解释：b表示该设备类型为块设备（c表示字符设备），8表示主设备号，0、1、2、5表示次设备号

lsblk	即list block，以树形格式列出块设备分区信息和它们之间的依赖关系（不会列出RAM盘的信息），用来查看当前连接系统的设备（这些设备有可能没有挂载）
	#例子：
	makima@makima-ubuntu-pc:~$ lsblk | grep sd	#过滤出块设备信息 比 ls /dev/sd*	通过查看设备关联文件来查看块设备信息要详细写
	sda      8:0    0    20G  0 disk 
	├─sda1   8:1    0   512M  0 part /boot/efi
	├─sda2   8:2    0     1K  0 part 
	└─sda5   8:5    0  19.5G  0 part /var/snap/firefox/common/host-hunspell

gparted	带有GUI的分区管理工具，需要下载sudo apt install gparted

free	查看内存和交换分区
	-h 即human readable，人性化显示大小

#交换分区管理，类似Windows的虚拟内存管理
swapon	查看交换分区的绝对路径、大小、挂载交换分区
	#用法：swapon	查看信息；swapon 交换分区的绝对路径	用指定的交换分区文件挂载为交换分区
swapoff	关闭交换分区
	#用法：swapoff 当前交换分区的绝对路径
fallocate 在磁盘上分配交换分区大小，相当于创建一个指定大小的文件
	-l 指明大小，可以使用单位G 
	#用法：sudo fallocate -l 4G /swapfile	在/下生成一个4G大小的文件
mkswap	设置交换分区，将一个文件转换为交换分区文件
	#用法：mkswap 文件的绝对路径
#设置交换分区的步骤：swapon (查看当前的交换分区路径)->swapoff 路径 (关闭当前的交换分区）->fallocate -l 大小 路径 （生成所需大小的文件）->mkswap 上一步生成文件的路径 （将生成的文件转换为交换分区文件）->swapon 交换分区文件路径 使用交换分区文件作为交换分区）
#注意：在设置新的交换分区前，需要先关闭当前交换分区
```

### 文件系统管理

```shell
du	即disk usage，查看磁盘目录下文件和目录的使用情况
	-h 人类友好型阅读方式
	-s 仅显示总计大小，仅显示到第一级目录，第一级目录下的子文件和目录不显示
	-a 列出所有文件和目录大小
	#用法：du 参数 目录
	#例子：du -sh * | sort -h	当前目录下的文件和目录按从小到大排序（du加上了s参数，不会显示当前目录下的子目录中的文件和目录大小，看着简洁）

df	即disk filesystem space usage，查看磁盘文件系统的空间使用情况（整个文件系统）相当于查看当前系统中各个分区的空间使用情况，(也可以用来查看磁盘分区的挂载点，但是信息不全）
	-h 人类友好
	-T 显示文件系统类型（显示时多显示一个类型属性）
	-t 查看挂载的文件系统是哪个分区，后面需要指定格式，如ext4，该参数需要放在最后，紧跟格式名称

blkid	即block id，查看块设备文件系统信息，显示块设备使用的文件系统类型、UUID、卷标等信息
	-k 列出所有已知文件系统
	-p 切换至低级超级探针模式
	-o 输出格式，如：blkid -o udev 使用键值对方式显示
	#用法：blkid [参数] [设备文件名] 加上sudo命令可以显示出设备的系统标签

e2label	设置文件系统卷标，相当于给设备加了个别名（标签）
	#用法：e2label 设备文件名 [新卷标名]

fsck	检查和修复文件系统
	-p 不提示用户，直接修复
	-n 只检查，不修复
	-f 强制检查
	-c 检查可能的坏块，并加入到坏块列表中
	#用法：sudo fsck [参数] [设备名]

mount	将指定的设备文件名挂载到指定的挂载点上
	-t 指定文件系统类型，一般不需要指定，mount会自动识别
	-l	列出所有挂载点
	#用法：sudo mount [参数] [参数值] [挂载路径]	先创建用于挂载的目录，一般创建在/mnt下，创建完目录后，mount 分区名 挂载目录路径
	#例子：sudo mkdir /mnt/test && sudo mount /dev/sdb1 /mnt/test
	#挂载后查看
    	makima@makima-ubuntu-pc:~$ mount -l | grep sdb1
    	/dev/sdb1 on /mnt/test type ext4 (rw,relatime) [makima]

umount	将指定的设备文件名从指定的挂载点上卸载
	#用法：sudo umount 分区名
	#例子：sudo umount /dev/sdb1	将/dev/sdb1分区从挂载点上卸载

#文件系统配置文件
	1.路径：/etc/fstab
	2.内容：每行六个字段，设备名、挂载点、文件系统类型、挂载选项、是否备份（0不备份，1备份）、是否检查文件系统及顺序（0不检查，1检查）；类似crontab
	
quota	分配用户磁盘空间
	-u 显示用户的磁盘空间配额
	-v 显示执行过程
	-a 显示所有的文件系统
	-s 人性化阅读显示（类似其他命令的 -h）
	#搭配setquota、edquota使用，且需要先下载，sudo apt install quota
```

### 系统备份与恢复

```shell
tar	备份和恢复分区（tar不仅可以用来归档压缩文件）
	-c 即create，创建新的备份文件
	-v 即verbose，显示执行过程
	-p 即same-permissions，保留原来的文件系统权限
	-z 使用gzip压缩
	-j 使用bzip2压缩（效率更高，压缩包更小）
	-f 指定备份或还原文件，后面要紧跟参数值，备份时参数值为要将文件备份到的压缩文件路径，还原时参数值为需要还原的压缩文件路径
	-x 解压备份文件
	-C 指定解压到的路径
	--exclude=<样例> 设置排除项，如：--exclude=/tmp，则备份时会将/tmp排除
	/lost+found 系统发生错误时，提供恢复丢失文件的方法
	#用法：tar 参数 参数值 文件或目录路径
	#例子：
		1.tar -cvjpf /tmp/test.tar.bz2 /boot	将boot文件备份压缩到/tmp下并将压缩包命名为test
		2.sudo mkdir ~/boot && tar -xvjpf /tmp/test.tar.bz2 -C ~/boot	将刚才备份的boot文件解压到~/下

dump	专业的备份文件工具，支持完全备份和增量备份，需要下载
	-f 指定备份或恢复文件，紧跟参数值，放在参数的最后面
	-t 查看备份或恢复文件
	-0~9 备份等级，0为完全备份，其他为增量备份
	#用法：sudo dump 参数 参数值 文件路径
	#例子：sudo dump -of /tmp/makima.dump /boot	将/boot备份到/tmp/makima/dump中，采用完全备份
restore	与dump对应的恢复文件工具，需要下载
	-f 指定备份文件，紧跟参数值，放在参数的最后面
	-t 查看备份文件
	-r 恢复备份文件
	#例子：sudo restore -rf /tmp/makima.dump
```

### 挂载与卸载U盘

```shell
一般U盘都是NTFS文件系统格式，需要先下载ntfs-3g才能识别，exFAT（FAT64）文件系统需要下载exfat-fuse和exfat-utils
#步骤：插入U盘->lsblk | grep sd 查看是否识别到设备和其分区->
```

## 网络管理

### 网络用户

```shell
who	也可以用w，查看所有登录到系统的用户信息
	-H 显示各栏位的标题信息，w不能使用-H参数

whoami	显示当前本终端登录的用户信息

hostname	临时修改主机名
	#用法：sudo hostname 主机名

#永久修改主机名
	sudo vim /etc/hostname	修改完成后需要重启生效
```

### IP地址管理

```shell
ip	地址管理，用于取代ifconfig命令
	1.ip a	管理ip地址
		ip a	显示当前系统所有的IP信息，inet为IPv4，inet6为IPv6
		ip a show 网卡名	网卡名就是：lo、ens33这样的ip a中显示的每一组的第一个名字就是网卡名
		ip a add/del IP地址 dev 网卡名	使用某个网卡添加/删除一个指定的IP地址
		#例子：
			1.ip a add 192.168.47.115/24 dev ens33	使用网卡ens33添加一个IP地址192.168.47.115，子网掩码255.255.255.0
			2.ip a del 192.168.47.115/24 dev ens33	删除刚才添加的ip
	2.ip route	查看网关信息
		makima@makima-ubuntu-pc:/mnt$ ip route
		default via 192.168.47.2 dev ens33 proto dhcp metric 100 
		169.254.0.0/16 dev ens33 scope link metric 1000 
		192.168.47.0/24 dev ens33 proto kernel scope link src 192.168.47.128 metric 100 
		#解释：其中192.168.47.2为网关，dev ens33表示使用的这张网卡，proto dhcp表示网络协议是动态ip
	3.ip link	查看网卡信息
		ip link show 网卡名	查看网卡信息
		ip link set 网卡名 down	关闭网卡
		ip link set 网卡名 up	开启网卡

nslookup	查询域名的ip地址
	#用法：nslookup 域名 [DNS服务器]	DNS服务器选填，不填则使用默认DNS查询，常见DNS服务器：8.8.8.8（Google的DNS）、114.114.114.114（中国移动、电信、联通的DNS）

ping	测试IP地址连通情况，ping基于ICMP（Internet控制消息协议）报文工作，测试目标IP与本机的连通性、速度、稳定性
	-c 即count，指定发送ping包的数量
	-s 指定发送ping包的字节数，默认为56B（内容） + 28B（ICMP报文头）共84B，ping包最大为65535B，内容最大即65535B - 28B即65507B
	-t 设置存活数值TTL，即IP包被路由器丢弃前运行通过的最大网段数
	-i 设置发送间隔，单位为：s，默认为1s
	#用法：ping [参数] IP地址或域名
	#例子：ping -c 10 -s 65507 127.0.0.1	发送10大小为65535B（实际内容为65507B）给ping包给127.0.0.1
```

### 网络通信

```shell
#ssh配置
	1.路径：/etc/ssh/sshd_config
	2.修改配置：sudo vim /etc/ssh/sshd_config && sudo service sshd restart	修改后重启sshd进程
		#例子：在sshd_config最后添加两行，并保存，则ssh就可以通过端口22和端口6666连接
			Port 22
			Port 6666

ssh	即Secure Shell，安全远程登录，是openssh套件中的客户端连接工具，使用ssh加密协议，可登录服务器提供终端进行管理。
	-p 指定端口，默认为22
	-i 即identity_file，从指定文件中读取传输使用的密钥信息
	#用法：ssh 参数 参数值 用户名@IP地址
	#例子：ssh -p 22 user@127.0.0.1	使用user用户登录127.0.0.1

wall	即write all，向当前所有登录的用户广播消息
	#用法：wall	然后会进入输入模式，Ctrl+D结束输入并发送消息
write	向指定用户发送消息
	#用法：write 用户名	然后会进入输入模式，Ctrl+D结束输入并发送消息
```

### 网络文件传输

```shell
wget	下在网络文件
	-P 指定下载到的路径，用来表明下载到哪个目录
	-p 指定所有用于显示HTML页面的图片等元素
	-A 逗号分隔的可接收的扩展名列表
	-r 递归下载，包括子目录
	-O 指定下载后保存的文件名
	#用法：wget 参数 参数值 URL地址
	#例子：nohup wget -P test -rpA "jpg" http://www.sciencenet.cn	将www.sciencenet.cn中的图片全部下载到test目录中

curl	文件传输，包括下载和上传，要传输文件到SSH服务器上则需要安装libssh2
	-O 使用URL中默认的文件名保存在当前路径
	-o 指定文件名，保存文件在当前路径
	-C 对大文件使用断点续传功能，需要加上参数值 - （没错就是一个 - ）
	-u 指定用户名和密码，格式-u user:passwd
	-T 指定上传的文件名，格式-T 文件路径
	#用法：curl 参数 参数值 URL地址
	#例子：
		1.curl -O http://www.gnu.org/manual/manual.html
		2.curl -u user:passwd -T testfile.gz sftp://127.0.0.1/srv/ssh/upload/123.gz 将testfile.gz使用用户user上传到127.0.0.1的/srv/ssh/upload下并重命名为123.gz
		3.curl -u user:passwd sftp:127.0.0.1/srv/ssh/download/test.tar -o 11.tar 将127.0.0.1的/srv/ssh/download目录下的test.tar文件使用用户user下载到本地并重命名为11.tar

scp	即secure copy，安全文件复制，需要安装openssh-server才能使用
	-P 指定连接端口
	-p 保留文件的访问和修改时间
	-C 即Compression，在复制过程中压缩文件或目录
	-c 即cipher，将数据传输进行加密
	-r 即recursively copy，递归复制
	-i 即identity_file，从指定文件中读取传输使用的密钥信息
	#用法：scp 参数 参数值 目标文件或目录的路径 需要传输到的路径
	#例子：
		1.scp -Cr test user@127.0.0.1:/home/111	将test目录使用user用户的信息递归复制到127.0.0.1的/home/111目录下
		2.scp -P 6666 test.gz user@127.0.0.1:/srv/ssh/upload/1.gz	将test.gz使用user用户上传到127.0.0.1的/srv/ssh/upload/下并重命名为1.gz
		3.scp -P 22 -r user@127.0.0.1:/srv/ssh/download test	将127.0.0.1的/srv/ssh/download目录使用user用户递归下载到当前路径并重命名为test

git clone	复制Git仓库到本地目录中，需要安装，sudo apt install git
	#用法：git clone github地址 [本地项目名]	本地项目名不填，则默认使用github中的项目名
```

### 信息统计与监控

```shell
ss	统计网络信息，类似netstat，但显示的信息更加丰富，通常搭配grep过滤出目标进程，一般要使用sudo，显示信息更多
	-a 即all，显示所有的socket连接
	-n 即numeric，不解析服务名，端口仍用数字表示
	-t 即TCP，显示TCP的socket
	-u 即UDP，显示UDP的socket
	-p 即process，显示使用socket的进程
	-w 即raw，显示原始socket信息
	-l 即listenling，仅列出在监听状态的socket
	-r 即resolve，尝试解析数字地址或端口，如：22就解析成ssh，80就http
	#用法：sudo ss 参数
	#例子：sudo ss -antp | grep 22	查看监听22端口的程序

lsof	即list opened files，列出进程打开网络端口或者文件信息
	-i 指定条件，如：tcp、udp，使用端口时格式为 -i :端口号
	#用法：sudo lsof 参数 参数值
	#例子：sudo lsof -i tcp 查看所有tcp网络信息；sudo lsof -i:22 查看22端口的信息

nethogs	实时网络流量监控，需要安装sudo apt install nethogs
	-d 指定监控的时间间隔，单位为：s
	#用法：sudo nethogs 参数 网卡名	进入监控界面，按Q或者Ctrl+C退出

ufw	网络防火墙，控制网路协议或端口的启动、禁用
	enable	启动防火墙
	disable	关闭防火墙
	status	查看防火墙状态
	allow	允许协议、端口、端口/协议
	deny	拒绝协议、端口、端口/协议
	reset	重置防火墙为初始状态
	#用法：sudo ufw 参数 参数值	参数值一般是端口名或协议名，如：22、http、https

wireshark	监听网络数据，需要安装：sudo apt install wireshark
	
```

## 软件安装

### 源码编译

```shell
make	编译源码
make install	将编译生成的可执行文件安装到系统
make clean	清除临时文件
```

### apt安装

```shell
#介绍：Ubuntu包管理工具，基于deb，Ubuntu16.04前使用apt-get（后续版本也可以使用），通过网络自动下载安装包进行安装
#常用参数
	-f	即fix，修复损坏的依赖关系
	update	更新软件包列表
	upgrade	升级本地可更新的全部软件包，存在依赖问题的不会更新
	install	普通安装
	reinstall	重新安装
	remove	移除式卸载
	purge	清除式卸载，删除软件时清除其配置
	show	显示某个已安装的软件信息
	list	列出已经安装的所有软件
	autoclean	删除/var/cache/apt/archives/中已经过期的deb文件（就是清除过期安装包）
	clean	清空/var/cache/apt/archives/中的所有deb文件（清除所有安装包）
#用法：sudo apt 参数 参数值
```

### dpkg安装

```shell
#介绍：debian package management，使用已经打包成deb安装包的文件进行安装，需要自行下载deb包到本地，再使用dpkg安装
#常见参数
	-i 即install，安装deb包
	-r 即remove，删除已经安装的软件
	-P 即purge，清除式删除已安装软件
	-c 即contents，列出deb包中包含的内容
	-I 即info，显示deb包中的信息
	-x 即extract，从deb包中抽取文件
	-l 即list，理出系统安装的deb包
#用法：sudo dpkg 参数 参数值
```

### gdebi安装

```shell
#介绍：gdebi也是通过下载号的deb文件进行安装，安装软件时会自动获取依赖关系，使用dpkg安装时有可能会有依赖问题，而gdebi没有，需要下载sudo apt install gdebi
#用法：sudo gdebi 软件deb包
```

### bash安装

```shell
#介绍：实际上就是利用下载的文件中配置好的sh脚本文件安装
#用法：sudo bash 安装脚本.sh
```

### 图形界面安装

```shell
#介绍：即使用新立得图形界面安装软件，类似Windows的Microsoft Store和IOS的Apple Store
```

## 其他

### Linux代理

```shell
proxychains	可以给别的程序开代理，一般在终端中使用
#用法：先在配置文件中设置好代理，配置文件位置：/etc/proxychains.conf，在最后的Proxy List中添加代理，之后使用命令：proxychains + 需要代理的命令
```

