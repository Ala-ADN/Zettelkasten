## Use cases
- simplify non linear relationships
- reduce the impact of outliers
- improve interpretability
- categorical features better suite decision trees
- get more recognizable patterns form time-series (hours into parts of the day)
## Equal frequency binning
split continuous column *col* into *n* categories with equal number of rows
```python
df['col'] = pd.qcut(df_all['col'], n)
```
**When to Use**
- Data is **skewed** (e.g., income, sales data)
- You need balanced representation across categories (e.g., customer segmentation)
- Outliers are present, and you want to minimize their effect
- Examples: Credit score groups, survey age brackets
**Limitations**
- Bins may have overlapping ranges if values are repeated
- Less effective if the data distribution is highly irregular
## Equal width binning
split continuous column *col* into *n* categories with equal width
```python
df['col'] = pd.qcut(df_all['col'], n)
```
**When to Use**
- Data is **uniformly distributed** (e.g., temperature readings).
- Preserving the **actual scale** matters (e.g., age groups: 0–20, 20–40)
- Domain knowledge dictates fixed thresholds (e.g., BMI categories)
- Examples: Exam score ranges, fixed time intervals (hours in a day)
**Limitations**
- Vulnerable to outliers (e.g., a single extreme value stretches bins)
- May create empty or sparse bins in skewed distributions
