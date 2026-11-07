## Methods of testing whether the data follows normal distribution
- **Shapiro-Wilk** compares the ordered sample with the expected order statistics of a normal distribution.
- **D'Agostino-Pearson** combines standardized skewness and kurtosis.
- **Jarque-Bera**: $JB = \frac{n}{6}\left(S^2 + \frac{(K-3)^2}{4}\right)$, where $S$ is skewness and $K$ is kurtosis.
Notes:
- **Skewness**: $S = \frac{1}{n}\sum_{i=1}^{n}\left(\frac{x_i - \bar{x}}{s}\right)^3$
- **Kurtosis**: $K = \frac{1}{n}\sum_{i=1}^{n}\left(\frac{x_i - \bar{x}}{s}\right)^4$ 
- Why to the power of 4? Actually you can also to the power of 6 which gives hyperkurtosis. But with higher power, the estimate becomes very noisy so it is seldomly used.
The return of stocks is more fat-tail distributed and have more extreme days.

## Three ways of calculating beta
1. OLS with `statsmodels` (keep this fitted result as `res`; it is used again later)
2. $\hat\beta = \dfrac{\widehat{\mathrm{Cov}}(m, r)}{\widehat{\mathrm{Var}}(m)}$
3. $\begin{bmatrix}\hat\alpha \\ \hat\beta\end{bmatrix} = (X^\top X)^{-1} X^\top y$, where $X$ has a column of ones and a column of market returns
>[!faq]- Why 3 has this formula? 
>Let $\hat \theta =\begin{bmatrix}\hat\alpha \\ \hat\beta\end{bmatrix}$ and if the y can be explained by simple linear regression, We are just solving $X\hat\theta=y$. However, the error term exists and let $e=y-X\hat\theta$. To minimize the sum of squared prediction errors, we want to minimize $|e|$. Visualize this in the dimensional space, there is a plane which represents the column space of $X$ and $y$ being a vector that is not on the plane. Our prediction $X\hat\theta$ is vector on the plane, and the error term is $e$. $|e|$ is minimize when it is perpendicular to the plane and prediction is just $y$'s projection on the plane. So we have $X^T e=0$. Expand this we will have that formula.

## Some statistics of simple linear regression model
### $R^2$
<u>How much of the variation in Y does the line explain?</u>
$$
R^2=1-\frac{\sum_{i=1}^n(Y_i-\hat Y_i)^2}  
{\sum_{i=1}^n(Y_i-\bar Y)^2}
$$
It compares the fitted line with a simple baseline:"always predict the mean".

### Residual standard deviation
<u>How large are the residual, in the units of Y?</u>
$$
s^2=\frac{\sum_{i=1}^n e_i^2}{n-2}
$$
$n-2$ because we have two variables $\hat\alpha$ and $\hat\beta$, so the degree of freedom decrease by 2.
As shown by the formula, $s$ is affected by the scale of Y.

### Standard error $SE(\hat\beta)$ and $SE(\hat\alpha)$
<u>How precisely have we estimated the slope?</u>
$$
\operatorname{SE}(\hat\beta)  
=\frac{s}{\sqrt{\sum_{i=1}^n(x_i-\bar x)^2}}
$$
Imagine given true beta, sampling new data of Y with error term varies, we will get slightly different estimated beta. $SE(\hat\beta)$ estimates the standard deviation of these beta.
This is only an estimation since we use $s$ to approximate the variance of $e$ which we don't know.

<u>How precisely have we estimated the intercept?</u>
$$
\operatorname{SE}(\hat\alpha) =s\sqrt{\frac1n+\frac{\bar x^2}{S_{xx}}}
$$
If we repeatedly collected data at the same \(x\)-values and fitted a line, the estimated intercept would change. This standard error estimates the standard deviation of those intercept estimates.

**heteroskedasticity-robust** (HC1) standard errors is another way of calculating standard error without assuming the variance of error is the same.


### t-statistic of $\hat\beta$ and $\hat\alpha$
$$
\boxed{t=\frac{\text{estimate}-\text{hypothesized value}} {\text{standard error of the estimate}}}
$$
$$
p=P\!\left(|T_{n-2}|\ge |t_{\text{observed}}|\right)
$$
## Multi linear regression model
$$
Y=\beta_0+\beta_1X_1+\cdots+\beta_pX_p+\varepsilon
$$
### Variance Inflation Factor (VIF)
Suppose we want to estimate the effect of $X_1$ with other predictors fixed. If $X_1$ is almost predictable from other predictors, $X_1$ contribute little information in predicting $Y$.
We can find the $R_1^2$ by regress $X_1$ with other predictors, then 
$$
\operatorname{VIF}_1=\frac{1}{1-R_1^2}
$$
We can of course do this for other predictors.
Furthermore, for standard error of each predictor $X_j$, it becomes
$$
 \operatorname{SE}(\hat\beta_j) =\frac{s}{\sqrt{\sum_i(x_{ij}-\bar x_j)^2}} \sqrt{\operatorname{VIF}_j}
$$
The reason is that only the variation in $X_j$ **left after accounting for the other predictors** helps identify its separate coefficient.
