## Regression methods
- **Piecewise linear**: a continuous function made of straight segments that may bend at `n_knots` equally spaced points. It is a linear regression on the basis $1, x, \max(x - k_1, 0), \dots, \max(x - k_m, 0)$.
- **Nadaraya-Watson (NW) kernel regression**: at each point, a weighted average of the nearby $y$ values, with Gaussian weights whose width is the `bandwidth`.
- **Local linear regression**: at each point, a weighted straight-line fit with the same weights, evaluated at that point.
- **Polynomial regression**: use polynomial of degree d to approximate

## Validation methods
Performance is measured by the out-of-sample $R^2$ relative to the training mean:
$$R^2_{\text{oos}} = 1 - \frac{\sum (y - \hat y)^2}{\sum (y - \bar y_{\text{train}})^2}.$$
Shuffled K_fold Split: Future data leakage.
Continuous K_fold Split: In the middle.
Time Series Split: Most close real use.

## Regularization

$$
\text{Ridge:}\ \min_\beta \sum_t (y_t - x_t^\top\beta)^2 + \alpha \sum_j \beta_j^2 \qquad\qquad \text{Lasso:}\ \min_\beta \tfrac{1}{2n}\sum_t (y_t - x_t^\top\beta)^2 + \alpha \sum_j |\beta_j|
$$
OSL: ordinary least square

## Attempts to solve the fat tail problem
OLS works well with the assumption that the error variance is constant. However, as we know, the financial return is usually fat tailed. Heteroskedasticity-robust (HC1) gives us better understanding about the coefficient standard error but the prediction itself still use OLS, which makes it susceptible to extreme days.
Here are some other objective function that try to solve this:
- **Weighted least squares (WLS)** gives less weight to observations with a larger error variance.
- **Huber regression** behaves like least squares for small errors and like absolute errors for large ones, so extreme days pull less.
- **Least absolute deviations (LAD)** minimizes $\sum |y_t - x_t^\top\beta|$; it estimates the conditional **median** instead of the mean.
- **Quantile regression** generalizes LAD to any quantile $q$: it estimates how the $q$-th quantile of $Y$ moves with $X$.

Exact process of using WLS:
Use OLS to estimate and get the error residual. Use absolute market return and a constant to linear regress the error variance. Then use $\frac{1}{\text{error variance}}$ as the weight of WLS.

## Regression for probability
Models
- The **linear probability model** (LPM) simply runs OLS on the 0/1 outcome.
- **Logistic regression** models the log-odds as linear: $\log \frac{p}{1 - p} = a + b\,x$, so that $p = \frac{1}{1 + e^{-(a + b x)}}$ always lies between 0 and 1.
Objective function
- **Log loss**: $-\frac{1}{n}\sum \left[y \log \hat p + (1 - y)\log(1 - \hat p)\right]$; lower is better, and confident wrong forecasts are punished heavily.
- **Brier score**: $\frac{1}{n}\sum (\hat p - y)^2$; lower is better.
- **Calibration**: among the days given a probability of about 70%, about 70% should turn out to be up days.
> [!question] Why logistic regression is preferred over LPM?
> - Range from 0 to 1
> - Sensible shape. From 98% to 99% requires more evidence than from 50% to 51%
> - Coincident with the Bayes' rule in odds forms as independent evidence adds on log odds.

