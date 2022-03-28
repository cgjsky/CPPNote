探索性数据分析（Exploratory Data Analysis，简称EDA），摘抄网上的一个中文解释，是指对已有的数据（特别是调查或观察得来的原始数据）在尽量少的先验假定下进行探索，通过作图、制表、方程拟合、计算特征量等手段探索数据的结构和规律的一种数据分析方法。

## 导包

```python
import numpy as np
import pandas as pd
import matplotlib as plt
import seaborn as sns 
%matplotlib line
import warnings
warnings.filterwarnings('igore')
```

## 导入数据

```python
data=pd.read_cav(" ")
```

## 变量探索

```
data.info()
data.describe()
data.head()
data.tail()
...
```

## 画图

不同的图对应不同的目的：

**Trends**：折线图，看y随着x的发展趋势

**Relationship** ：

条形图：对比不同组别的数量

回归散点图：在散点图中包括一条回归线，使我们更容易看到两个变量之间的任何线性关系。

**Distribution** 

```python
plt.figure(figsize=(16,6))
#折线图
sns.lineplot(x=...,y=...,label="...")
plt.title("...")
plt.xlabel=...
plt.ylabel=...
#条形图
sns.barplot(x=...,y=...)
#热力图
#annot是否显示每个cell的数字
sns.heatmap(data=...,annot=True)
#散点图
sns.scatterplot(x=...,y=...)
#增加回归线的散点图
sns.regplot(x=...,y=...)
#增加颜色编码的散点图，研究三个变量的关系,一般hue是一个yes/no的二值变量
sns.scatterplot(x=..., y=..., hue=...)
#增加回归线的彩色散点图,注意此时x，y，hue不再是具体值而是属性
sns.lmplot(x="..",y="..",hue="..",data=...)
#分布图
sns.distplot(a=data,kde=False)
plt.legend()
```

