## 【任务一】企业收入的多样性

【题目描述】一个企业的产业收入多样性可以仿照信息熵的概念来定义收入熵指标：

$$\mathrm{I}=-\sum_{\mathrm{i}} \mathrm{p}\left(\mathrm{x}_{\mathrm{i}}\right) \log \left(\mathrm{p}\left(\mathrm{x}_{\mathrm{i}}\right)\right)$$

 是企业该年某产业收入额占该年所有产业总收入的比重。在`company.csv`中存有需要计算的企业和年份，在`company_data.csv`中存有企业、各类收入额和收入年份的信息。现请利用后一张表中的数据，在前一张表中增加一列表示该公司该年份的收入熵指标 
$ I $
 。


```python
import pandas as pd
df1 = pd.read_csv('company.csv')
df1.head()
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
      <th>证券代码</th>
      <th>日期</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>#000007</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>1</th>
      <td>#000403</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>2</th>
      <td>#000408</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>3</th>
      <td>#000408</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>4</th>
      <td>#000426</td>
      <td>2015</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = pd.read_csv('company_data.csv')
df2.tail()
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
      <th>证券代码</th>
      <th>日期</th>
      <th>收入类型</th>
      <th>收入额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>964017</th>
      <td>900957</td>
      <td>2016/12/31</td>
      <td>12</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>964018</th>
      <td>900957</td>
      <td>2016/12/31</td>
      <td>13</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>964019</th>
      <td>900957</td>
      <td>2016/12/31</td>
      <td>14</td>
      <td>52072238.97</td>
    </tr>
    <tr>
      <th>964020</th>
      <td>900957</td>
      <td>2016/12/31</td>
      <td>15</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>964021</th>
      <td>900957</td>
      <td>2016/12/31</td>
      <td>16</td>
      <td>52072238.97</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1['证券代码']=df1['证券代码'].apply(lambda x:x.strip('#').lstrip('0'))
df1.head()
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
      <th>证券代码</th>
      <th>日期</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>1</th>
      <td>403</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>2</th>
      <td>408</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>3</th>
      <td>408</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>4</th>
      <td>426</td>
      <td>2015</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2['年份']=df2['日期'].str[:4]
df2.head()
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
      <th>证券代码</th>
      <th>日期</th>
      <th>收入类型</th>
      <th>收入额</th>
      <th>年份</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2008/12/31</td>
      <td>1</td>
      <td>1.084218e+10</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2008/12/31</td>
      <td>2</td>
      <td>1.259789e+10</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2008/12/31</td>
      <td>3</td>
      <td>1.451312e+10</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>2008/12/31</td>
      <td>4</td>
      <td>1.063843e+09</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2008/12/31</td>
      <td>5</td>
      <td>8.513880e+08</td>
      <td>2008</td>
    </tr>
  </tbody>
</table>
</div>



后续的根据上述公式计算，这部分没学好，前面没来得及，得再复习
