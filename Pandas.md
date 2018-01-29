[1. Pandas学习]()

### Pandas学习

#### Series
- Series 是一个值的**序列**，它只有一个列，以及索引。

#### DataFrame
- DataFrame 是表格型数据结构，包含一组有序的列，每个列拥有一个label/name。
- DataFrame 有行索引和列索引
- 生成
```
df = pd.DataFrame(...)
```
- 简单操作
```
df.dtypes # 数据类型
df.index  # 索引
df.colums # 列
df.values # 值
df.describe() # 数据总结
df.T # 转置
df.sort_index(axis=1,ascending=False) # 根据index排序
df.sort_values(by='B') # 根据列数据值排序
```
- 取值 select
```
df['A'],df.A #取A列
df[0:3] #取多行 df[3:3]是个空对象
df.loc['0'] #取第一行数据
df.loc[:,['A','B']] 取所有行的A,B两列数据
df.iloc[3,1] 根据矩阵坐标取数 第四行第二列 （行列都是从0开始计数）
df.iloc[3:5,1:3] 取第4，5行 第2，3列的数据
df.iloc[[1,3,5],1:3] 取第2,4,6行，第2，3列的数据
df.ix[:3,['A','C']] 混合取
df[df.A>8] 判断取数
```
- 设置值 insert

- 合并 concat
```
df1 = pd.DataFrame(...)
df2 = pd.DataFrame(...)
df3 = pd.DataFrame(...)
s1 = pd.Series([1,2,3,4],index = ['a','b','c','d'])
1.纵向合并·列一致
res = pd.concat([df1,df2,df3],axis=0, ignore_index=True) #1.axis合并方向 0默认是纵向合并 类似union, 2.ignore_index忽略源来的index，重新自动生成
2.纵向合并·列不一致
res = pd.concat([df1,df2],axis=0,join="outer") #outer 相同的列合并，其他独自成列，用NaN填充空值
res = pd.concat([df1,df2],axis=0,join="inner") #inner 相同的列合并，其他抛弃
3.横向合并·行一致
res = pd.concat([df1,df2],axis=1,join_axes=[df1.index]) #left join,df1为主表
res = pd.concat([df1,df2],axis=1) #full join
4.append只有纵向合并
res = df1.append([df2,df3],ignore_index=True)
res = df1.append(s1,ignore_index=True)
```
- 合并 merge
    1. merge主要用于两组有key column的数据，统一索引的数据
    ```
    left = pd.DataFrame({'key':['K0','K1','k2','k3'],
                                'A':['A0','A1','A2','A3'],
                                'B':['B0','B1','B2','B3']})
    right = pd.DataFrame({'key':['K0','K1','k2','k3'],
                                'C':['C0','C1','C2','C3'],
                                'D':['D0','D1','D2','D3']})
    res = pd.merge(left,rignt,on='key')
    ```
    2. merge有四种方式 how = ['left','right','outer','inner'],预设值 how = 'inner'
    3. indicator = True 会记录合并的数据来自['left_only','both','right_only']
    4. 依据index合并
    ```
    res = pd.merge(left,right,left_index=True,right_index=True,how='outer')
    res = pd.merge(left,right,left_index=True,right_index=True,how='inner')
    ```
    5. overlapping
    ```
    res = pd.merge(left,right,on='key',suffixes=['_left','_riht'],how='inner')
    ```
- 导入导出 lod
```
df.read_csv($filepath)
df.to_csv($filepath)
```
    