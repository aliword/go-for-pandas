# 一、长宽表的变形

变形函数

## 1. pivot

参数：
>index -- 用于制作新框架索引的列。  
columns -- 用于制作新框架列的列。  
values -- 用于填充新框架值的列  

新生成表的列索引是 columns 对应列的 unique 值，而新表的行索引是 index 对应列的 unique 值，而 values 对应了想要展示的数值列,pivot 相关的三个参数允许被设置为列表

**满足唯一性的要求:** 原表中的 index 和 columns 对应两个列的行组合必须唯一


```python
import pandas as pd
df = pd.DataFrame({'Class':[1, 1, 2, 2, 1, 1, 2, 2],
                   'Name':['San Zhang', 'San Zhang', 'Si Li', 'Si Li',
                           'San Zhang', 'San Zhang', 'Si Li', 'Si Li'],
                   'Examination': ['Mid', 'Final', 'Mid', 'Final',
                                   'Mid', 'Final', 'Mid', 'Final'],
                   'Subject':['Chinese', 'Chinese', 'Chinese', 'Chinese',
                              'Math', 'Math', 'Math', 'Math'],
                   'Grade':[80, 75, 85, 65, 90, 85, 92, 88],
                   'rank':[10, 15, 21, 15, 20, 7, 6, 2]})

df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Class</th>
      <th>Name</th>
      <th>Examination</th>
      <th>Subject</th>
      <th>Grade</th>
      <th>rank</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>San Zhang</td>
      <td>Mid</td>
      <td>Chinese</td>
      <td>80</td>
      <td>10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>San Zhang</td>
      <td>Final</td>
      <td>Chinese</td>
      <td>75</td>
      <td>15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Si Li</td>
      <td>Mid</td>
      <td>Chinese</td>
      <td>85</td>
      <td>21</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>Si Li</td>
      <td>Final</td>
      <td>Chinese</td>
      <td>65</td>
      <td>15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>San Zhang</td>
      <td>Mid</td>
      <td>Math</td>
      <td>90</td>
      <td>20</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1</td>
      <td>San Zhang</td>
      <td>Final</td>
      <td>Math</td>
      <td>85</td>
      <td>7</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2</td>
      <td>Si Li</td>
      <td>Mid</td>
      <td>Math</td>
      <td>92</td>
      <td>6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2</td>
      <td>Si Li</td>
      <td>Final</td>
      <td>Math</td>
      <td>88</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



把测试类型和科目联合组成的四个类别（期中语文、期末语文、期中数学、期末数学）转到列索引，并且同时统计成绩和排名：


```python
pivot_multi = df.pivot(index = ['Class', 'Name'],
                       columns = ['Subject','Examination'],
                       values = ['Grade','rank'])

pivot_multi
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">Grade</th>
      <th colspan="4" halign="left">rank</th>
    </tr>
    <tr>
      <th></th>
      <th>Subject</th>
      <th colspan="2" halign="left">Chinese</th>
      <th colspan="2" halign="left">Math</th>
      <th colspan="2" halign="left">Chinese</th>
      <th colspan="2" halign="left">Math</th>
    </tr>
    <tr>
      <th></th>
      <th>Examination</th>
      <th>Mid</th>
      <th>Final</th>
      <th>Mid</th>
      <th>Final</th>
      <th>Mid</th>
      <th>Final</th>
      <th>Mid</th>
      <th>Final</th>
    </tr>
    <tr>
      <th>Class</th>
      <th>Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <th>San Zhang</th>
      <td>80</td>
      <td>75</td>
      <td>90</td>
      <td>85</td>
      <td>10</td>
      <td>15</td>
      <td>20</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <th>Si Li</th>
      <td>85</td>
      <td>65</td>
      <td>92</td>
      <td>88</td>
      <td>21</td>
      <td>15</td>
      <td>6</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



新表的行索引等价于对 index 中的多列使用 drop_duplicates ，而列索引的长度为 values 中的元素个数乘以 columns 的唯一组合数量（与 index 类似） 

## 2. pivot_table

pivot 的使用依赖于唯一性条件, pivot_table 可以实现聚合功能


```python
df.pivot_table(index = 'Name',
               columns = 'Subject',
               values = 'Grade',
               aggfunc = 'mean') ## aggfunc 参数就是使用的聚合函数
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Subject</th>
      <th>Chinese</th>
      <th>Math</th>
    </tr>
    <tr>
      <th>Name</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>San Zhang</th>
      <td>77.5</td>
      <td>87.5</td>
    </tr>
    <tr>
      <th>Si Li</th>
      <td>75.0</td>
      <td>90.0</td>
    </tr>
  </tbody>
</table>
</div>



 pivot_table 具有边际汇总的功能，可以通过设置 margins=True 

## 3. melt

通过相应的逆操作把宽表转为长表  
在列索引中被压缩的一组值对应的列元素只能代表同一层次的含义，即 values_name

## 4. wide_to_long

如果列中包含了交叉类别，比如期中期末的类别和语文数学的类别，那么想要把 values_name 对应的 Grade 扩充为两列分别对应语文分数和数学分数，只把期中期末的信息压缩，这种需求下就要使用 wide_to_long 函数来完成。


```python
df = pd.DataFrame({'Class':[1,2],'Name':['San Zhang', 'Si Li'],
                   'Chinese_Mid':[80, 75], 'Math_Mid':[90, 85],
                   'Chinese_Final':[80, 75], 'Math_Final':[90, 85]})

df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Class</th>
      <th>Name</th>
      <th>Chinese_Mid</th>
      <th>Math_Mid</th>
      <th>Chinese_Final</th>
      <th>Math_Final</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>San Zhang</td>
      <td>80</td>
      <td>90</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Si Li</td>
      <td>75</td>
      <td>85</td>
      <td>75</td>
      <td>85</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.wide_to_long(df,
                stubnames=['Chinese', 'Math'],
                i = ['Class', 'Name'],
                j='Examination',
                sep='_',
                suffix='.+')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th>Chinese</th>
      <th>Math</th>
    </tr>
    <tr>
      <th>Class</th>
      <th>Name</th>
      <th>Examination</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">1</th>
      <th rowspan="2" valign="top">San Zhang</th>
      <th>Mid</th>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>Final</th>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2</th>
      <th rowspan="2" valign="top">Si Li</th>
      <th>Mid</th>
      <td>75</td>
      <td>85</td>
    </tr>
    <tr>
      <th>Final</th>
      <td>75</td>
      <td>85</td>
    </tr>
  </tbody>
</table>
</div>



# 二、索引的变形

## 1. stack与unstack

行列索引之间 的交换

* unstack 函数的作用是把行索引转为列索引

unstack 的主要参数是移动的层号，默认转化最内层，移动到列索引的**最内层**，同时支持同时转化多个层

```python
df.unstack(2) #转换第三层
df.unstack([0,2]) #转换第一、三层
```
 unstack 中必须保证 被转为列索引的行索引层 和 被保留的行索引层 构成的组合是唯一的

* stack 的作用就是把列索引的层压入行索引，其用法完全类似

## 2. 聚合与变形的关系

除了带有聚合效果的 pivot_table 以外，所有的函数在变形前后并不会带来 values 个数的改变，只是这些值在呈现的形式上发生了变化；但由于聚合之后把原来的多个值变为了一个值，因此 values 的个数产生了变化，这也是分组聚合与变形函数的最大区别。

# 三、其他变形函数

## 1. crosstab

 不推荐使用，crosstab 可以统计元素组合出现的频数，即 count 操作

 crosstab 的对应位置传入的是具体的序列，而 pivot_table 传入的是被调用表对应的名字

## 2. explode

explode 参数能够对某一列的元素进行纵向的展开，被展开的单元格必须存储 list, tuple, Series, np.ndarray 中的一种类型。


```python
df_ex = pd.DataFrame({'A': [[1, 2],'my_str', {1, 2},pd.Series([3, 4])],'B': 1})
df_ex
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[1, 2]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>my_str</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>{1, 2}</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0    3
1    4
dtype: int64</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_ex.explode('A')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>my_str</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>{1, 2}</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



## 3. get_dummies

get_dummies 是用于特征构建的重要函数之一，其作用是把类别特征转为指示变量。
