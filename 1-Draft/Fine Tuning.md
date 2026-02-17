> if you have no idea what value a hyperparameter should have, try consecutive powers of 10 
### Grid Search
use cross-validation to evaluate all the possible combinations of hyperparameter values
```python
from sklearn.model_selection import GridSearchCV

param_grid = [
	{'n_estimators': [3, 10, 30],
	 'max_features': [2, 4, 6, 8]
	},
	{'bootstrap': [False],
	 'n_estimators': [3, 10],
	 'max_features': [2, 3, 4]
	},
]

forest_reg = RandomForestRegressor()
grid_search = GridSearchCV(forest_reg, param_grid, cv=5,
						scoring='neg_mean_squared_error',
						return_train_score=True)
grid_search.fit(housing_prepared, housing_labels)

cvres = grid_search.cv_results_
for mean_score, params in zip(cvres["mean_test_score"], cvres["params"]):
	#show evalutaions
	print(np.sqrt(-mean_score), params)
```
### Randomized Search
preferable when the hyperparameter search space is large
