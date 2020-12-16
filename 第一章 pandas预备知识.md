# 一、python基础

## 1、字符串运算  

字符串有很多运算方式，最直观的是字符串的 + 和 * 运算，分别表示连接和重复


```python
my_str = "Hello Python!"
my_str*2
```




    'Hello Python!Hello Python!'




```python
my_str + " you are so intersting!"
```




    'Hello Python! you are so intersting!'



如果要反向字符串，可以直接用 **[::-1]** 进行索引


```python
my_str[::-1]
```




    '!nohtyP olleH'



**join()方法** 

    若列表元素都是字符串，可用join()方法拼接字符串  


```python
str_list = ["that", "is", "so", "beautiful"]
print(" ".join(str_list) + "!")
```

    that is so beautiful!
    

## 2、列表推导式

常规方法生产一个数字序列


```python
def func(x):
    return x**2

my_list = []
for i in range(5):
    my_list.append(func(i))

my_list
```




    [0, 1, 4, 9, 16]



列表推导式的一般语法

>  [ expression for item in * ]

expression 为映射函数，item 为指代内容，* 为迭代对象

若需要加条件语句

>  [ expression if conditional for item in * ] 


```python
[func(i) for i in range(5)]
```




    [0, 1, 4, 9, 16]




```python
[func(i) if i > 1 else 0 for i in range(6)]
```




    [0, 0, 4, 9, 16, 25]



**推导式多层嵌套**，如下面的例子中第一个for为外层循环，第二个为内层循环：


```python
[m + "_" + n for m in ("a", "b") for n in ["x", "y"]]
```




    ['a_x', 'a_y', 'b_x', 'b_y']



**条件赋值**，其形式为  
> value = a if condition else b：


```python
value = "汪汪汪" if 2 in [1,2,3] else "喵喵喵"
value
```




    '汪汪汪'



**等价写法**


```python
a, b = "汪汪汪", "喵喵喵"
condition = 2 in [1,2,3]
if condition:
    value = a
else:
    value = b

value
```




    '汪汪汪'



## 3、匿名函数与map方法

匿名函数**Lambda**最常执行一些直观的运算，它并不需要标准的函数定义，而且也不需要新的函数名再次调用。


```python
my_func = lambda x: x**2
my_func(2)
```




    4



上述用法有违匿名函数本意，匿名函数一般不进行命名，而是直接使用


```python
list1 = [(lambda x: x**2)(i) for i in range(5)]
list1
```




    [0, 1, 4, 9, 16]



对于上述的这种列表推导式的匿名函数映射，Python中提供了**map函数**来完成，它返回的是一个**map对象**，需要通过list转为列表：


```python
list(map(lambda x: x**2, range(5)))
```




    [0, 1, 4, 9, 16]



对于多个输入值的函数映射，可以通过追加迭代对象实现：


```python
list(map(lambda x, y: str(x)+'_'+y, range(5), list('abcde')))
```




    ['0_a', '1_b', '2_c', '3_d', '4_e']



## 4、zip对象与enumerate方法

**zip** 可以将多个列表、 元组或其它序列成对组合成一个元组构成的可迭代对象，它返回了一个zip对象，通过tuple, list可以得到相应的打包结果：


```python
L1, L2, L3 = list("abc"), list("def"), list("gih")
list(zip(L1, L2, L3))
```




    [('a', 'd', 'g'), ('b', 'e', 'i'), ('c', 'f', 'h')]




```python
tuple(zip(L1, L2, L3))
```




    (('a', 'd', 'g'), ('b', 'e', 'i'), ('c', 'f', 'h'))



`zip` 可以处理任意多的序列， 元素的个数取决于最短的序列


```python
seq1 = ['foo', 'bar', 'baz']
seq2 = ['one', 'two', 'three', 'four']

list(zip(seq1, seq2))
```




    [('foo', 'one'), ('bar', 'two'), ('baz', 'three')]



`zip` 和 `*`搭配进行解压


```python
zipped = list(zip(seq1, seq2))
zipped
```




    [('foo', 'one'), ('bar', 'two'), ('baz', 'three')]




```python
list(zip(*zipped))
```




    [('foo', 'bar', 'baz'), ('one', 'two', 'three')]



【备注】解压只能返回原来压缩的内容seq2中的 "four" 由于不在压缩的迭代对象中，故无法返回

**`enumerate`** 是一种特殊的打包，它可以在迭代时绑定迭代元素的遍历序号：

> for i, value in enumerate(collection):

当你索引数据时， 使用 enumerate 的一个好方法是计算序列（ 唯一的） dict 映
射到位置的值：


```python
some_list = ['foo', 'bar', 'baz']
dic1 = {}
for nu, va in enumerate(some_list):
    dic1[va] = nu
dic1
```




    {'foo': 0, 'bar': 1, 'baz': 2}



`zip` 可以迭代多个序列，也可搭配enumerate使用


```python
se1 = ['foo', 'bar', 'baz']
se2 = ['one', 'two', 'three']
for i, (a, b) in enumerate(zip(se1, se2)):
    print('{0}: {1} {2}'.format(i, a, b))
```

    0: foo one
    1: bar two
    2: baz three
    

# 二、Numpy基础
## 1、np数组的构造
最一般的方法是通过`array`来构造：


```python
import numpy as np
```


```python
np.array([1, 2, 3])
```




    array([1, 2, 3])



### **特殊数组的生成方式：**

**【a】等差序列：`np.linspace`, `np.arange`**


```python
np.linspace(1,5,11) # 起始、终止（包含）、样本个数
```




    array([1. , 1.4, 1.8, 2.2, 2.6, 3. , 3.4, 3.8, 4.2, 4.6, 5. ])




```python
len(np.linspace(1,5,11))
```




    11




```python
np.arange(1,5,0.5) # 起始、终止（不包含）、步长
```




    array([1. , 1.5, 2. , 2.5, 3. , 3.5, 4. , 4.5])



**【b】特殊矩阵：`zeros`, `eye`, `full`**


```python
np.zeros((2,3)) # 传入元组表示各维度大小
```




    array([[0., 0., 0.],
           [0., 0., 0.]])




```python
np.eye(3) # 3*3的单位矩阵
```




    array([[1., 0., 0.],
           [0., 1., 0.],
           [0., 0., 1.]])




```python
np.eye(3, k=1) # 偏移主对角线1个单位的伪单位矩阵
```




    array([[0., 1., 0.],
           [0., 0., 1.],
           [0., 0., 0.]])




```python
np.full((2,3), 10) # 元组传入大小，10表示填充数值
```




    array([[10, 10, 10],
           [10, 10, 10]])




```python
np.full((2,3), [1,2,3]) # 通过传入列表填充每列的值
```




    array([[1, 2, 3],
           [1, 2, 3]])



**【c】随机矩阵：`np.random`**  

最常用的随机生成函数为**`rand`, `randn`, `randint`, `choice`** ，它们分别表示**0-1均匀分布的随机数组、标准正态的随机数组、随机整数组和随机列表抽样**：


```python
np.random.rand(3) # 生成服从0-1均匀分布的三个随机数
```




    array([0.78686194, 0.14150128, 0.66352379])




```python
np.random.rand(3, 3) # 注意这里传入的不是元组，每个维度大小分开输入
```




    array([[0.69321446, 0.02589279, 0.5177624 ],
           [0.86836806, 0.74895643, 0.44917273],
           [0.91446313, 0.23897637, 0.43715537]])



对于服从区间`a`到`b`上的均匀分布可以如下生成：


```python
a, b = 5, 15
(b-a)*np.random.rand(3) + a
```




    array([ 8.2544405 ,  8.38389049, 10.47486673])



**`randn`生成了N(0,I)**的标准正态分布：


```python
np.random.randn(3)
```




    array([ 0.99045925, -1.12818397, -0.98422731])




```python
np.random.randn(3,3)
```




    array([[ 1.0596823 ,  0.78790549, -0.27377982],
           [-0.56261307, -0.44039484, -0.96286794],
           [-1.79584687, -0.02533404,  0.20033518]])



对于服从方差为$\sigma^2$均值为$\mu$的一元正态分布可以如下生成：


```python
sigma, mu = 2.5, 3
mu + np.random.randn(3) * sigma
```




    array([-0.09098904,  1.50320743,  5.09355826])



**`randint`**可以指定生成随机整数的最小值最大值和维度大小：


```python
low, high, size = 5, 15, (2,2)
np.random.randint(low, high, size)
```




    array([[10,  5],
           [10,  7]])



**`choice`**可以从给定的列表中，以一定概率和方式抽取结果，当不指定概率时为均匀采样，默认抽取方式为有放回抽样：


```python
my_list = ['a', 'b', 'c', 'd']
np.random.choice(my_list, 2, replace=False, p=[0.1, 0.7, 0.1 ,0.1])
```




    array(['b', 'a'], dtype='<U1')




```python
np.random.choice(my_list, (3,3))
```




    array([['d', 'b', 'c'],
           ['b', 'b', 'c'],
           ['b', 'b', 'a']], dtype='<U1')



当返回的元素个数与原列表相同时，等价于使用**permutation**函数，即打散原列表：


```python
np.random.permutation(my_list)
```




    array(['a', 'c', 'd', 'b'], dtype='<U1')



最后，需要提到的是随机种子，它能够固定随机数的输出结果：


```python
np.random.seed(0)
np.random.rand()
```




    0.5488135039273248




```python
np.random.seed(0)
np.random.rand()
```




    0.5488135039273248



## 2、np数组的变形与合并  

**【a】转置：`T`**


```python
np.zeros((2,3)).T
```




    array([[0., 0.],
           [0., 0.],
           [0., 0.]])



**【b】合并操作：r_, c_**

对于二维数组而言，r_和c_分别表示上下合并和左右合并：


```python
np.r_[np.zeros((2,3)),np.zeros((2,3))]
```




    array([[0., 0., 0.],
           [0., 0., 0.],
           [0., 0., 0.],
           [0., 0., 0.]])




```python
np.c_[np.zeros((2,3)),np.random.randint(1,6,(2,3))]
```




    array([[0., 0., 0., 1., 1., 5.],
           [0., 0., 0., 3., 2., 1.]])



一维数组和二维数组进行合并时，应当把其视作列向量，在长度匹配的情况下只能够使用左右合并的c_操作：


```python
try:
     np.r_[np.array([0,0]),np.zeros((2,1))]
except Exception as e:
     Err_Msg = e
Err_Msg
```




    ValueError('all the input arrays must have same number of dimensions, but the array at index 0 has 1 dimension(s) and the array at index 1 has 2 dimension(s)')




```python
np.c_[np.random.rand(2), np.zeros(2)]
```




    array([[0.21655035, 0.        ],
           [0.13521817, 0.        ]])




```python
np.c_[np.array([1,2]),np.zeros(2)]
```




    array([[1., 0.],
           [2., 0.]])




```python
np.r_[np.array([1,2]),np.zeros(2)]
```




    array([1., 2., 0., 0.])




```python
np.c_[np.array([0,0]),np.zeros((2,3))]
```




    array([[0., 0., 0., 0.],
           [0., 0., 0., 0.]])



**【c】维度变换：reshape**

reshape能够帮助用户把原数组按照新的维度重新排列。在使用时有两种模式，分别为C模式和F模式，分别以逐行和逐列的顺序进行填充读取。


```python
arr1 = np.arange(8).reshape(2,4)

arr1
```




    array([[0, 1, 2, 3],
           [4, 5, 6, 7]])




```python
arr1.reshape(4,2)
```




    array([[0, 1],
           [2, 3],
           [4, 5],
           [6, 7]])



按照行读取和填充


```python
arr1.reshape((4,2), order='C')
```




    array([[0, 1],
           [2, 3],
           [4, 5],
           [6, 7]])



按照列读取和填充


```python
arr1.reshape((4,2), order='F')
```




    array([[0, 2],
           [4, 6],
           [1, 3],
           [5, 7]])



特别地，由于被调用数组的大小是确定的，reshape允许有一个维度存在空缺，此时只需填充-1即可：


```python
arr1.reshape(2, -1)
```




    array([[0, 1, 2, 3],
           [4, 5, 6, 7]])



下面将n*1大小的数组转为1维数组的操作是经常使用的：


```python
arr2 = np.ones((3,1))
arr2
```




    array([[1.],
           [1.],
           [1.]])




```python
arr2.reshape(-1)
```




    array([1., 1., 1.])



## 3、np数组的切片与索引

数组的切片模式支持使用**slice**类型的**`start:end:step`**切片，还可以直接传入列表指定某个维度的索引进行切片：


```python
arr1 = np.arange(9).reshape(3,3)
arr1
```




    array([[0, 1, 2],
           [3, 4, 5],
           [6, 7, 8]])




```python
arr1[:-1, [0,2]]  # 最后一行除外，第0和第2列
```




    array([[0, 2],
           [3, 5]])



此外，还可以利用**np.ix_**在对应的维度上使用布尔索引，但此时不能使用slice切片：


```python
arr1[np.ix_([True, False, True], [True, False, True])] # 行列都为True时返回对应位置的值
```




    array([[0, 2],
           [6, 8]])




```python
arr1[np.ix_([True, False, True], [True, False, False])] # 行列不同时为True时，不返回
```




    array([[0],
           [6]])




```python
arr1[np.ix_([1,2], [True, False, True])]
```




    array([[3, 5],
           [6, 8]])



当数组维度为1维时，可以直接进行布尔索引，而无需np.ix_：


```python
new = arr1.reshape(-1)
new[new%2==0]
```




    array([0, 2, 4, 6, 8])



## 4、常用函数

**【a】where**

where是一种条件函数，可以指定满足条件与不满足条件位置对应的填充值：


```python
a = np.array([-1,1,-1,0])
np.where(a>0, a, 5) # 对应位置为True时填充a对应元素，否则填充5
```




    array([5, 1, 5, 5])



**【b】`nonzero`, `argmax`, `argmin`**

这三个函数返回的都是索引，`nonzero`返回非零数的索引，`argmax`, `argmin`分别返回最大和最小数的索引：


```python
a = np.array([-2,-5,0,1,3,-1])
np.nonzero(a)
```




    (array([0, 1, 3, 4, 5], dtype=int64),)




```python
a.argmax()
```




    4




```python
a.argmin()
```




    1



**【c】`any`, `all`**

`any`指当序列至少 **存在一个** `True`或非零元素时返回`True`，否则返回`False`

`all`指当序列元素 **全为** `True`或非零元素时返回`True`，否则返回`False`


```python
a = np.array([0,1])
a.any()
```




    True




```python
 a.all()
```




    False



**【d】`cumprod`, `cumsum`, `diff`**

`cumprod`, `cumsum`分别表示累乘和累加函数，返回同长度的数组，`diff`表示和前一个元素做差，  
由于第一个元素为缺失值，因此在默认参数情况下，返回长度是原数组减1


```python
a = np.array([1,2,3,5])
a.cumprod()
```




    array([ 1,  2,  6, 30], dtype=int32)




```python
a.cumsum()
```




    array([ 1,  3,  6, 11], dtype=int32)




```python
np.diff(a)
```




    array([1, 1, 2])



**【e】 统计函数**

常用的统计函数包括`max, min, mean, median(中位数), std(标准差), var(方差), sum, quantile(分位数)`，其中分位数计算是全局方法，因此不能通过`array.quantile`的方法调用：


```python
target = np.arange(1,10).reshape(3,-1)
target
```




    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])




```python
target.sum(0)  # 0-各列求和；
```




    array([12, 15, 18])




```python
target.sum(1)  # 1-各行求和
```




    array([ 6, 15, 24])



## 5、广播机制

广播机制用于处理两个不同维度数组之间的操作，这里只讨论不超过两维的数组广播机制。

**【a】标量和数组的操作**

当一个标量和数组进行运算时，标量会自动把大小扩充为数组大小，之后进行逐元素操作：


```python
res = 3 * np.ones((2,2)) + 1
res
```




    array([[4., 4.],
           [4., 4.]])




```python
res = 1 / res
res
```




    array([[0.25, 0.25],
           [0.25, 0.25]])



**【b】二维数组之间的操作**

当两个数组维度完全一致时，使用对应元素的操作，否则会报错，除非其中的某个数组的维度是 𝑚×1 或者 1×𝑛 ，那么会扩充其具有 1 的维度为另一个数组对应维度的大小。例如， 1×2 数组和 3×2 数组做逐元素运算时会把第一个数组扩充为 3×2 ，扩充时的对应数值进行赋值。但是，需要注意的是，如果第一个数组的维度是 1×3 ，那么由于在第二维上的大小不匹配且不为 1 ，此时报错。


```python
res = np.ones((3,2))
res
```




    array([[1., 1.],
           [1., 1.],
           [1., 1.]])




```python
res * np.array([[2,3]]) # 扩充第一维度为3
```




    array([[2., 3.],
           [2., 3.],
           [2., 3.]])




```python
res * np.array([[2],[3],[4]]) # 扩充第二维度为2
```




    array([[2., 2.],
           [3., 3.],
           [4., 4.]])




```python
res * np.array([[2]]) # 等价于两次扩充
```




    array([[2., 2.],
           [2., 2.],
           [2., 2.]])



**【c】一维数组与二维数组的操作**

当一维数组$A_k$与二维数组$B_{m,n}$操作时，等价于把一维数组视作$A_{1,k}$的二维数组，使用的广播法则与【b】中一致，当$k!=n$且$k,n$都不是$1$时报错。


```python
np.ones(3) + np.ones((2,3))
```




    array([[2., 2., 2.],
           [2., 2., 2.]])




```python
np.ones(3) + np.ones((2,1))
```




    array([[2., 2., 2.],
           [2., 2., 2.]])




```python
np.ones(1) + np.ones((2,3))
```




    array([[2., 2., 2.],
           [2., 2., 2.]])



## 6、向量与矩阵的计算
**【a】向量内积：`dot`**

$$\rm \mathbf{a}\cdot\mathbf{b} = \sum_ia_ib_i$$


```python
a = np.array([1,2,3])
b = np.array([1,3,5])
a.dot(b)
```




    22



**【b】向量范数和矩阵范数：`np.linalg.norm`**

在矩阵范数的计算中，最重要的是`ord`参数，可选值如下：

| ord | norm for matrices | norm for vectors |
| :---- | ----: | ----: |
| None   | Frobenius norm | 2-norm |
| 'fro'  | Frobenius norm  | / |
| 'nuc'  | nuclear norm    | / |
| inf    | max(sum(abs(x), axis=1))   | max(abs(x)) |
| -inf   | min(sum(abs(x), axis=1))  |  min(abs(x)) |
| 0      | /   |  sum(x != 0) |
| 1      | max(sum(abs(x), axis=0))  |  as below |
| -1     | min(sum(abs(x), axis=0))   |  as below |
| 2      | 2-norm (largest sing. value) | as below |
| -2     | smallest singular value    | as below |
| other  | /   | sum(abs(x)**ord)**(1./ord) |


```python
martix_target =  np.arange(4).reshape(-1,2)
martix_target
```




    array([[0, 1],
           [2, 3]])




```python
np.linalg.norm(martix_target, 'fro')
```




    3.7416573867739413




```python
np.linalg.norm(martix_target, np.inf)
```




    5.0




```python
np.linalg.norm(martix_target, 2)
```




    3.702459173643833




```python
vector_target =  np.arange(4)
vector_target
```




    array([0, 1, 2, 3])




```python
np.linalg.norm(vector_target, np.inf)
```




    3.0




```python
np.linalg.norm(vector_target, 2)
```




    3.7416573867739413




```python
np.linalg.norm(vector_target, 3)
```




    3.3019272488946263



**【c】矩阵乘法：`@`**

$$\rm [\mathbf{A}_{m\times p}\mathbf{B}_{p\times n}]_{ij} = \sum_{k=1}^p\mathbf{A}_{ik}\mathbf{B}_{kj}$$


```python
a = np.arange(4).reshape(-1,2)
a
```




    array([[0, 1],
           [2, 3]])




```python
b = np.arange(-4,0).reshape(-1,2)
b
```




    array([[-4, -3],
           [-2, -1]])




```python
a@b
```




    array([[ -2,  -1],
           [-14,  -9]])



# 三、练习

## Ex1：利用列表推导式写矩阵乘法

一般的矩阵乘法根据公式，可以由三重循环写出，请将其改写为列表推导式的形式。


```python
M1 = np.random.rand(2,3)
M2 = np.random.rand(3,4)
res = np.empty((M1.shape[0],M2.shape[1]))
for i in range(M1.shape[0]):
    for j in range(M2.shape[1]):
        item = 0
        for k in range(M1.shape[1]):
            item += M1[i][k] * M2[k][j]
        res[i][j] = item
((M1@M2 - res) < 1e-15).all() # 排除数值误差
```




    True



**【答案】**


```python
M1 = np.random.rand(2,3)
M2 = np.random.rand(3,4)
res = [[sum([M1[i][k] * M2[k][j] for k in range(M1.shape[1])]) for j in range(M2.shape[1])] for i in range(M1.shape[0])]
((M1@M2 - res) < 1e-15).all()
```




    True



### Ex2：更新矩阵
设矩阵 $A_{m×n}$ ，现在对 $A$ 中的每一个元素进行更新生成矩阵 $B$ ，更新方法是 $B_{ij}=A_{ij}\sum_{k=1}^n\frac{1}{A_{ik}}$ ，例如下面的矩阵为 $A$ ，则 $B_{2,2}=5\times(\frac{1}{4}+\frac{1}{5}+\frac{1}{6})=\frac{37}{12}$ ，请利用 `Numpy` 高效实现。
$$\begin{split}A=\left[ \begin{matrix} 1 & 2 &3\\4&5&6\\7&8&9 \end{matrix} \right]\end{split}$$

**【答案】**


```python
A = np.arange(1,10).reshape(3,-1)
B = A*(1/A).sum(1).reshape(-1,1)  
B
```




    array([[1.83333333, 3.66666667, 5.5       ],
           [2.46666667, 3.08333333, 3.7       ],
           [2.65277778, 3.03174603, 3.41071429]])



**上述过程拆分如下：**


```python
A = np.arange(1,10).reshape(3,-1)   
C = (1/A)         # 将A矩阵中所有元素倒装
D = C.sum(1)      # 将上述矩阵各行求和，获得新的向量
E = D.reshape(-1,1) # 转换为$A_n1$ 
E
```




    array([[1.83333333],
           [0.61666667],
           [0.37896825]])



利用广播机制，将原矩阵与新矩阵中的元素分别相乘,**注意不是矩阵乘法**


```python
A*E
```




    array([[1.83333333, 3.66666667, 5.5       ],
           [2.46666667, 3.08333333, 3.7       ],
           [2.65277778, 3.03174603, 3.41071429]])



## Ex3：卡方统计量

设矩阵$A_{m\times n}$，记$B_{ij} = \frac{(\sum_{i=1}^mA_{ij})\times (\sum_{j=1}^nA_{ij})}{\sum_{i=1}^m\sum_{i=1}^nA_{ij}}$，定义卡方值如下：
$$\chi^2 = \sum_{i=1}^m\sum_{j=1}^n\frac{(A_{ij}-B_{ij})^2}{B_{ij}}$$
请利用`Numpy`对给定的矩阵$A$计算$\chi^2$ 


```python
np.random.seed(0)
A = np.random.randint(10, 20, (8, 5))
```

**【答案】**


```python
np.random.seed(0)
A = np.random.randint(10, 20, (8, 5))
B = A.sum(0)*A.sum(1).reshape(-1, 1)/A.sum()
res = ((A-B)**2/B).sum()
res
```




    11.842696601945802



## Ex4：改进矩阵计算的性能
设$Z$为$m×n$的矩阵，$B$和$U$分别是$m×p$和$p×n$的矩阵，$B_i$为$B$的第$i$行，$U_j$为$U$的第$j$列，下面定义$\displaystyle R=\sum_{i=1}^m\sum_{j=1}^n\|B_i-U_j\|_2^2Z_{ij}$，其中$\|\mathbf{a}\|_2^2$表示向量$a$的分量平方和$\sum_i a_i^2$。

现有某人根据如下给定的样例数据计算$R$的值，请充分利用`Numpy`中的函数，基于此问题改进这段代码的性能。


```python
np.random.seed(0)
m, n, p = 100, 80, 50
B = np.random.randint(0, 2, (m, p))
U = np.random.randint(0, 2, (p, n))
Z = np.random.randint(0, 2, (m, n))
def solution(B=B, U=U, Z=Z):
    L_res = []
    for i in range(m):
        for j in range(n):
            norm_value = ((B[i]-U[:,j])**2).sum()
            L_res.append(norm_value*Z[i][j])
    return sum(L_res)
solution(B, U, Z)
```




    100566



**【答案】**

改进方法：

令$Y_{ij} = \|B_i-U_j\|_2^2$，则$\displaystyle R=\sum_{i=1}^m\sum_{j=1}^n Y_{ij}Z_{ij}$，这在`Numpy`中可以用逐元素的乘法后求和实现，因此问题转化为了如何构造`Y`矩阵。

$$
\begin{split}Y_{ij} &= \|B_i-U_j\|_2^2\\
&=\sum_{k=1}^p(B_{ik}-U_{kj})^2\\
&=\sum_{k=1}^p B_{ik}^2+\sum_{k=1}^p U_{kj}^2-2\sum_{k=1}^p B_{ik}U_{kj}\\\end{split}
$$

从上式可以看出，第一第二项分别为$B$的行平方和与$U$的列平方和，第三项是两倍的内积。因此，$Y$矩阵可以写为三个部分，第一个部分是$m×n$的全$1$矩阵每行乘以$B$对应行的行平方和，第二个部分是相同大小的全$1$矩阵每列乘以$U$对应列的列平方和，第三个部分恰为$B$矩阵与$U$矩阵乘积的两倍。从而结果如下：


```python
(((B**2).sum(1).reshape(-1,1) + (U**2).sum(0) - 2*B@U)*Z).sum()
```




    100566



对比它们的性能：


```python
%timeit -n 30 solution(B, U, Z)
```

    60.4 ms ± 979 µs per loop (mean ± std. dev. of 7 runs, 30 loops each)
    


```python
%timeit -n 30 ((np.ones((m,n))*(B**2).sum(1).reshape(-1,1) + np.ones((m,n))*(U**2).sum(0) - 2*B@U)*Z).sum()
```

    717 µs ± 98.5 µs per loop (mean ± std. dev. of 7 runs, 30 loops each)
    

## Ex5：连续整数的最大长度

输入一个整数的`Numpy`数组，返回其中递增连续整数子数组的最大长度，正向是指递增方向。例如，输入\[1,2,5,6,7\]，\[5,6,7\]为具有最大长度的连续整数子数组，因此输出3；输入\[3,2,1,2,3,4,6\]，\[1,2,3,4\]为具有最大长度的连续整数子数组，因此输出4。请充分利用`Numpy`的内置函数完成。（提示：考虑使用`nonzero, diff`函数）


```python
f = lambda x:np.diff(np.nonzero(np.r_[1,np.diff(x)!=1,1])).max()
f([1,2,5,6,7])
f([3,2,1,2,3,4,6])
```




    4


