# Count NULL
```python
#Find null values
def null_columns(df):
    c = []
    for a in df.columns:
        n = df[a].isnull().sum()
        if n !=0:
            print(a, df[a].dtype, n)
            c.append(a)
    return c
```
# Missing data matrix
```python
msno.matrix(df, color=(0.0, 0.2, 0.4))
plt.title('Missing Data Locations in Dataset', fontsize=26)
plt.xlabel('Columns', fontsize=16)
plt.show()
```

# Test for skewed data
```python
df.select_dtypes(include = ['number']).hist(figsize=(20, 20), bins=50, xlabelsize=8, ylabelsize=8)
```
![[651px-Relationship_between_mean_and_median_under_different_skewness 1.png]]

# Fill missing values
### if data is not skewed
```python
df['COLUMN'].fillna(df['COLUMN'].median(),inplace=True)
```
### if data is skewed
```python
df['COLUMN'].fillna(df['COLUMN'].mode(),inplace=True)
```
### if column is intercorrelated with others
*for example: missing age values need to account for other features like sex*
```python
df['col'].fillna(df.groupby(['cols'])['col'].transform('median'),inplace=True)
```

```python
df['col'] = df.groupby(['cols'])['col'].transform(lambda x: x.fillna(pd.Series.mode(x)[0]) if x.notna().any() else x)
```

## Custom Transformer
transformer compatible with sklearn pipelines
```python
class AbstractNullTransformer(BaseEstimator, TransformerMixin):
    def fit(self, X, y=None):
        # Fill nulls with mode for categorical columns
        self.col1_mode_ = X['col1'].mode()[0]
        # Fill nulls with median for numerical columns
        self.col2_median_ = X['col2'].median()
        # Custom aggregation (e.g., mode) for a column grouped by another column
        self.col3_agg_ = X.groupby('col4')['col3'].agg(
            lambda x: x.mode()[0] if not x.isnull().all() else np.nan
        )
        # Group by one column and calculate median for another column
        self.col4_groupby_ = X.groupby('col4')['col2'].median()
        return self
    
    def transform(self, X, y=None):
        df = X.copy()
        # Fill nulls in categorical column with mode
        df['col1'].fillna(self.col1_mode_, inplace=True)
        # Fill nulls in numerical column with median
        df['col2'].fillna(self.col2_median_, inplace=True)
        # Fill nulls using custom aggregation (mode) based on another column
        df['col3'] = df.apply(
            lambda row: self.col3_agg_.get(row['col4'], row['col3'])
            if pd.isnull(row['col3']) else row['col3'], axis=1
        )
        # Fill nulls using groupby median
        df['col2'] = df.apply(
            lambda row: self.col4_groupby_.get(row['col4'], row['col2'])
            if pd.isnull(row['col2']) else row['col2'], axis=1
        )
        return df
```

# Remove columns with too many missing values
```python
def purge_null(df):
		df2 = df[[c for c in df if df[c].count()/len(df)>=0.3]]
		print("Removed:", ",".join([c for c in df if c not in df2.columns]))
		return df2
```

# Remove rows with missing values
```python
df.dropna(subset=[col1,col2], inplace=True)
```

# Missing Header
```python
columns = []
data = pd.read_csv(path, names=columns)
```
