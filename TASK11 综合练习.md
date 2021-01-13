## 【任务一】企业收入的多样性


```python
import pandas as pd
import numpy as np

df1 = pd.read_csv('company.csv')
df2 = pd.read_csv('company_data.csv')
```


```python
df1
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
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1043</th>
      <td>#600978</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>1044</th>
      <td>#600978</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>1045</th>
      <td>#600978</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>1046</th>
      <td>#600978</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>1047</th>
      <td>#600978</td>
      <td>2017</td>
    </tr>
  </tbody>
</table>
<p>1048 rows × 2 columns</p>
</div>




```python
df2
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
      <th>0</th>
      <td>1</td>
      <td>2008/12/31</td>
      <td>1</td>
      <td>1.084218e+10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2008/12/31</td>
      <td>2</td>
      <td>1.259789e+10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2008/12/31</td>
      <td>3</td>
      <td>1.451312e+10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>2008/12/31</td>
      <td>4</td>
      <td>1.063843e+09</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2008/12/31</td>
      <td>5</td>
      <td>8.513880e+08</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>964017</th>
      <td>900957</td>
      <td>2016/12/31</td>
      <td>12</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>964018</th>
      <td>900957</td>
      <td>2016/12/31</td>
      <td>13</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>964019</th>
      <td>900957</td>
      <td>2016/12/31</td>
      <td>14</td>
      <td>5.207224e+07</td>
    </tr>
    <tr>
      <th>964020</th>
      <td>900957</td>
      <td>2016/12/31</td>
      <td>15</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>964021</th>
      <td>900957</td>
      <td>2016/12/31</td>
      <td>16</td>
      <td>5.207224e+07</td>
    </tr>
  </tbody>
</table>
<p>964022 rows × 4 columns</p>
</div>




```python
df2['证券代码'] = df2['证券代码'].map(lambda x:("#"+"0"*(6-len(str(x)))+str(x)))
df2 = df2[df2['证券代码'].isin(df1['证券代码'])]
df2['日期'] = df2['日期'].apply(lambda x: int(x[:4]))
res = df2.groupby(['证券代码', '日期'])['收入额']
res = res.apply(lambda x: -((x/x.sum()*np.log(x/x.sum()))).sum()).reset_index()
res = df1.merge(res, how='left', on=['证券代码', '日期']).rename(columns={'收入额': '收入熵'})
res
```

    <ipython-input-32-638af8a0b71f>:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df2['日期'] = df2['日期'].apply(lambda x: int(x[:4]))
    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\series.py:726: RuntimeWarning: divide by zero encountered in log
      result = getattr(ufunc, method)(*inputs, **kwargs)
    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\series.py:726: RuntimeWarning: invalid value encountered in log
      result = getattr(ufunc, method)(*inputs, **kwargs)
    




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
      <th>收入熵</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>#000007</td>
      <td>2014</td>
      <td>3.070462</td>
    </tr>
    <tr>
      <th>1</th>
      <td>#000403</td>
      <td>2015</td>
      <td>2.790585</td>
    </tr>
    <tr>
      <th>2</th>
      <td>#000408</td>
      <td>2016</td>
      <td>2.818541</td>
    </tr>
    <tr>
      <th>3</th>
      <td>#000408</td>
      <td>2017</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>#000426</td>
      <td>2015</td>
      <td>3.084266</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1043</th>
      <td>#600978</td>
      <td>2011</td>
      <td>3.319059</td>
    </tr>
    <tr>
      <th>1044</th>
      <td>#600978</td>
      <td>2014</td>
      <td>2.788100</td>
    </tr>
    <tr>
      <th>1045</th>
      <td>#600978</td>
      <td>2015</td>
      <td>3.012628</td>
    </tr>
    <tr>
      <th>1046</th>
      <td>#600978</td>
      <td>2016</td>
      <td>3.021157</td>
    </tr>
    <tr>
      <th>1047</th>
      <td>#600978</td>
      <td>2017</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>1048 rows × 3 columns</p>
</div>



## 【任务二】组队学习信息表的变换


```python
df = pd.read_excel('组队信息汇总表（Pandas）.xlsx')
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
      <th>所在群</th>
      <th>队伍名称</th>
      <th>队长编号</th>
      <th>队长_群昵称</th>
      <th>队员1 编号</th>
      <th>队员_群昵称</th>
      <th>队员2 编号</th>
      <th>队员_群昵称.1</th>
      <th>队员3 编号</th>
      <th>队员_群昵称.2</th>
      <th>...</th>
      <th>队员6 编号</th>
      <th>队员_群昵称.5</th>
      <th>队员7 编号</th>
      <th>队员_群昵称.6</th>
      <th>队员8 编号</th>
      <th>队员_群昵称.7</th>
      <th>队员9 编号</th>
      <th>队员_群昵称.8</th>
      <th>队员10编号</th>
      <th>队员_群昵称.9</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Pandas数据分析</td>
      <td>你说的都对队</td>
      <td>5</td>
      <td>山枫叶纷飞</td>
      <td>6</td>
      <td>蔡</td>
      <td>7.0</td>
      <td>安慕希</td>
      <td>8.0</td>
      <td>信仰</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Pandas数据分析</td>
      <td>熊猫人</td>
      <td>175</td>
      <td>鱼呲呲</td>
      <td>44</td>
      <td>Heaven</td>
      <td>37.0</td>
      <td>吕青</td>
      <td>50.0</td>
      <td>余柳成荫</td>
      <td>...</td>
      <td>25.0</td>
      <td>Never say never</td>
      <td>55.0</td>
      <td>K</td>
      <td>120.0</td>
      <td>Y.</td>
      <td>28.0</td>
      <td>X.Y.Q</td>
      <td>151.0</td>
      <td>swrong</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Pandas数据分析</td>
      <td>中国移不动</td>
      <td>107</td>
      <td>Y's</td>
      <td>124</td>
      <td>🥕</td>
      <td>75.0</td>
      <td>Vito</td>
      <td>146.0</td>
      <td>张小五</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Pandas数据分析</td>
      <td>panda</td>
      <td>11</td>
      <td>太下真君</td>
      <td>35</td>
      <td>柚子</td>
      <td>108.0</td>
      <td>My</td>
      <td>42.0</td>
      <td>星星点灯</td>
      <td>...</td>
      <td>157.0</td>
      <td>Zys</td>
      <td>158.0</td>
      <td>不器</td>
      <td>102.0</td>
      <td>嘉平佑染</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pandas数据分析</td>
      <td>一路向北</td>
      <td>13</td>
      <td>黄元帅</td>
      <td>15</td>
      <td>化</td>
      <td>16.0</td>
      <td>未期</td>
      <td>18.0</td>
      <td>太陽光下</td>
      <td>...</td>
      <td>23.0</td>
      <td>🚀</td>
      <td>169.0</td>
      <td>听风</td>
      <td>189.0</td>
      <td>Cappuccino</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Pandas数据分析</td>
      <td>西部战车</td>
      <td>149</td>
      <td>老狼</td>
      <td>51</td>
      <td>Robin or Michael</td>
      <td>72.0</td>
      <td>慕安春临</td>
      <td>160.0</td>
      <td>哦豁</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Pandas数据分析</td>
      <td>Why—贰肆</td>
      <td>48</td>
      <td>Cliff</td>
      <td>81</td>
      <td>ww</td>
      <td>147.0</td>
      <td>小清新</td>
      <td>47.0</td>
      <td>哈桑的雨天</td>
      <td>...</td>
      <td>119.0</td>
      <td>可口可乐与果粒橙</td>
      <td>92.0</td>
      <td>一代天骄</td>
      <td>139.0</td>
      <td>xg</td>
      <td>1.0</td>
      <td>MoXQian</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Pandas数据分析</td>
      <td>师承潘大师队</td>
      <td>33</td>
      <td>Tango</td>
      <td>49</td>
      <td>拿铁</td>
      <td>100.0</td>
      <td>懵懂出茅庐</td>
      <td>52.0</td>
      <td>Esther</td>
      <td>...</td>
      <td>38.0</td>
      <td>栗子</td>
      <td>162.0</td>
      <td>落叶</td>
      <td>150.0</td>
      <td>小浣熊</td>
      <td>159.0</td>
      <td>遇安</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Pandas数据分析</td>
      <td>扫地僧</td>
      <td>181</td>
      <td>阿煜</td>
      <td>31</td>
      <td>Bartender</td>
      <td>70.0</td>
      <td>林木木</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pandas数据分析</td>
      <td>翻滚吧，熊猫</td>
      <td>96</td>
      <td>夏洛克</td>
      <td>168</td>
      <td>嘿</td>
      <td>98.0</td>
      <td>Gocara</td>
      <td>83.0</td>
      <td>鉹</td>
      <td>...</td>
      <td>93.0</td>
      <td>Lance</td>
      <td>34.0</td>
      <td>☁️</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Pandas数据分析</td>
      <td>Kung Fu Pandas</td>
      <td>180</td>
      <td>星</td>
      <td>167</td>
      <td>swordsman</td>
      <td>32.0</td>
      <td>陈易男</td>
      <td>161.0</td>
      <td>zzx</td>
      <td>...</td>
      <td>111.0</td>
      <td>#Paletteboo</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Pandas数据分析</td>
      <td>不急不躁我最棒✌️</td>
      <td>2</td>
      <td>Lyndsey</td>
      <td>90</td>
      <td>Roman.</td>
      <td>91.0</td>
      <td>李松泽 Orwell</td>
      <td>115.0</td>
      <td>L.</td>
      <td>...</td>
      <td>62.0</td>
      <td>佬仔</td>
      <td>30.0</td>
      <td>JWJ</td>
      <td>154.0</td>
      <td>👀</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Pandas数据分析</td>
      <td>没想好叫什么队</td>
      <td>4</td>
      <td>王瑞楠</td>
      <td>64</td>
      <td>17</td>
      <td>73.0</td>
      <td>叶</td>
      <td>46.0</td>
      <td>山茶</td>
      <td>...</td>
      <td>24.0</td>
      <td>清风</td>
      <td>153.0</td>
      <td>小闹</td>
      <td>78.0</td>
      <td>罐罐儿</td>
      <td>117.0</td>
      <td>xxxxxxl</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Pandas数据分析</td>
      <td>鲲鲲玩Python</td>
      <td>26</td>
      <td>木南居士</td>
      <td>187</td>
      <td>冻草莓</td>
      <td>61.0</td>
      <td>Shawn</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Pandas数据分析</td>
      <td>pandas gogogo</td>
      <td>27</td>
      <td>daydayup</td>
      <td>183</td>
      <td>郝翊 Eva</td>
      <td>182.0</td>
      <td>辣白菜</td>
      <td>138.0</td>
      <td>时代的病人</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Pandas数据分析</td>
      <td>joyful</td>
      <td>9</td>
      <td>AI</td>
      <td>106</td>
      <td>moon</td>
      <td>87.0</td>
      <td>快乐的貔貅</td>
      <td>12.0</td>
      <td>双手</td>
      <td>...</td>
      <td>10.0</td>
      <td>天国之影</td>
      <td>170.0</td>
      <td>WinqiHe</td>
      <td>17.0</td>
      <td>王婷-TeneT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Pandas数据分析</td>
      <td>pandas从入门到精通</td>
      <td>3</td>
      <td>苏limin</td>
      <td>74</td>
      <td>carlos</td>
      <td>56.0</td>
      <td>maybe</td>
      <td>156.0</td>
      <td>glow</td>
      <td>...</td>
      <td>136.0</td>
      <td>lun</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Pandas数据分析</td>
      <td>Attention！keep干饭</td>
      <td>21</td>
      <td>阿芒Aris</td>
      <td>152</td>
      <td>Alex</td>
      <td>95.0</td>
      <td>Jie</td>
      <td>104.0</td>
      <td>梦想家</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Pandas数据分析</td>
      <td>Null</td>
      <td>103</td>
      <td>Co</td>
      <td>137</td>
      <td>Z.</td>
      <td>69.0</td>
      <td>Albert</td>
      <td>76.0</td>
      <td>北方</td>
      <td>...</td>
      <td>85.0</td>
      <td>慕明智</td>
      <td>133.0</td>
      <td>小菜鸡</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Pandas数据分析</td>
      <td>七星联盟</td>
      <td>14</td>
      <td>卡鲁鼙欧！</td>
      <td>140</td>
      <td>Radio</td>
      <td>105.0</td>
      <td>减肥的卡比兽</td>
      <td>118.0</td>
      <td>好奇宝宝</td>
      <td>...</td>
      <td>112.0</td>
      <td>rain</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Pandas数据分析</td>
      <td>应如是</td>
      <td>54</td>
      <td>思无邪</td>
      <td>58</td>
      <td>Justzer0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>21 rows × 24 columns</p>
</div>




```python
temp = df.iloc[:,1::2].set_index('队伍名称').T.reset_index(drop=True)
temp['是否队长'] = np.r_[[1], np.zeros(temp.shape[0]-1)].astype('int')
melted = temp.melt(id_vars = '是否队长', value_vars = temp.columns[:-1], var_name = '队伍名称', value_name = '昵称').dropna().reset_index(drop=True)
number = pd.concat([df.iloc[:, 2*(i+1): 2*(i+2)].T.reset_index(drop=True).T for i in range(11)]).rename({0:'编号', 1:'昵称'}, axis=1).dropna().reset_index(drop=True)
df1 = melted.merge(number, how='left', on='昵称')
df1
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
      <th>是否队长</th>
      <th>队伍名称</th>
      <th>昵称</th>
      <th>编号</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>你说的都对队</td>
      <td>山枫叶纷飞</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>你说的都对队</td>
      <td>蔡</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>你说的都对队</td>
      <td>安慕希</td>
      <td>7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>你说的都对队</td>
      <td>信仰</td>
      <td>8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>你说的都对队</td>
      <td>biubiu🙈🙈</td>
      <td>20</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>141</th>
      <td>0</td>
      <td>七星联盟</td>
      <td>Daisy</td>
      <td>63</td>
    </tr>
    <tr>
      <th>142</th>
      <td>0</td>
      <td>七星联盟</td>
      <td>One Better</td>
      <td>131</td>
    </tr>
    <tr>
      <th>143</th>
      <td>0</td>
      <td>七星联盟</td>
      <td>rain</td>
      <td>112</td>
    </tr>
    <tr>
      <th>144</th>
      <td>1</td>
      <td>应如是</td>
      <td>思无邪</td>
      <td>54</td>
    </tr>
    <tr>
      <th>145</th>
      <td>0</td>
      <td>应如是</td>
      <td>Justzer0</td>
      <td>58</td>
    </tr>
  </tbody>
</table>
<p>146 rows × 4 columns</p>
</div>



## 【任务三】美国大选投票情况


```python
df1 = pd.read_csv('president_county_candidate.csv')
df2 = pd.read_csv('county_population.csv')
df1
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
      <th>state</th>
      <th>county</th>
      <th>candidate</th>
      <th>party</th>
      <th>total_votes</th>
      <th>won</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Delaware</td>
      <td>Kent County</td>
      <td>Joe Biden</td>
      <td>DEM</td>
      <td>44552</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Delaware</td>
      <td>Kent County</td>
      <td>Donald Trump</td>
      <td>REP</td>
      <td>41009</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Delaware</td>
      <td>Kent County</td>
      <td>Jo Jorgensen</td>
      <td>LIB</td>
      <td>1044</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Delaware</td>
      <td>Kent County</td>
      <td>Howie Hawkins</td>
      <td>GRN</td>
      <td>420</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Delaware</td>
      <td>New Castle County</td>
      <td>Joe Biden</td>
      <td>DEM</td>
      <td>195034</td>
      <td>True</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>32172</th>
      <td>Arizona</td>
      <td>Maricopa County</td>
      <td>Write-ins</td>
      <td>WRI</td>
      <td>1331</td>
      <td>False</td>
    </tr>
    <tr>
      <th>32173</th>
      <td>Arizona</td>
      <td>Mohave County</td>
      <td>Donald Trump</td>
      <td>REP</td>
      <td>78535</td>
      <td>True</td>
    </tr>
    <tr>
      <th>32174</th>
      <td>Arizona</td>
      <td>Mohave County</td>
      <td>Joe Biden</td>
      <td>DEM</td>
      <td>24831</td>
      <td>False</td>
    </tr>
    <tr>
      <th>32175</th>
      <td>Arizona</td>
      <td>Mohave County</td>
      <td>Jo Jorgensen</td>
      <td>LIB</td>
      <td>1302</td>
      <td>False</td>
    </tr>
    <tr>
      <th>32176</th>
      <td>Arizona</td>
      <td>Mohave County</td>
      <td>Write-ins</td>
      <td>WRI</td>
      <td>37</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>32177 rows × 6 columns</p>
</div>




```python
df2
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
      <th>US County</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>.Autauga County, Alabama</td>
      <td>55869</td>
    </tr>
    <tr>
      <th>1</th>
      <td>.Baldwin County, Alabama</td>
      <td>223234</td>
    </tr>
    <tr>
      <th>2</th>
      <td>.Barbour County, Alabama</td>
      <td>24686</td>
    </tr>
    <tr>
      <th>3</th>
      <td>.Bibb County, Alabama</td>
      <td>22394</td>
    </tr>
    <tr>
      <th>4</th>
      <td>.Blount County, Alabama</td>
      <td>57826</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3137</th>
      <td>.Sweetwater County, Wyoming</td>
      <td>42343</td>
    </tr>
    <tr>
      <th>3138</th>
      <td>.Teton County, Wyoming</td>
      <td>23464</td>
    </tr>
    <tr>
      <th>3139</th>
      <td>.Uinta County, Wyoming</td>
      <td>20226</td>
    </tr>
    <tr>
      <th>3140</th>
      <td>.Washakie County, Wyoming</td>
      <td>7805</td>
    </tr>
    <tr>
      <th>3141</th>
      <td>.Weston County, Wyoming</td>
      <td>6927</td>
    </tr>
  </tbody>
</table>
<p>3142 rows × 2 columns</p>
</div>



### question1


```python
temp = df2['US County'].copy()
df2['state'] = temp.apply(lambda x:x.split(', ')[1])
df2['county'] = temp.apply(lambda x:x.split(', ')[0][1:])
df2 = df2.drop(['US County'],axis=1)
df1 = df1.merge(df2, on=['state','county'],how='left')
df1['pop_rate'] = df1['total_votes']/df1['Population']
rat = df1.groupby(['state','county'])['pop_rate'].agg(lambda x:x.sum())
(rat>0.5).sum()
```




    1434



### question2


```python
df3 = df1.pivot_table(index='state',columns='candidate',values='total_votes',aggfunc='sum').reindex(df1.groupby('candidate')['total_votes'].sum().sort_values(ascending=False).index,axis=1)
df3.head(10)
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
      <th>candidate</th>
      <th>Joe Biden</th>
      <th>Donald Trump</th>
      <th>Jo Jorgensen</th>
      <th>Howie Hawkins</th>
      <th>Write-ins</th>
      <th>Rocky De La Fuente</th>
      <th>Gloria La Riva</th>
      <th>Kanye West</th>
      <th>Don Blankenship</th>
      <th>Brock Pierce</th>
      <th>...</th>
      <th>Tom Hoefling</th>
      <th>Ricki Sue King</th>
      <th>Princess Jacob-Fambro</th>
      <th>Blake Huber</th>
      <th>Richard Duncan</th>
      <th>Joseph Kishore</th>
      <th>Jordan Scott</th>
      <th>Gary Swing</th>
      <th>Keith McCormic</th>
      <th>Zachary Scalf</th>
    </tr>
    <tr>
      <th>state</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>Alabama</th>
      <td>849648.0</td>
      <td>1441168.0</td>
      <td>25176.0</td>
      <td>NaN</td>
      <td>7312.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Alaska</th>
      <td>153405.0</td>
      <td>189892.0</td>
      <td>8896.0</td>
      <td>NaN</td>
      <td>34210.0</td>
      <td>318.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1127.0</td>
      <td>825.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Arizona</th>
      <td>1672143.0</td>
      <td>1661686.0</td>
      <td>51465.0</td>
      <td>NaN</td>
      <td>2032.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Arkansas</th>
      <td>423932.0</td>
      <td>760647.0</td>
      <td>13133.0</td>
      <td>2980.0</td>
      <td>NaN</td>
      <td>1321.0</td>
      <td>1336.0</td>
      <td>4099.0</td>
      <td>2108.0</td>
      <td>2141.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>California</th>
      <td>11109764.0</td>
      <td>6005961.0</td>
      <td>187885.0</td>
      <td>81025.0</td>
      <td>80.0</td>
      <td>60155.0</td>
      <td>51036.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Colorado</th>
      <td>1804352.0</td>
      <td>1364607.0</td>
      <td>52460.0</td>
      <td>8986.0</td>
      <td>0.0</td>
      <td>636.0</td>
      <td>1035.0</td>
      <td>8089.0</td>
      <td>5061.0</td>
      <td>572.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>495.0</td>
      <td>355.0</td>
      <td>NaN</td>
      <td>196.0</td>
      <td>175.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Connecticut</th>
      <td>1080680.0</td>
      <td>715291.0</td>
      <td>20227.0</td>
      <td>7538.0</td>
      <td>544.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Delaware</th>
      <td>296268.0</td>
      <td>200603.0</td>
      <td>5000.0</td>
      <td>2139.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>District of Columbia</th>
      <td>317323.0</td>
      <td>18586.0</td>
      <td>2036.0</td>
      <td>1726.0</td>
      <td>3137.0</td>
      <td>NaN</td>
      <td>855.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>693.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>5297045.0</td>
      <td>5668731.0</td>
      <td>70324.0</td>
      <td>14721.0</td>
      <td>1055.0</td>
      <td>5966.0</td>
      <td>5712.0</td>
      <td>NaN</td>
      <td>3902.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 38 columns</p>
</div>



### question3


```python
def select(x):
    def inner_select(inner_x):
        Total = inner_x.total_votes.sum()
        Biden = inner_x.query('candidate=="Joe Biden"').total_votes.sum()
        Trump = inner_x.query('candidate=="Donald Trump"').total_votes.sum()
        return (Biden-Trump)/Total
    bt = x.groupby('county')[['candidate','total_votes']].apply(inner_select)
    return bt.median() > 0
df1.groupby('state').filter(select).state.unique()
```

    <ipython-input-43-ede84539ff06>:6: RuntimeWarning: invalid value encountered in longlong_scalars
      return (Biden-Trump)/Total
    




    array(['Delaware', 'District of Columbia', 'Hawaii', 'Massachusetts',
           'New Jersey', 'Rhode Island', 'Vermont', 'California',
           'Connecticut'], dtype=object)



## 【任务四】显卡日志


```python
df = pd.read_table('benchmark.txt')
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
      <th>start</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>benchmark start :  2020/12/24 12:12:48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Number of GPUs on current device : 1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CUDA Version : 11.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cudnn Version : 8005</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Device Name : GeForce RTX 3090</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>390</th>
      <td>shufflenet_v2_x1_5  model average inference ti...</td>
    </tr>
    <tr>
      <th>391</th>
      <td>Benchmarking Inference double precision type s...</td>
    </tr>
    <tr>
      <th>392</th>
      <td>shufflenet_v2_x2_0  model average inference ti...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>benchmark end :  2020/12/24 12:56:47</td>
    </tr>
    <tr>
      <th>394</th>
      <td>end</td>
    </tr>
  </tbody>
</table>
<p>395 rows × 1 columns</p>
</div>


