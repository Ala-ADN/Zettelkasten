Machine Learning algorithms don’t perform well when the input numerical attributes have very different scales
! scaling the target values is generally not required
!! splitting should be done before so data from the test set doesn't contaminate the training set
### Normalization | Min-Max scaling
subtract the min and divide by max - min
result ranges from \[0,1\]
! min or max could be outliers (other values will be crushed)
```python
scaler = MinMaxScaler()
#assign min and max to the scaler
ds_train_normalized = scaler.fit_transform(ds_train)
ds_test_normalized = scaler.fit_transform(ds_test)
```
### Standardization
subtract the mean value then divide by the standard deviation
result has a zero mean and and unit variance
! result is not bound to a specific range (not suited for neural nets)
```python
scaler = StandardScaler()
#assign mean and std to the scaler
ds_train_standardized = scaler.fit_transform(ds_train)
ds_test_standardized = scaler.fit_transform(ds_test)
```
