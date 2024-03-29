---
title: C语言基础
date: 2023-03-12 15:39:21
tag: C
---

### 数据类型

```c
1.基本类型
    整型
    字符型
    浮点型
    	a.单精度
    	b.双精度
2.构造类型
    数组类型
    结构体类型
    联合体类型
    枚举型	例子：enum Weekday {Monday,Tuesday,Wednesday,Thursday,Friday,Saturday,Sunday};不赋值，第一个元素默认为1，后面依次加1
3.指针类型
4.空类型
```

### 常用头文件

```c
#include <stdio.h>	//标准输入输出头文件
#include <math.h>	//数学函数头文件，sin(),tan(),sqrt(),pow()
#include <stdlib.h>	//标准库头文件，内存管理、字符串转换、算法排序，malloc(),free()
#include <string.h>	//字符串头文件，strlen(),strcpy()
#include <stdbool.h>//引入bool头文件，c语言默认没有bool类型
```

### 基础知识

```c
运算符执行顺序，优先级高者优先，同优先级下，从左到右一次执行，注意是前面的结果运算参加比较如：int a=3,b=2,c=1; int d = a>b>c;d的结果为0，因为a>b为真结果为1，而1>c为假，最终表达式结果为假，即0，//C语言中0为假，非0皆为真，哪怕是负数也为真

比较运算符中==和!=优先级最低
    
=	//赋值符号的优先级仅高于逗号，赋值运算符表达式的最终结果就是赋的值，如：a=1表达式的值为1，a=0表达式的值为0
,	//逗号优先级最低，且表达式的数值为最右边那个表达式的数值如：a = (1,2>5,3,4)	a最终的值为4，而a = 1,2>5,3,4 a最终的值为1
%	//左边右边都得是整数，且运算结果的符号（正负号）和%前的数据一致

进制转换，整数部分对应位置上的数乘上基数的对应次方即可，小数部分对应数乘上基数的对应次方分之一即可，如：FF.1的十进制整数部分为15*16+15=255，小数部分为1*(1/16)=0.0625，最终结果为255.0625
    
int x;x=0.5;	//最后x会丢掉小数，即x最后值为0，表达式中遇高（高位类型）则高（表达式结果转换为高位类型），但是最终赋值需要看变量的类型，如果是低位则会舍弃多余位，通常带有小数的表达式中要注意变量的类型，如果是float和double类型，在做除法，被除数的整数时记得加上小数点，如：1.0/(x*x)，将结果转换为浮点型，浮点型在不指定小数位数时为6位小数

取x的整数部分位数：(int)log10(x)+1
表示x，y中只有一个为0的表达式：x*y==0 && x+y !=0

//浮点型的比较存在精度误差，一般不直接比较，而是用作差后小于某一个非常小（比如1e-6）的数来控制误差

&&和||	//会出现“短路”现象，当&&前面条件为假时，其后面表达式不执行，当||前面表达式为真时，其后面表达式不执行
case	//会出现“穿透”现象，当case中没有break时，有一个case满足时，执行完该case中的语句后，会继续将该case后的case中的语句都执行直到遇到break或者switch语句执行完毕。如果default语句在所有case最后，此时可以不加break；如果default语句之后还有case语句，如果不加break，则default语句执行过之后会继续下面的case语句
else	//匹配离其最近的没有被匹配的if
强制类型转换例子：(int)x 将x强制类型转换为int类型，注意和python区分！！！

break 与 continue	//break 用来结束所有循环，循环语句不再有执行的机会；continue 用来结束本次循环，直接跳到下一次循环，如果循环条件成立，还会继续循环，在多层循环中，break 语句只向外跳一层，continue语句也只结束本层的本次循环。他俩可以用在循环结构和选择结构中。
    
A ? B:C	//多个三目运算符一起时遵守就近原则，:匹配离它最近的未匹配的?
    
字符：\ddd和\xhh	ddd表示3位八进制，hh表示2位十六进制，如：char c = '\x53', d='\123', e='S'，都是'S'，\123是八进制123即十进制83，十六进制53，ASCII码表中为'S'，char类型只占一个字节，0xFF即255，一个直接全为1，在有符号数下为-1，大写字母和其对应小写字母之间的差值为32，A为65，a为97
	\b	//表示退格，删除前一个字符
    \t	//水平制表符
    \v	//垂直制表符
    \a	//表示响铃
    \n	//换行
    \r	//表示回车
    \\	//表示 \
    \f	//表示换页
   	\0	//表示NULL，NULL是在stdio.h中使用宏定义命令定义的
    %%	//输出%，%也是转义符类似\
```

### 原码、反码、补码

```c
//正数的原码、反码、补码均相同。

原码：用最高位表示符号位，其余位表示数值位的编码称为原码。其中，正数的符号位为 0，负数的符号位为 1。

负数的反码：把原码的符号位保持不变，数值位逐位取反，即可得原码的反码。

负数的补码：在反码的基础上加 1 即得该原码的补码
```



### 输入输出格式

```c
scanf("输入格式",变量地址);	//输入时必须按照scanf的输入格式输入，后面参数必须为变量的地址，否则要么输入无效，要么报错，如：scanf("a=%d, b=%d",&a,&b);输入时为：a=13245, b=123123	中间的空格可以少，但是"a="、","、"b="这些格式不能少

printf("%4d", a);	//输出格式为4位整形，多了不管，不够时用空格补足4位，加上.则.前面表示整数部分多少位，.后面表示小数部分多少位，如：%5.2f	表示整数部分5位，小数部分2位

//注意：printf是标准函数库里的函数，不是C语句，C语句是指赋值、循环、复合（用{}括起来那种）、跳转这些实现c语言程序功能的代码，预处理命令也不是C语句

int类型变量使用%f输出是乱数，float类型使用%d输出也是乱数，基本类型不一致输出的都是意料之外的数，比如都是整型long int和int的输出一般可以正常输出（数据在该类型范围以内），同理double和float也是
```

### 指针

```c
*p++	//先取p所指向的地址中的值，再指针下移，这是因为++在变量右边时的特性，变量先参加运算然后再自增，注意类似的有：!x-- 同样是先吧x值取出来，然后对值取非，然后x再--
(*p)++	//取p所指向地址的值，再把值加一，这是使用括号改变优先级了
++*P	//先取p所指地址的值，然后将值加1
*++p	//p指针先下移，再取值，后面这两个例子实际上就是，*和++同优先级时从右向左结合
      
char a = 'b';char *p, **q;p=&a;q=&p;	//p的值是a的地址，*p的值是a的值即'b'；q的值是p的地址，*q的值是p的值，即a的地址，**q的值是a的值即'b'

同类型指针变量不能进行的运算是+，因为指针变量的值实际上是地址，进行加法相当于地址相加，但是可以进行减法，可以得到俩指针变量之间相同类型变量的个数
//注意：!（取非运算符）、~ (取反运算符)它们和++、--实际上是同一优先级且它们的结合方向是从右到左，但是当操作的对象在--或者++左边时，--和++的特性会让它左边的运算式先执行，再执行--或++ 
```

### 函数相关

```c
函数的实现如果放在调用函数之后，则需要在调用处前对被调用函数进行说明，格式为：函数返回类型 函数名(参数类型, 参数类型...);	//形参名不重要，可以不一致，但是类型必须有且一致，而且形参类型的顺序也必须一致

仅仅是传值形参类型，形参的数值变化不会影响实参（原变量）的数值
    
//函数调用不能作为一个函数的形参，当可以作为实参
    可以使用函数指针，将一个函数作为另一个函数的形参，实现回调函数功能
```

### 字符串常用代码

```c
//while((ch=getchar()) == '\n')...	一般用于捕获字符输入，检测到换行时结束输入

//void fun(char *p1, char *p2){while((*p1=*p2) != '/0'){p1++;p2++;}}	把p2所指的字符串复制到p1所指的内存空间中

//使用strlen()函数时需要导入string.h头文件，strlen返回的是字符串的长度，即有多少个字符，不包括'\0'（字符串结束符），所以总的字符串长度为strlen(string)+1，一般会在malloc分配空间时，在这个位置出改错题，特别注意使用fgets()获取字符串时会将回车保存为字符串的最后一个字符，没有'\0'，而scanf("%s",str);则以空格为终止符号，且最后一个字符为'\0'，不会保存空格

//使用手动复制字符串什么的，注意字符串最后一定要是'\0'，一般改错题会有这个坑
#include <stdio.h>
int main() {
    char scanf_s[100];		//定义一个100个char的字符数组
    scanf("%s", scanf_s);	//输入字符串到scanf_s中，scanf检测到空格（回车也会）会认为字符串结束（结束以'\0'表示），并将空格后的字符作为下一个输入处理
    char *p = scanf_s;
    p++;				//p++，即指针下移
    printf("scanf_s:%s, *p:%c\n", scanf_s, *p);
    fflush(stdin);      //清空标准输入缓冲区，第一次scanf输入后缓冲区还剩一个回车，不清空的话回车会直接使gets函数停止接收字符串
    char gets_s[100];
    char fgets_s[100];
    int input_max_char_num = 99;
    gets(gets_s);   //gets函数获取输入字符串，以回车为字符串输入结束的标识，因为没有限制输入字符串的大小，存在缓冲区溢出问题
    printf("gets_s:%s\n", gets_s, fgets_s);
    fgets(fgets_s, input_max_char_num, stdin);  //fgets函数是gets的升级版，需要额外指出字符串的最大输入字符数和输入源，stdin表示标准输入
    printf("fgets_s:%s", gets_s, fgets_s);
    return 0;
}
```

#### 输出

```shell
scanf string
scanf_s:scanf, *p:c
gets string
gets_s:gets string
fgets string
fgets_s:gets string
Process finished with exit code 0
```

### 算阶乘

```c
#include <stdio.h>
#include <math.h>
//计算1，-1/3！，1/5！，-1/7！...
int main() {
    double sum = 1, temp = 1;
    int i = 3;
    do {
        temp = -temp * (i - 1) * i;
        sum += 1 / temp;
        i += 2;
    } while (fabs(temp) < 1e-5);
    printf("%.4f", sum);
    return 0;
}
```



### 其他

```powershell
1.高级语言解释方式：源程序由解释程序边解释边执行
2.在第三代计算机期间出现了操作系统
3.Internet的含义：网络中的网络，即互连各个网络
```

