# Pandas处理时间序列数据

## Pandas序列(Series)
Series可以理解为一维数组，它和一维数组的区别，在于Series具有索引。Series用来处理一维数据非常有用，比如时间序列数据： 设备数据，日志数据， 股价数据。

## Series基本操作
### 创建Series
  [官方文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html):  
  Series(data=None, index=None, dtype=None, name=None, copy=False, fastpath=False)[source]
  One-dimensional ndarray with axis labels (including time series).

  Labels need not be unique but must be a hashable type. The object supports both integer- and label-based indexing and provides a host of methods for performing operations involving the index. Statistical methods from ndarray have been overridden to automatically exclude missing data (currently represented as NaN).

  Operations between Series (+, -, /, , *) align values based on their associated index values– they need not be the same length. The result index will be the sorted union of the two indexes.

  Parameters:	
  data : array-like, Iterable, dict, or scalar value
  Contains data stored in Series.

  Changed in version 0.23.0: If data is a dict, argument order is maintained for Python 3.6 and later.

  index : array-like or Index (1d)
  Values must be hashable and have the same length as data. Non-unique index values are allowed. Will default to RangeIndex (0, 1, 2, …, n) if not provided. If both a dict and index sequence are used, the index will override the keys found in the dict.

  dtype : str, numpy.dtype, or ExtensionDtype, optional
  Data type for the output Series. If not specified, this will be inferred from data. See the user guide for more usages.

  copy : bool, default False
  Copy input data.

  举例:
  ```py
  money_series = pd.Series([200, 300, 10, 5], name="money") # 未设置索引的情况下，自动从0开始生成，也可以设置键值对参数index
  """
  0    200
  1    300
  2     10
  3      5
  Name: money, dtype: int64
  """
  ```
### 获取序列值
  根据索引获取序列值
  ```py
  money_series[0]
  # 200
  ```
### 排序序列
  ```py
  money_serires.sort_values()
  """
  3      5
  2     10
  0    200
  1    300
  Name: money, dtype: int64
  """
  money_series[0] # 根据索引获取具体的值，0对应的依旧是200
  # 200
  money_series.iloc[0] # 根据排序后的序号获取具体的值
  # 5
  ```
### 切片
  根据索引切片
  ```py
  money_series = pd.Series({'d': 200, 'c': 300, 'b': 10, 'a': 5}, name='money')
  """
  a    200
  b    300
  c     10
  d      5
  Name: money, dtype: int64
  """
  money_series.loc['a'] # 等价于 money_series['a']
  # 200
  money_series.loc['c':'a':-1] # 从c取到 a，倒序
  """
  c     10
  b    300
  a    200
  Name: money, dtype: int64
  """
  money_series.loc[['d', 'a']] # d, a的值，等价于 money_series[['d', 'a']]
  """
  d      5
  a    200
  """
  ```
  根据序号切片
  ```py
  money_series.iloc[0]
  # 200
  money_series.iloc[1:3] # 根据序号取值，不包含结束，等价于 money_series[1:3]
  """
  b    300
  c     10
  Name: money, dtype: int64
  """
  money_series.iloc[[3, 0]] # 取第三个值和第一个值
  """
  d      5
  a    200
  Name: money, dtype: int64
  """
  ```
  根据条件切片
  ```py
  money_series[money_series > 50] # 选取大于50的值
  """
  c    300
  d    200
  Name: money, dtype: int64
  """
  money_series[lambda x: x ** 2 > 50] # 选取值平方大于50的值
  """
  b     10
  c    300
  d    200
  Name: money, dtype: int64
  """
  ```
### 由Series创建dataframe
  ```py
  price_series = pd.Series([100, 200, 30], index=['T001', 'T002', 'T005'])
  quantity_series = pd.Series([3, 3, 10, 2], index=['T001', 'T002', 'T003', 'T004'])
  df = pd.DataFrame({'单价': price_series, '数量': quantity_series})
  # 数据中不含有对应元素，则置为NaN
  """
           单价    数量
  T001  100.0   3.0
  T002  200.0   3.0
  T003    NaN  10.0
  T004    NaN   2.0
  T005   30.0   NaN
  """
  ```
### 获取序列索引集合
  ```py
  price_series.index
  # ['T001', T002', 'T005']
  ```
### 获取序列值集合
  ```py
  price_series.values
  # [100, 200, 300]
  ```
