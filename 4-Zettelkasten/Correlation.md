### Pivoting Features
```python
df[['col', 'target']].groupby(['col'], as_index=False).mean().sort_values(by='target', ascending=False)
```

### Visualiser Correlation
```python
plt.figure(figsize=(16,12))
sns.heatmap(df.select_dtypes(include = ['number']).corr(),annot=True,fmt=".2f")
```

### Delete highly inter-correlated features
inter-correlated data can dilute the feature pool without adding additional information

### Select features with high correlation with the target data
```python
df_num = df.select_dtypes(include = ['number'])
df_num_corr = df_num.corr()['target'][:-1] # -1 exclude target
golden_features_list = df_num_corr[abs(df_num_corr) > 0.5].sort_values(ascending=False)
```

### Predictive Score
can detect non linear relationships between columns
```bash
!pip install ppscore
```

```python
import ppscore as pps
```

```python
plt.figure(figsize=(16,12))
sns.heatmap(pps.matrix(df),annot=True,fmt=".2f")
```