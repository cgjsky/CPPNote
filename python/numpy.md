# 基础运算

numpy的加减乘除都是批量运算

```python
加减乘除
+-*/
np.dot()
数据统计分析
np.max() np.min() np.sum() np.prod() np.count()
np.std() np.mean() np.median()
特殊运算符号
np.argmax() np.argmin()
np.ceil() np.floor() np.clip()
```



```python
#list
l=[1,2,3,4,5]
for i in range(len(l)):
  l[i]+=3
print(l)
#numpy +-*/
l=np.array([1,2,3,4,5])
print(l+3)
#np.dot()
a=np.array([[1,2],[3,4]])
b=np.array([[5,6],[7,8]])
print(np.dot(a,b))
```

# 数据分析函数

```python
#常用函数
a = np.array([150, 166, 183, 170])
print("最大：", np.max(a))
print("最小：", np.min(a))
print(np.sum(a))
print("累乘：", a.prod())
print("总数：", a.size)   
print("非零总数：", np.count_nonzero(a))
print("平均工资：", np.mean(a))
print("工资中位数：", np.median(a))
print("标准差：", np.std(a))

#argmin argmax
name = ["小米", "OPPO", "Huawei", "诺基亚"]
high_idx = np.argmax(a)
low_idx = np.argmin(a)
print("{} 最高".format(name[high_idx]))
print("{} 最矮".format(name[low_idx]))

#向上保留小数还是向下保留小数
a = np.array([150.1, 166.4, 183.7, 170.8])
print("ceil:", np.ceil(a))
print("floor:", np.floor(a))
#自定义取值截取空间
a = np.array([150.1, 166.4, 183.7, 170.8])
print("clip:", a.clip(160, 180))
##clip: [160.  166.4 180.  170.8]
```

# 改变数据形态

```python
改变形态
array[np.newaxis, :]#增加维度
array.reshape() #用的更多
array.ravel(), array.flatten()
array.transpose()
合并数组
np.column_stack(), np.row_stack()
np.vstack(), np.hstack(), np.stack()
np.concatenate()
拆解
np.vsplit(), np.hsplit(), np.split()
```

```python
#按列合并
feature_a = np.array([1,2,3,4,5,6])
feature_b = np.array([11,22,33,44,55,66])
c_stack = np.column_stack([feature_a, feature_b])

#按行合并
sample_a = np.array([0, 1.1])
sample_b = np.array([1, 2.2])
c_stack = np.row_stack([sample_a, sample_b])

#np.concatenate()用axis来控制按列合并还是按行合并
a = np.array([[1,2],[3,4]])
b = np.array([[5,6],[7,8]])
print(np.concatenate([a, b], axis=0))
print(np.concatenate([a, b], axis=1))

#拆解
#np.split()与concatenate()一样，用axis控制纵横
a = np.array([[ 1, 11, 2, 22],[ 3, 33, 4, 44],[ 5, 55, 6, 66],[ 7, 77, 8, 88]])
 # 分成两段 行分开
print(np.split(a, indices_or_sections=2, axis=0)) 
# 0~2 一段，2~3 一段，3~一段 列分开
print(np.split(a, indices_or_sections=[2,3], axis=1)) 
```

