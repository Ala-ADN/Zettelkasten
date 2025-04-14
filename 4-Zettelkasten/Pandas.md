
1. Loading a pandas DataFrame
```python
df = pd.read_csv('/content/train.csv')
```
2. Shape of dataframe
```python
df.shape
```
3. View first rows
```python
df.head()
```
4. counting the number of missing values in the df
```python
df.isnull().sum()
```
5. fill null values
```python
df = df.fillna(<filler>)
```
6. check column types
```python
df.dtypes
```
7. unique values of each column
```python
df.unique()
```
8. info
```python
df.info()
```
9. describe (count, min, max, mean)
```python
df.describe()
```
10. rows with condition
```python
df[(df["col"]>=5)&(df["col"]!=7)]
```
11. subset (inclusive)
```python
df.loc[t:b]
```
[kaggle cheatsheet](https://www.kaggle.com/code/grroverpr/pandas-cheatsheet)

![[Pandas_Cheat_Sheet.pdf]]

