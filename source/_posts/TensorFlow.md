---
title: TensorFlow
date: 2023-03-06 0:39:52
tag: 机器学习框架
---

### 张量的定义

```python
import tensorflow as tf #导入tensorflow并使用tf作为其别名

x = tf.range(12)    #使用range创建一个行向量，默认为整数0到11，可以指定类型，如：tf.range(12,dtype=tf.float32)
x.shape             #查看x的形状（每个轴的长度）
tf.size(x)          #查看x的元素总数，这里就是12

y = tf.reshape(x,(3,4)) #修改x的形状为3行4列，不改变值然后赋值给y，其中第二个元组用来指定形状，可以使用-1来自动计算形状如x是1行12列的张量，reshape时形状指定为(2,-1)，则会自动生成2行6列的张量

tf.zeros((2,3,4))       #创建一个全0的张量（就像矩阵），不太好形容，就是两个3行4列的矩阵组成的一个大矩阵
tf.ones((2,3,4))        #创建一个全1的张量，效果类似上面一句代码
tf.random.normal(shape=[3,4])   #创建一个形状为3行4列的张量，每个元素都是从标准正态分布（人工智能里常称标准高斯分布）中随机采样的值
tf.constant([[2, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]]) #使用一个python的列表初始化一个常量张量，该张量的值不可更改

tf.zeros_like(X)    #创建一个和X形状一样的元素全为0的张量
```

### 张量的运算

```python
1.相同形状的张量，直接使用四则运算则为对应位置上的元素进行四则运算
#例子：
    x = tf.constant([1.0, 2, 4, 8])
    y = tf.constant([2.0, 2, 2, 2])
    x + y, x - y, x * y, x / y, x ** y  # **运算符是求幂运算
#输出：tf.Tensor类型类似常量，可以改变形状但不能改变元素的值，Variable是可变容器，但TensorFlow中的梯度不能通过Variable反向传播
    (<tf.Tensor: shape=(4,), dtype=float32, numpy=array([ 3.,  4.,  6., 10.], dtype=float32)>,
     <tf.Tensor: shape=(4,), dtype=float32, numpy=array([-1.,  0.,  2.,  6.], dtype=float32)>,
     <tf.Tensor: shape=(4,), dtype=float32, numpy=array([ 2.,  4.,  8., 16.], dtype=float32)>,
     <tf.Tensor: shape=(4,), dtype=float32, numpy=array([0.5, 1. , 2. , 4. ], dtype=float32)>,
     <tf.Tensor: shape=(4,), dtype=float32, numpy=array([ 1.,  4., 16., 64.], dtype=float32)>)
    
2.合并张量，即把多个张量连结在一起，组成一个更大的张量
#例子：
    X = tf.reshape(tf.range(12, dtype=tf.float32), (3, 4))
    Y = tf.constant([[2.0, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
    tf.concat([X, Y], axis=0)   #axis=0指定沿轴-0（这里就是行），形状的第一个元素合并
    tf.concat([X, Y], axis=1)   #axis=1指定沿轴-1（这里就是列），形状的第二个元素合并
#输出：
    (<tf.Tensor: shape=(6, 4), dtype=float32, numpy=
     array([[ 0.,  1.,  2.,  3.],
            [ 4.,  5.,  6.,  7.],
            [ 8.,  9., 10., 11.],
            [ 2.,  1.,  4.,  3.],
            [ 1.,  2.,  3.,  4.],
            [ 4.,  3.,  2.,  1.]], dtype=float32)>,
     <tf.Tensor: shape=(3, 8), dtype=float32, numpy=
     array([[ 0.,  1.,  2.,  3.,  2.,  1.,  4.,  3.],
            [ 4.,  5.,  6.,  7.,  1.,  2.,  3.,  4.],
            [ 8.,  9., 10., 11.,  4.,  3.,  2.,  1.]], dtype=float32)>)
3.判断两个张量元素对应情况，直接使用 == ，需要两个张量的形状一样
#例子：
    x == Y
#输出：
    <tf.Tensor: shape=(3, 4), dtype=bool, numpy=
    array([[False,  True, False,  True],
           [False, False, False, False],
           [False, False, False, False]])>
4.对张量所有元素求和
#例子：
    tf.reduce_sum(x)
#输出：
    <tf.Tensor: shape=(), dtype=float32, numpy=66.0>
```

### 广播机制

```python
使用广播机制来实现两个不同形状的张量按元素操作，实际上，就是先将两个张量适当复制元素，使它们形状相同再进行操作
a = tf.reshape(tf.range(3), (3, 1))#a是3行1列的张量
b = tf.reshape(tf.range(2), (1, 2))#b是1行2列的张量
a+b
#输出：对a，b进行了扩展，把a的第一列复制了，放在a的第二列上，生成了一个3行2列的张量；把b的第一行复制了，放在b的第2行和第3行上，生成了一个3行2列的张量，然后将两形状相同的张量相加
    <tf.Tensor: shape=(3, 2), dtype=int32, numpy=
    array([[0, 1],
           [1, 2],
           [2, 3]])>
```

### 索引、切片、Variable（注意TensorFlow中的梯度不会通过Variable反向传播）

```python
1.张量和python中的数组类型变量一样，可以进行索引访问，第一个索引为0，最后一个为-1，多维张量注意索引使用类似二维数组的第一个元素是一个一维数组，不过多维是用的','如选中2行3列的元素为：x[2,3]，注意下标是从0开始的，也可以用x[2][3]表示但是会多打一个符号

x = tf.reshape(tf.range(12),(2,-1))
x[1]
#输出：
<tf.Tensor: shape=(6,), dtype=int32, numpy=array([ 6,  7,  8,  9, 10, 11])>

2.使用tf.Variable()将Tensor转换为Variable，进而修改Variable的值
x_v = tf.Variable(x)
x_v
x_v[1,2].assign(9)
#输出
    <tf.Variable 'Variable:0' shape=(2, 6) dtype=int32, numpy=
    array([[ 0,  1,  2,  3,  4,  5],
           [ 6,  7,  8,  9, 10, 11]])>
```

### 内存相关

```python
如果对张量直接使用赋值符号则会使张量重新指向另一片空间，但是使用x.assign(x+y)则张量会在原来的内存空间原地更新元素的值
可以使用id(变量名)查看张量的地址  #由于TensorFlow的Tensors不可变，而且梯度不能通过Variable传递，所以引入了tf.function修饰符，它会使TensorFlow进行优化，删除未使用的值，并复用之前分配但不再使用的值，减少TensorFlow计算的内存开销
```

### 对象间相互转换

```python
1.使用type(变量名)查看变量的类型
#如：type(tf.range(12))
<class 'tensorflow.python.framework.ops.EagerTensor'>

2.张量使用 变量名.numpy()转换为numpy的张量ndarray，使用tf.constant(变量名)将ndarray张量转换为tensorflow张量

3.如果是大小为1的张量则可以使用 变量名.item()来转换为Python的标量
#如：
    a = tf.constant([2.1]).numpy()
#输出：array([2.1], dtype=float32)
    a.item()
#输出：2.0999999046325684
```