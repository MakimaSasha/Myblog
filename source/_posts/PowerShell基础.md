---
title: PowerShell基础
date: 2022-05-22 19:53:26
tag: Powershell
---

## 常见命令 &&  常识

```powershell
update-help				#更新帮助文档
updata-help -UICulture en-US -ErrorAction SilentlyContinue	#选择英语且忽略错误
Help	*name*			#查询与name相关的命令
Help -full				#显示所有文档
Help -ShowWindow		#以图形化显示文档
Help -online			#用默认浏览器打开对应命令的在线文档
[]						#表示可选参数
<>						#表示必选参数
[[-parameter] <string>]		#意思是该参数可选，当如果选上则必须要给出值
#位置参数				即不用输入参数的名称，按位置输入顺序来对应参数	position标出该参数是哪个位置，named表示要带参数名给值

#开关参数				不需要给值的参数，如：ls -force

```

## 配置相关

### PSProvider

```powershell
get-psprovider		#查看当前powershell适配器

Name                 Capabilities                Drives
----                 ------------                ------
Registry             ShouldProcess               {HKLM, HKCU}
Alias                ShouldProcess               {Alias}
Environment          ShouldProcess               {Env}
FileSystem           Filter, ShouldProcess, Cre… {C, D, E, F…}
Function             ShouldProcess               {Function}
Variable             ShouldProcess               {Variable}


ShouldProcess	支持-whatif，-confirm参数
Filter		支持-filter参数
Credentials		支持凭据参数（-Credentials）连接数据存储
Transactions	支持事务，提交操作，回滚等
```

### Get-PSDrive

```powershell
get-psdrive			#查看当前已连接的驱动器

Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Alias                                  Alias
C                  68.84        130.49 FileSystem    C:\
Cert                                   Certificate   \
D                 180.71        450.17 FileSystem    D:\
E                  25.86         74.15 FileSystem    E:\
Env                                    Environment
F                  19.49         99.52 FileSystem    F:\
Function                               Function
G                 115.35        815.94 FileSystem    G:\
H                   0.00          0.10 FileSystem    H:\
HKCU                                   Registry      HKEY_CURRENT_USER
HKLM                                   Registry      HKEY_LOCAL_MACHINE
Temp               68.84        130.49 FileSystem    C:\Users\Administrato…
Variable                               Variable
WSMan                                  WSMan
```



## 常用命令

### Get-ChildItem

```markdown
1.	aliases: ls, dir, gci
2.	文件属性
    l (link)
    d (directory)
    a (archive)
    r (read-only)
    h (hidden)
    s (system)
3.	参数
	-path		指定查看路径（可以指定驱动器，磁盘，注册表）
	-force		显示隐藏文件
	-filter		过滤出指定文件
	-Recurse	递归子文件夹
```

### Get-ItemProperty

```powershell
查看文件对象属性
```

### Set-Location

```powershell
1.	aliases: cd

2.	切换当前目录
```

### Set-ItemProperty

```powershell
设置文件对象的属性
set-itemproperty -name isreadonly -value $true

```

### Get-Member

```powershell
1.	用途
		查看当前命令输出类型，查看当前命令的属性，方法等
2.	缩写
		gm
```

### Get-Content

```powershell
1.	用途
		读取文件内容
2.	使用方法
		Get-Content file_name
3.	参数
		-Path	指定文件
		-TotalCount	n	指定读取多少行
```

### Select-Object

```powershell
1.	用途
	挑选对象，或者对象的属性
2.	使用方法
	通常搭配管道符使用，A | select-object -property p1,p2选择某些特定的属性显示，p1,p2指选择的属性，
3.	参数
	-property	指定显示对象的属性
	-last num	选择显示后num个对象
	-first	num	选择显示前num个对象
	-unique		去掉重复的对象（大小写不敏感）
```

### Sort-Object

```powershell
1.	用途
	按一定规则排序
2.	使用方法
	类似select-object，一般搭配pipeline使用
3.	参数
	-property	按指定的属性排序，不指定时默认按属性Name排序
	-last num	选择后num个对象
	-first num	选择前num个对象
	-unique		去掉重复内容（大小写不敏感）
```

### ==Where-Object==

```powershell
1.	用途
	查找对象，常与操作符搭配使用
2.	使用方法
	类似select-object，一般搭配pipeline使用。使用时会用到 $_ 占位符号用来代表对象（其实可以理解成函数的形参，当变量传递过来时，用$_来表示当前搜索的这一行的这个对象，$_.property，访问当前对象的某个属性（相当于类的对象的成员变量的使用）），{} 脚本区域（需要使用-FilterScript参数）
3.	参数
	-FilterScript {code}	使用脚本{}内为脚本的代码
	-property	指定用于判断的属性
```

### ==操作符==

```powershell
1.	比较操作符（多个值（用逗号隔开）比较时，返回结果为true的对象，单个值比较时，返回true或者false）
	-GE			大于等于	grater than or equal 
	-CGE		大于等于	C代表case-sensitive大小写敏感（以下参数都有带C格式的）
	-GT			大于		grater than
	-EQ			等于		equal	-CEQ代表相等（相同）且是大小写敏感的
	-LE 		小于等于	less than or equal
	-LT			小于		
	-NE			不等于
2.	模糊比较
	-like		模糊匹配	like和notlike使用通配符时，用此参数。同样也有-CLIKE
	-notlike	like和notlike使用时用于判定的值要加 ""或'' ，如：1 -like "1"
	-Contains	包含	如果对象的属性值中的任何项与指定值完全匹配，则此 cmdlet 获取对象，（包含一个完全相同的值（而不是值的一部分）。始终返回布尔值）
	-Notcontains	不包含一个完全相同值。始终返回布尔值。
	
```



## 管道——pipeline

```powershell
1.	|	管道符
2.	使用方法：
		A | B		将A命令的执行结果作为B命令的输入
3.	效果：		
		减少重复输入
4.	结合Get_Member查看当前命令输出类型，查看当前命令的属性，方法等
	如：ls | Get_Member			
```

### 管道参数输入
```powershell
需要用help（在PARAMETERS）查看后续命令是否支持pipeline传的值（两种类型：ByPropertyName和ByValue）
1.	ByValue		单传值方式	只允许使用一个参数接收管道返回的对象类型（即只能有一个地方接收上一条命令的结果）
		#上一个命令A的结果是下一条命令B的结果，如：get-content test.txt，需要查看B的输入（B的输入使用help B查看）是否有可以接收A的输出类型的参数（A的输出使用 | gm 查看）

2.	ByPropertyName	最佳匹配方式	一次传入多个参数类型，并且命令会自动匹配
```

### ByPropertyName例子

```powershell
aliases.csv文件:			#文件中的数据是按照下一个命令的输入要求编的，new-alias命令有一个Name参数和Value参数可以接收ByPropertyName类型的pipeline input
Name, Value
sel, Select-Object
gs, Get-Serveice

1.	导入csv文件
Import-Csv .\aliases.csv | gm

   TypeName: System.Management.Automation.PSCustomObject	#显示用户自定义对象

Name        MemberType   Definition
----        ----------   ----------
Equals      Method       bool Equals(System.Object obj)
GetHashCode Method       int GetHashCode()
GetType     Method       type GetType()
ToString    Method       string ToString()
Name        NoteProperty string Name=sel			#有一个Name的信息
Value       NoteProperty string Value=Select-Object	 #有一个Value的信息，他俩用来给下一条命令传递参数值，实际上Name和Value就是下一个

2.	查看B命令的帮助文档
help new-alias			#Creates a new alias.为命令创建新的别名
	...
    -Name <System.String>					#参数（传值标记）Name
    Specifies the new alias. You can use any alphanumeric characte
    rs in an alias, but the first character cannot be a number.
    Required?                    true
    Position?                    0
    Default value                None
    Accept pipeline input?       True (ByPropertyName)		#支持使用pipeline的ByPropertyName方式传值
    Accept wildcard characters?  false
    ...
    -Value <System.String>					#参数（传值标记）Value表
        Specifies the name of the cmdlet or command element that is be
        ing aliased.

        Required?                    true
        Position?                    1
        Default value                None
        Accept pipeline input?       True (ByPropertyName)	#支持使用pipeline的ByPropertyName方式传值
        Accept wildcard characters?  false
        
3.	执行代码
	Import-Csv .\aliases.csv | new-alias

4.	成功

#注：若AB命令为同类的命令，则一般都能成功传递参数如：（各种service）get-service, stop-service, start-service
```



## PowerShell的对象

```powershell
1.	powershell命令输出的一行就是一个对象，列就是对象的属性（状态，名称...）
所有对象在一起叫集合，就是输出的那张表
如：
ls
        Directory: C:\Users\Administrator\Desktop\test

#一列就是对象的一个属性
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a---         2022/3/19     20:35          16532   1.txt		#一行就是一个对象
-a---         2022/3/19     20:26             50   aliases.csv
-a---         2022/3/19     19:47              8   asdfas.asdfasdf

#整个输出叫集合

2.	查看命令的属性（property）和方法（method）
	command | get-member		#command的是代指命令
```

### 显示对象指定的属性

```powershell
command | select -property p_name_1, p_name_2....		#select是select-object的缩写，-property指需要显示什么属性，多个属性用逗号隔开
如：
	get-service | select -property ServiceName,ServiceType,UserName
	
	ServiceName                                                        ServiceType UserName
-----------                                                        ----------- --------
AarSvc_820a0                                                               224
AJRouter                                                     Win32ShareProcess NT AUTHORITY\LocalService
ALG                                                            Win32OwnProcess NT AUTHORITY\LocalService
AppIDSvc                                                     Win32ShareProcess NT Authority\LocalService
Appinfo                                     Win32OwnProcess, Win32ShareProcess LocalSystem
AppMgmt                                                      Win32ShareProcess LocalSystem
AppReadiness                                Win32OwnProcess, Win32ShareProcess LocalSystem
AppVClient                                                     Win32OwnProcess LocalSystem
AppXSvc                                     Win32OwnProcess, Win32ShareProcess LocalSystem
...
```



## 脚本块

### 分类

```powershell
1.	.NET框架命令下使用
2.	变量使用法
3.	与比较符联合使用
```

### .NET框架命令下使用

```powershell
使用：
	Invoke-Command -ScriptBlock { code }
如：
	Invoke-command -ScriptBlock { Get-Process }
```

### 变量使用法

```powershell
使用：
	$var = { code }			#其实就是把脚本块赋值给了一个变量（相当于用这个变量来代表那个脚本块）
	&$var					# &代表调用变量 $var代表的脚本块，$var代表脚本块本身
```

### 与比较符联合使用

```powershell
1.	where-object -FilterScript { code }		#where-object命令的FilterScript参数可以使用脚本块
如：
	 Get-Process | where -FilterScript { $_.name -like '*system*'}

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
      0     0.18      40.59       0.00     136   0 Secure System
      0     0.09      21.68       0.00       4   0 System
     56    48.61       2.91       2.03   11708   1 SystemSettings

2.	foreach-object -process { code }	#ForEach-Object命令的process参数可以使用脚本块
如：
	Get-Process | ForEach -Process { if($_ -like '*vm*'){$_} }
# 先判断条件语句，如果成立则执行内部{}中的代码，$_表示显示（默认显示，也可以用 . 来指定要显示对象的什么属性）当前控制的对象
 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     10     2.38      11.70       0.00    3848   0 vmcompute
     26   179.80     160.73       0.00    2832   0 vmms
     12     2.37       7.41       0.00    4788   0 vmnat
      8     7.64       5.87       0.00    4856   0 vmnetdhcp
     27    22.10      30.55       0.00    4812   0 vmware-authd
     34    34.30      53.90       0.00    6496   0 vmware-hostd
     17     3.67      12.12       0.03    4584   1 vmware-tray
     13     2.91      12.24       0.00    4840   0 vmware-usbarbitrator64
 
 3.	select-object -property property_name, { code }		#select-object命令的property参数可以是脚本块
 如：
 	Get-Process | Select-Object -Property {$_.name}, {$_.StartTime.DayOfWeek}
 	# $_代表当前操作的对象，对象通过 . 来调用方法或属性。注：{} 不能少
    $_.name                 $_.StartTime.DayOfWeek
    -------                 ----------------------
    AggregatorHost
    ApplicationFrameHost    Tuesday
    backgroundTaskHost      Tuesday
    chrome                  Tuesday
    chrome                  Tuesday
    chrome                  Tuesday
    chrome                  Tuesday	
 	
```







