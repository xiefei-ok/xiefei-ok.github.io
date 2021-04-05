---
title: pandas学习笔记
---


# pandas学习笔记


### 1.批量读取csv文件
```python
import pandas as pd
import numpy as np
import glob,os,re

path=r'D:/data'    #批量表格所在文件路径
file=glob.glob(os.path.join(path, "HIST_DMIND_MERGE_201809**.csv"))   #每一个表格相同名称部分
print(file)
dl= []
for f in file:
dl.append(pd.read_csv(f,index_col=None,encoding='ANSI'))   #读取每个表格
df=pd.concat(dl)   #合并

# 第二种方法
os.chdir(r'Y:..\CA_MCMC_Worldpop_1km\pop_to_grid')  # 所在文件夹
for file in os.listdir():
    df = pd.read_csv(file).rename(columns={'value':re.findall('\d+',file)[0]}).set_index('id')
    final = pd.concat([final,df],axis=1)
```

### 2.表合并

* concat（沿轴合并——就硬拼在一起）

  ```python 
  concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False, keys=None, levels=None, names=None, verify_integrity=False, sort=None, copy=True)
  
  ```

  > 1. objs:需要连接到 对象集合，一般是列表或字典；
  > 2. axis：连接轴向（默认为axis=0）；
  > 3. join：参数为outer或inner；
  > 4. join_axes=[]：指定自定义的索引，axis=1的时候，outer和inner可以理解为索引的并交集；
  > 5. keys=[]：创建层次化索引；
  > 6. ignore_index = True：重建索引；
  > 7. names：列命名。

* merge（通过键拼接列——表连接）

  ```python
  merge(left, right, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False, sort=False, suffixes=('_x', '_y'), copy=True, indicator=False, validate=None)
  
  ```

  > 1. left和right：两个不同的Dataframe，右边有时候也可以为Series；
  > 2. how：{‘left’，'right'，'outer'，'inner'},default为'inner';
  > 3. on：指的是用于连接的列索引名称，必须存在于左右两个dataframe中，如果没有指定且其他的参数也没有指定，则以两个dataframe列名交集作为连接键；
  > 4. left_on:左侧dataframe中用于连接键的列名；
  > 5. right_on：右侧dataframe中用于连接键的列名；
  > 6. left_index：使用左侧dataframe中行索引作为连接键；
  > 7. right_index：使用左侧dataframe中行索引作为连接键；

* join（主要用于索引上的合并——基于索引的表连接）

  ```python
  join(self, other, on=None, how='left', lsuffix='', rsuffix='', sort=False)
  ```

  > join是DataFrame的方法，只能pd.DataFrame.join
