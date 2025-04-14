A custom transformer in Scikit-Learn is a class that implements `fit()` and `transform()` methods Adding `TransformerMixin` gives access to `fit_transform()`, and inheriting `BaseEstimator` enables hyperparameter tuning with `get_params()` and `set_params()`
This allows seamless integration with Scikit-Learn pipelines
```python
import numpy as np
from sklearn.base import BaseEstimator, TransformerMixin
class CombinedAttributesAdder(BaseEstimator, TransformerMixin):
    def __init__(self, add_bedrooms_per_room=True):
        self.add_bedrooms_per_room = add_bedrooms_per_room
    def fit(self, X, y=None):
        return self  # No fitting needed, just return self
    def transform(self, X):
        # Create new features
        #rooms_ix, bedrooms_ix, population_ix, households_ix = 3, 4, 5, 6
        rooms_per_household = X[:, 3] / X[:, 6] 
        population_per_household = X[:, 5] / X[:, 6]
        # Optionally add bedrooms per room
        if self.add_bedrooms_per_room:
            bedrooms_per_room = X[:, 4] / X[:, 3] 
            return np.c_[X, rooms_per_household, population_per_household, bedrooms_per_room]
        else:
            return np.c_[X, rooms_per_household, population_per_household]

# Example usage
attr_adder = CombinedAttributesAdder()
housing_extra_attribs = attr_adder.transform(housing.values)
```