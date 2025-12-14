```python
# General libraries
import numpy as np
import pandas as pd
import os
import sys
import random
import warnings
warnings.filterwarnings('ignore')

# Visualization libraries
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
sns.set(style="darkgrid")

# Machine Learning libraries
from sklearn.model_selection import train_test_split, GridSearchCV, StratifiedKFold, cross_val_score
from sklearn.preprocessing import StandardScaler, MinMaxScaler, LabelEncoder, OneHotEncoder, OrdinalEncoder
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, confusion_matrix, classification_report
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier, RandomForestRegressor, GradientBoostingRegressor
from sklearn.linear_model import LogisticRegression, LinearRegression
from sklearn.svm import SVC, SVR
from sklearn.pipeline import Pipeline, make_pipeline

# Additional libraries
from scipy.stats import skew, norm
from tqdm import tqdm
import gc

```


```python

# Seed for reproducibility
def seed_everything(seed=42):
    random.seed(seed)
    np.random.seed(seed)
    os.environ['PYTHONHASHSEED'] = str(seed)

seed_everything()

# Data loading
def load_data(file_path):
    return pd.read_csv(file_path)

#Find null values
def null_columns(df):
    c = []
    for a in df.columns:
        n = df[a].isnull().sum()
        if n !=0:
            print(a,n)
            c.append(a)
    return c

# Model evaluation
def evaluate_model(model, X_test, y_test):
    predictions = model.predict(X_test)
    print("Accuracy:", accuracy_score(y_test, predictions))
    print("Precision:", precision_score(y_test, predictions, average='macro'))
    print("Recall:", recall_score(y_test, predictions, average='macro'))
    print("F1 Score:", f1_score(y_test, predictions, average='macro'))
    print("Confusion Matrix:\n", confusion_matrix(y_test, predictions))
    print("Classification Report:\n", classification_report(y_test, predictions))
```

```python
# Deep Learning libraries
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout, Flatten, Conv2D, MaxPooling2D, BatchNormalization
from tensorflow.keras.optimizers import Adam
from tensorflow.keras.callbacks import EarlyStopping, ModelCheckpoint
```
#### Main
```python
# Load data
train = load_data('train.csv')
test = load_data('test.csv')
df = pd.concat([train, test], sort=True).reset_index(drop=True)
dfs = [train, test]
```
