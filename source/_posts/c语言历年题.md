---
title: c语言复试历年题
date: 2023-03-18 22:07:02
title: c
---

# 2019年

## 编程题

### 1到50的阶乘

```c
//因为到50的阶乘，数太大，int和long long都装不下，得用高精度，这题有点逆天

```

### 输入N，输出小于N的素数

```c
//埃筛，数据大时会超时
#include <stdio.h>
int nums[100000005];
int main() {
    int n;
    scanf("%d", &n);
    for (int i = 2; i <= n; i++) {	//从2开始因为1不是素数，呃呃绷
        if (!nums[i]) {		//把每一个素数的倍数都标记上，可能会出现重复标记，比如：2*8=16和4*4=16，相当于16被筛了两次
            for (int j = 2; j * i <= n; j++)nums[j * i] = 1;
            printf("%d ",i);
        }
    }
    return 0;
}

//使用欧筛(又称线性筛)（c语言没有bool类型，int大数组时，可能报超内存）
#include <stdio.h>
int nums[100000005];
int prime[2000000],cnt;

void judge(int n){
    for(int i=2;i<=n;i++){
        if(!nums[i])prime[++cnt]=i;		//用一个数组专门存素数
        for(int j=1;j<=cnt && i*prime[j]<=n;j++){	//将每一个数和素数数组中各个素数的倍数标记上，当该数是素数数组中某个素数的倍数时结束内层循环，减少了重复标记的次数（别问为什么，我也不知道怎么证明）
            nums[i*prime[j]] = 1;
            if(i%prime[j] == 0)break;
        }
    }
}

int main(){
    int n;
    scanf("%d",&n);
    judge(n);
    for(int i=1;i<=cnt;i++){
        printf("%d ",prime[i]);
    }
    return 0;
}


```

### 输入n个数，从小到大排序（n<10000）

```c
//直接快排一直取第一个数为枢轴，数据量大或者数据基本有序时一般会超时
#include <stdio.h>
#define MAX_N 10005
int nums[MAX_N];
void sort(int low, int high) {
    if(low < high){
        int l=low,h=high;
        int tmp = nums[l];
        while(l<h){
            while(l<h && nums[h]>tmp)h--;
            if(l<h){
                nums[l] = nums[h];
                l++;
            }
            while(l<h && nums[l]<tmp)l++;
            if(l<h){
                nums[h] = nums[l];
                h--;
            }
        }
        nums[l] = tmp;
        sort(low,l-1);
        sort(l+1,high);
    }
}

int main() {
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &nums[i]);
    }
    sort(1, n);
    for (int i = 1; i <= n; i++) {
        if (i == 1)
            printf("%d", nums[i]);
        else printf(" %d", nums[i]);
    }
    return 0;
}

//二分快排，取中间值作为枢轴，类似选随机数作为枢轴
#include <stdio.h>

int nums[100005];

void qsort(int low, int high) {
    if (low < high) {	//当low等于大于high时没必要执行，可以不要这个判断，不影响，
        int l = low, h = high, tmp;
        int mid = nums[(l + h) / 2];    //取中间位置的数作为枢轴
        while (l <= h) {
            while (nums[h] > mid)h--;
            while (nums[l] < mid)l++;
            if (l <= h) {            //遇到逆序数（在mid左边的数比mid大，在mid右边的数比mid小，这俩数就是逆序的）时交换位置
                tmp = nums[h];
                nums[h] = nums[l];
                nums[l] = tmp;
                h--;l++;			//千万别忘了这句，否则就死循环了
            }
        }
        if (h > 1)qsort(low, h);
        if (l < high)qsort(l, high);
    }
}

int main() {
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &nums[i]);
    }
    qsort(1, n);
    for (int i = 1; i <= n; i++) {
        if (i == 1) printf("%d", nums[i]);
        else printf(" %d", nums[i]);
    }
}
```

# 2018年

## 简答题

### C语言隐式转换

```c
//float转int且进行四舍五入
int num = (int)(x + 0.5);//强制类型转换可以不要，会隐式地转换

1.算术运算式中，低类型能够转换为高类型
2.赋值表达式中，右边表达式的值自动隐式转换为左边变量的类型，再赋值给左边变量
3.函数递归调用传递参数时，系统隐式地将实参转换为形参类型后，再赋值给形参
4.函数有返回值时，系统隐式地将返回表达式类型转换为返回值类型，再赋给调用函数
```

### 提高C语言执行效率的方法

```c
1.使用指针，可以操作内存，占用内存少，运行速度快，指针可以拥有类似于汇编的寻址方式，使程序更高效。
2.使用位运算，减少了除法和取模运算，大大加快了执行效率
3.使用API，可以调用系统API，接近底层
4.宏定义，可以编译的时候替换
5.使用寄存器变量，提高存取速度
6.使用宏函数，函数调用需占用一定的CPU，宏函数不会调用函数，只需占用一定的空间
7.循环嵌套中，将循环次数较长的设为内置循环，循环次数较短的设为外置循环，减少了CPU跨循环层的次数，提高程序的运行效率
8.嵌入汇编语言，让代码效率贴近极限
9.使用条件编译，减少被编译的语句，从而减少目标程序的长度，减少运行时间
10.循环嵌套中将较长循环设为内置循环，较短循环设为外置循环，减少cpu跨切循环层的次数，提高程序的运行效率
```

### 数组越界的后果

```c
数组越界将会把数据存放到一个未知的区域，当这个区域是系统的主要位置时，会导致系统出现错误甚至崩溃，因此为了系统安全需要进行数组下标越界检查。
```

### 形参实参之间的传递方式

```c
形参和实参都是数组元素时：值传递
形参是指针，实参是数组地址时：地址传递
形参和实参都是数组地址时：地址传递
形参是数组，实参是数组名时：地址传递
```

## 算法题

### 找第K小

```c
#include <stdio.h>

int ksort(int nums[], int low, int high, int k) {
    int l = low, h = high;
    int tmp = nums[l];	//这里必须是选第一个位置的数为枢轴，因为下面会把这个位置和其他数交换，注意此处和二分快排的区别
    while (l < h) {
        while (l < h && nums[h] > tmp)h--;
        nums[l] = nums[h];
        while (l < h && nums[l] < tmp)l++;
        nums[h] = nums[l];
    }
    nums[l] = tmp;
    if (l == k)return nums[l];
    else if (l > k)return ksort(nums, low, l - 1, k);
    else return ksort(nums, l + 1, high, k);
}

int main() {
    int n, k, nums[1000];
    scanf("%d %d", &n, &k);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &nums[i]);
    }
    printf("%d", ksort(nums, 1, n, k));
}
```

### 埃筛

```c
#include <stdio.h>

int main(){
    int nums[205]={0};
    for(int i=2;i<=200;i++){
        if(!nums[i]){
            for(int j=2;j*i<=200;j++){
                nums[i*j] = 1;
            }
        }
    }
    for(int i=2;i<=200;i++){
        if(!nums[i])
        	printf("%d ", i);
    }
    return 0;
}
```

### 字符加密，输入俩字符串，将第二个字符串转换为整数，然后用来对第一个字符串进行凯撒加密

```c
//注意char *p="123";和char a[]="123";的区别。第一个等价于char *p; p="123";其中"123"是无名字符数组不可修改，第二个则是一个名为a的字符数组，属于变量，可以修改；即第一个中如果有p[1]='4';是错误的（能编译但运行出问题返回值异常），而第二个a[2]='2';则是正确的
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

char *encrypt(char text[], int key[], int n) {
    char *ret = (char *) malloc(sizeof(char) * n);
    for (int i = 0; i < n; i++) {
        if (text[i] + key[i] <= 'z')		
            ret[i] = text[i] + key[i];
        else ret[i] = text[i] + key[i] - 26;	//当移动位置超出小写字母范围时，回正
    }
    return ret;
}

char *decrypt(char text[], int key[], int n) {
    char *ret = (char *) malloc(sizeof(char) * n);
    for (int i = 0; i < n; i++) {
        if (text[i] - key[i] >= 'a') {
            ret[i] = text[i] - key[i];
        } 
        else ret[i] = text[i] - key[i] + 26;	//当移动位置超出小写字母范围时，回正
    }

    return ret;
}

int main() {
    char text[100], keytext[100];
    int key[100], n;
    scanf("%s %s", text, keytext);
    n = strlen(keytext);
    for (int i = 0; i < n; i++) {
        key[i] = keytext[i] - 'a' + 1;	//将密钥字符串转换为整数
    }
    char *encrypt_str = encrypt(text, key, n);
    encrypt_str[n] = '\0';  //最后一个字符设置为'\0'
    puts(encrypt_str);		//打印加密后的结果
    char *decrypt_str = decrypt(encrypt_str, key, n);
    decrypt_str[n] = '\0';
    puts(decrypt_str);		//打印解密后的结果
    free(encrypt_str);
    free(decrypt_str);
    return 0;
}
```



### 输入若干个整数（以0结束），构建逆序双链表

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct node{
    int num;
    struct node *pre;
    struct node *next;
}node;

int main(){
    node *head = (node*)malloc(sizeof(node));
    head->num = 0;	//可要可不要，不给头指针的num成员赋值的话，num会随机值（不赋值也行，反正不会用到）
    head->next = NULL;
    head->pre = NULL;
    while(1){
        node *p = (node*)malloc(sizeof(node));
        scanf("%d",&p->num);
        if(p->num == 0){
            break;
        }
        p->next = head->next;
        p->pre = head;
        if(head->next != NULL)head->next->pre=p;
        head->next = p;
    }
    for(node *t=head->next;t!=NULL;t=t->next)printf("%d ",t->num);	//顺序打印结果，查看逆序效果
    printf("\n");
    node *q=head->next;
    while(q->next!=NULL)q=q->next;
    for(;q!=head;q=q->pre)printf("%d ",q->num);			//再倒着打印一次查看pre指针是否正常
    return 0;
}
```

## 其他

```c
C语言中使用双引号括起来的字符串都属于字符串常量，如char *p = "string";其中"string"为无名字符串常量，存储在内存的常量区，而p储存在其所属的函数中，属于局部变量

1.代码区：存放程序代码的内存区域，即CPU执行的机器指令，权限为只读
2.常量区：存放常量，如字符串常量"1234"，10，1，2（这些数字是代码中使用的数字如数组下标，大小；例如int nums[10]中的10）
3.静态区：使用static标识的变量的储存位置，程序运行结束此空间才会释放
4.全局区：定义到函数体外的变量的存储位置，程序运行结束此空间才会释放
5.堆区：使用molloc函数主动申请的内存空间，不需要时使用free进行释放，不释放可能会导致内存泄漏
6.栈区：存放函数的局部变量，形参和函数返回值，系统自动回收自动分配
```

# 汇编

## 常用命令

1. MOV：将寄存器ax中的值移动到寄存器bx中：

   ```
   mov bx, ax
   ```

2. ADD/SUB：将寄存器ax和寄存器bx中的值相加，并将结果存储到寄存器cx中：

   ```
   add cx, ax
   add cx, bx
   ```

   将寄存器ax和寄存器bx中的值相减，并将结果存储到寄存器cx中：

   ```
   sub cx, ax
   sub cx, bx
   ```

3. CMP：比较寄存器ax和寄存器bx中的值大小，并根据比较结果设置标志位：

   ```
   cmp ax, bx
   ```

4. JMP：无条件跳转到标签位置my_label：

   ```
   jmp my_label
   ```

   无条件跳转到内存地址0x1000处：

   ```
   jmp 0x1000
   ```

5. JE/JNE：如果前一次比较结果相等，则跳转到标签位置my_label1，否则跳转到标签位置my_label2：

   ```
   cmp ax, bx
   je my_label1
   jmp my_label2
   ```

6. CALL/RET：调用函数my_func，并将返回地址存储到栈中：

   ```
   call my_func
   ```

