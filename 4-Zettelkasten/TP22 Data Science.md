
## 1. Data Import
```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

# Load CSV with two columns: x, y
data = np.genfromtxt('Exemple2Reference_SReg_PW2.csv', delimiter=',', names=True)
x = data['x']
y = data['y']
n = x.size
````
## 2. Scatter Plot
```python
plt.figure()
plt.scatter(x, y, alpha=0.7)
plt.title('Scatter Plot of Y vs X')
plt.xlabel('X')
plt.ylabel('Y')
plt.show()
```
**Interpretation:**
Points exhibit a positive linear trend: as $X$ increases, $Y$ tends to increase.
## 3. Least‐Squares Estimates
The normal‐equations solution gives
$$

\hat\beta_1 = \frac{\sum_i(x_i-\bar x)(y_i-\bar y)}{\sum_i(x_i-\bar x)^2},\quad

\hat\beta_0 = \bar y - \hat\beta_1,\bar x

$$
```python
x_mean, y_mean = x.mean(), y.mean()
SS_xy = np.sum((x - x_mean)*(y - y_mean))
SS_xx = np.sum((x - x_mean)**2)

beta1 = SS_xy / SS_xx
beta0 = y_mean - beta1 * x_mean
print(f'Intercept = {beta0:.4f}, Slope = {beta1:.4f}')
```
## 4. Regression Line
```python
plt.figure()
plt.scatter(x, y, alpha=0.7)
plt.plot(x, beta0 + beta1*x, 'r-', label='Fit')
plt.legend(); plt.show()
```
**Interpretation:**
The fitted line captures the central tendency of the data, confirming the estimated slope.
## 5. ANOVA Table

##### SST (total): $\sum (y_i - \bar y)^2$
##### SSR (regression): $\sum (\hat y_i - \bar y)^2$
##### SSE (error): $\sum (y_i - \hat y_i)^2$
##### DF₁ = 1, DF₂ = $n-2$
##### MSR = SSR/1, MSE = SSE/(n-2)
##### $F=\mathrm{MSR}/\mathrm{MSE}$

```python
y_hat = beta0 + beta1*x
SST = np.sum((y - y_mean)**2)
SSR = np.sum((y_hat - y_mean)**2)
SSE = np.sum((y - y_hat)**2)

MSR = SSR / 1
MSE = SSE / (n-2)
F_stat = MSR / MSE
p_value = 1 - stats.f.cdf(F_stat, 1, n-2)

print(f'SSR={SSR:.2f}, SSE={SSE:.2f}, SST={SST:.2f}')
print(f'F={F_stat:.2f}, p-value={p_value:.3e}')
```

## 6. Residual Diagnostics

Python

```python
resid = y - y_hat

# Residual vs. Fitted
plt.figure(); plt.scatter(y_hat, resid); plt.axhline(0, ls='--')
plt.title('Residuals vs Fitted'); plt.show()

# Q–Q plot
stats.probplot(resid, dist="norm", plot=plt)
plt.title('Normal Q–Q Plot'); plt.show()
```

**Interpretation:**

No pattern in residuals $\rightarrow$ linearity & homoscedasticity plausible.

Q–Q roughly straight $\rightarrow$ Gaussian residuals plausible.

## 7. 95 % Confidence Intervals

$$

\mathrm{SE}{\beta_1} = \sqrt{\frac{\mathrm{MSE}}{\sum (x_i-\bar x)^2}},\quad

\mathrm{SE}{\beta_0} = \sqrt{\mathrm{MSE}\Bigl(\frac1n + \frac{\bar x^2}{\sum(x_i-\bar x)^2}\Bigr)}

$$

$t_{crit} = t_{0.975,,n-2}$

Python

```python
SE_b1 = np.sqrt(MSE / SS_xx)
SE_b0 = np.sqrt(MSE * (1/n + x_mean**2 / SS_xx))
t_crit = stats.t.ppf(0.975, n-2)

ci_b1 = (beta1 - t_crit*SE_b1, beta1 + t_crit*SE_b1)
ci_b0 = (beta0 - t_crit*SE_b0, beta0 + t_crit*SE_b0)
print(f'95% CI for slope: {ci_b1}')
print(f'95% CI for intercept: {ci_b0}')
```

## 8. Prediction at $X=500$

```python
x0 = 500
y0 = beta0 + beta1*x0
# prediction interval
SE_pred = np.sqrt(MSE*(1 + 1/n + (x0 - x_mean)**2 / SS_xx))
pi = (y0 - t_crit*SE_pred, y0 + t_crit*SE_pred)
print(f'Predicted Y at X=500: {y0:.2f}')
print(f'95% prediction interval: {pi}')
```

## Conclusion

The model shows a significant linear relationship ($F$-statistic & small $p$).

Residual checks support normality and homoscedasticity.

95 % CIs and prediction at $X=500$ are provided.