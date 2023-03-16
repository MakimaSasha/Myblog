---
title: C++算法模板
date: 2022-02-22 22:25:32
tags: C++
---

### 常见算法


```c++
sscanf(,,)	转换字符串的格式，如
	sscanf(s,"%d",&a) 将字符串s转换为整型并存储到a中
sprintf(,,,,,) 格式化输出	如
	sprintf(s,"%d%d%d",a,b,c) 将a,b,c的值按%d%d%d输出到字符串s中

ios::sync_with_stdio(false);	关闭c++cin和cout与scanf和printf的流相关。
cin.tie(0);cin.tie(0);

sort(a,a+n,cmp);
sort(a,b,cmp);	将a，到b范围内数据排序，自定义排序方式cmp函数构造，对自己定义的结构体数组排序时也要用cmp。如果认为第一个参数比第二个小，也就是第一个参数需要排在第二个参数前面时返回true，反之返回 false。系统默认a<b时返回true，于是从小到大排。而上面的例子是当b小于a时，认为a小于b。所以排序的结果就是将元素按从大到小的顺序排序。
重载<运算符 
bool operator<(const int &a, const int &b){
	return a<b;
}
返回值作用同sort的cmp，需要a排在b前时返回true。
```



### 快排

```c++
void qsort(int l, int r){
	int i=l,j=r;				//i，j工作指针
	int pivot = d[(l+r)/2];		//选择中间元素作为枢纽，当然也可以选择其他位置的元素
	while(i<=j){				//当i大于j时完成一次划分
		while(d[j]>pivot)j--;	//从右向左找比枢纽小的数
		while(d[i]<pivot)i++;	//从左向右找比枢纽大的数
		if(i<=j){				//不加‘=’的话，当i==j 且d[i]==pivot时，会死循环	如：1 2 4 5 4序列
			int temp=d[i];d[i]=d[j];d[j]=temp;	//交换d[i]，d[j]的值
			j--;i++;			//i，j移向各自的下一位
		}
	}							//划分成的两部分继续进行划分
	if(i<r)qsort(i,r);			//剩下的右半部分
	if(j>l)qsort(l,j);			//剩下的左半部分
}
```

### 找第K小的元素（可以开O2的话，直接快排完所有元素大概率能过）

```c++
void qsort_to_find_nth(int l, int r, int ind){	//查找第ind个元素，从第1个元素开始计数
	int i=l,j=r;				//i，j工作指针
	int pivot=d[(l+r)/2];		//选中间元素为枢纽
	while(i<=j){				//当i大于j时完成一次划分，此时i，j中剩一个元素时，且那个元素是序列是ind是走最后的else
		while(d[i]<pivot)i++;
		while(d[j]>pivot)j--;
		if(i<=j){
			swap(d[i],d[j]);
			i++;
			j--;
		}
	}
	if(ind<=j)qsort_to_find_nth(l,j,ind);		//当i，j中间剩下的那个元素是序列是ind时结束
	else if(ind>=i)qsort_to_find_nth(i,r,ind);
	else cout<<d[j+1];
}
```

### 快速幂

```c++
一般的快速幂（一般会让求余数）
    如：base^index mod c				//index在c++中是关键字，一下用indx代替
    for(int i=1;i<=n;i++){
        ans = (ans%c*(base%c))%c;	//等价于 ans = (ans*base)%c
    }
	ans %= c;						//最后再求一次于。因为：(a*b*c*d)%e == ((a%e)*(b%e)*(c%e)*(d%e))%e

优化一：把base的index次方看成base的index/2次方的平方和index%2次方的乘积 
    即：base^index = base^(index/2)*base^(index/2)*base^(index%2) 然后递归
    long long qpow(long long base, long long indx, long long c){
    	if(indx == 0)return 1;
    	else if(indx == 1)return base;
    	else{
            long long ans = (qpow(base,index/2,c))%c;	//先把指数拆两半，每一半的结果再%c
            ans = ((ans%c)*(ans%c))%c;					//把两半对c的余数的乘积对c求余数，这样写是避免爆long long
            											//即：(ans*ans)%c = ((ans%c)*(ans%c))%c
            if(index%2 == 1) ans = ((ans%c)*(base%c))%c;//判断index是否为奇数，奇数得(ans*base)%c
            return ans;
		}
	}
	ans = qpow(base, indx, c);
	ans %= c;				//以免出现index等于1时，没对c取余

优化二：将index转化为二进制
    long long qpow(long long base, long long indx, long long c){
    	long long ans=1,tmp=base;
    	while(indx != 0){
            if(indx&1)						//1表示二进制中最后一位为1，用于判断indx最后一位是否为1
                ans = ((ans%c)*(temp%c))%c;	//indx最后一位为1，实质就是指indx是个奇数，所以要算(ans*temp)%c
            temp = ((temp%c)*(temp%c))%c;	//temp自乘，相当于再算temp*temp % c
            indx = indx>>1;					//indx左移一位，相当于除以二
        }
    	ans %= c;
    	return ans;
	}
	ans = qpow(base,indx,c);
	ans %= c;								//避免出现indx为0，c为1时ans为1，结果没有取余的情况
```

### KMP算法

```c++

```

[KMPblog(不推荐)](http://www.matrix67.com/blog/archives/115) 	[KMP题解](https://www.luogu.com.cn/problem/solution/P3375)

[Trie树](https://blog.csdn.net/qq_30974369/article/details/74936845)

[AC自动机](https://www.cnblogs.com/cjyyb/p/7196308.html)

[AC自动机学习笔记](https://xminh.github.io/2018/06/02/KMP+AC%E8%87%AA%E5%8A%A8%E6%9C%BA-%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.html)

### 高精度模板

#### 加法——两个数相加后的位数最多比两个数中位数较大的一个多一位

```c++
#include <bits/stdc++.h>
using namespace std;
int main(){
	char s1[1001];			//用字符串保存大数
	char s2[1001];
	int d1[1001];			//实际运算使用的数组
	int d2[1001];
	int ans[1001];			//最终结果保存的数组
	cin>>s1>>s2;
	int s1_l = strlen(s1);
	int s2_l = strlen(s2);
	for(int i=0;i<s1_l;i++){
		d1[i] = s1[s1_l - i - 1] - '0';		//将字符串转化为整型数字，并把低位储存在数组的低位
	}
	for(int i=0;i<s2_l;i++){
		d2[i] = s2[s2_l - i - 1] - '0';
	}
	int ca = 0;								//ca用于保存是否进位，ca只会是0或1，因为最多进1，初始值为0
	int max_len = max(s1_l,s2_l);			//使用较大的一个数的位数作为循环的次数
	for(int i=0;i<max_len;i++){
		ans[i] = d1[i] + d2[i] + ca;		//对应位数相加并加上上一次的进位
		ca = ans[i] / 10;					//将十位上的数赋给ca
		ans[i] %= 10;						//数组的每一个元素仅保留一位
	}
	if(ca != 0)cout<<ca;					//若最后一次有进位就先输出它
	for(int i=max_len-1;i>=0;i--){
		cout<<ans[i];						//倒着输出剩下的位
	}
	return 0;
}
```

#### 乘法——两个数相乘结果最大有它俩位数之和位

```c++
#include <bits/stdc++.h>
using namespace std;
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);cout.tie(0);
	char s1[2005],s2[2005];							//两个字符串用于储存最初的大数字符
	cin>>s1>>s2;									//不要忘记输入，不要忘记输入，不要忘记输入
	int d1[2005]={0},d2[2005]={0},ans[4010]={0};   //两个整型数组用于储存俩个大数，ans数组用于存储最终结果
	int s1_l = strlen(s1);
	int s2_l = strlen(s2);
	for(int i=0;i<s1_l;i++)
		d1[i] = s1[s1_l - i -1] - '0';		//将字符串转化位整型
	for(int i=0;i<s2_l;i++)
		d2[i] = s2[s2_l - i -1] - '0';
	for(int i=0;i<s1_l;i++)				//模拟竖式乘法，这里没进位。ans中现在保存的数都是竖式中一列的和
		for(int j=0;j<s2_l;j++)
			ans[i+j] += d1[i] * d2[j];		//注意此处是 +=
	int indx = s1_l + s2_l;				//indx保存的是最终结果的位数
	for(int i=0;i<indx;i++){	//进位操作，当ans数组的元素不止一位时，把个位留下，其他位加到下一个元素中
		if(ans[i]>9){
			ans[i+1] += ans[i] / 10;
			ans[i] %= 10;
		}
	}
	while(ans[indx] == 0 && indx>=1)indx--;		//计算实际位数
	for(int i=indx;i>=0;i--)cout<<ans[i];		//输出
	return 0;
}
```

### 高精度*单精度

```c++
#include <bits/stdc++.h>
using namespace std;
int func(int n, int a){
	int d[5000]={1};	//给数组的第一位赋初值
	int ret = 0;
	int num = 1;
	int i,j;
	for(i=2;i<=n;i++){
		int ca = 0;
		for(j=0;j<num;j++){
			d[j] = i * d[j] + ca;	//单精度*高精度的每一位 + 上一次的进位
			ca = d[j] /10;			//当前位只保留个位
			d[j] %= 10;
		}
		while(ca>0){
			d[j] = ca%10;
			ca /= 10;
			j++;
		}
		num = j;
	}
	for(i=0;i<num;i++){
		if(d[i] == a)ret++;
	}
	return ret;
}

int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);cout.tie(0);
	int n;
	cin>>n;
	while(n--){
		int temp,target;
		cin>>temp>>target;
		int ans = func(temp,target);
		cout<<ans<<endl;
	}
	return 0;
}
```

[对应的习题](https://www.luogu.com.cn/problem/P1591)



### 二叉搜索树第K大节点

```c++
// 非递归，用栈来暂存
// 思路：二叉搜索树的中序遍历（左根右）得到的序列是从小到大的序列，而找第k大节点，可以倒着找，（右根左）
int kthLargest(TreeNode* root, int k) {
    stack<TreeNode*> s;
    TreeNode *cur = root;
    while(cur != nullptr || !s.empty()){
        if(cur != nullptr){
            s.push(cur);
            cur = cur->right;
        }else{
            cur = s.top();s.pop();
            if(--k == 0)return cur->val;
            cur = cur->left;
        }
    }
    return -1;
}
```

