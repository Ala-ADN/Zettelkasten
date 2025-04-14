The Pipeline constructor takes a list of name/estimator pairs defining a sequence of steps
All but the last estimator must be transformers (i.e., must have a fit_transform() method)
```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

num_pipeline = Pipeline([
		('imputer', SimpleImputer(strategy="median")),
		('attribs_adder', CombinedAttributesAdder()),
		('std_scaler', StandardScaler()),
	])

df_num_tr = num_pipeline.fit_transform(df)
```
### ColumnTransformer
The constructor requires a list of tuples, where each tuple contains a name, a transformer, and a list of names (or indices) of columns that the transformer should be applied to
```python
from sklearn.compose import ColumnTransformer

full_pipeline = ColumnTransformer([
		("num", num_pipeline, num_attribs),
		("cat", cat_pipeline, cat_attribs),
	])

df_prepared = full_pipeline.fit_transform(df)
```
