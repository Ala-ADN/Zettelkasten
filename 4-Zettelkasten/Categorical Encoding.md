### Identify categorical columns
```python
categorical_cols = df.select_dtypes(include=['object']).columns
categorical_cols
```
### Label encoding
assigning categories ordinals from 1 to n
used when categories have inherent ordering (*low*, *medium*, *high*)
```python
cat_features = []
for col in cat_features:
    df[col] = LabelEncoder().fit_transform(df[col])
```
### One-hot encoding
creates a new binary column for each category
used when there is no relation between categories (*apples*, *oranges*, *bananas*)
#### Pandas Dummies
only for simple analysis
```python
df = pd.get_dummies(df,columns=cat_features, drop_first=True)
```
### Sklearn OneHotEncoder
provides a persistent encoder that handles training and testing data consistently
if a category in present only in the testing data, a new column will not be created
```python
for col in cat_features:
    reshaped = df[col].values.reshape(-1, 1)
    encoded_feat = OneHotEncoder().fit_transform(reshaped).toarray()
    cols = ['{}_{}'.format(col, n) for n in range(1, df[col].nunique() + 1)]
    encoded_df = pd.DataFrame(encoded_feat, columns=cols)
    df = df.drop(col, axis=1) 
    df = pd.concat([df, encoded_df], axis=1)
```