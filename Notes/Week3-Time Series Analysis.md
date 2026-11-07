## Stationary
A series $X_t$ is weakly stationary if 
1. Its mean $\mathrm E(X_t)=\mu$ for every $t$
2. Its variance $\mathrm Var(X_t)=\gamma_0$ is finite and remains constant for every $t$
3. The auto-covariance is constant at every lag $k$. That is $\mathrm Cov(X_t,X_{t-k})=\gamma_k$ for every $t$
## Autocorrelation
Autocorrelation function ACF is
$$
\rho_k=\frac{\gamma_k}{\gamma_0}=\mathrm COR(X_t,X_{t-k})
$$
It is estimated by sample using following equation
$$
\hat\rho_k = \frac{\sum_{t=k+1}^{n} (x_t - \bar x)(x_{t-k} - \bar x)}{\sum_{t=1}^{n} (x_t - \bar x)^2}
$$
**White noise benchmark**:
If the observations are independent and identically distributed, then every $\rho_k=0$. The sample estimate $\hat \rho_k$ follows $\mathcal N(0,1/n)$ for large $n$. The $95\%$ confidence interval is $\pm 1.96/\sqrt{n}$.
**Ljung box test:**
Combining the first m sample autocorrelation into one test, we get
$$
Q(m)=n(n+2)\sum_{k=1}^m \frac{\hat\rho_k}{n-k}
$$
Under the null hypothesis that observations being iid, Q(m) follows $\chi^2$ with $m$ degree of freedom. Small p-value rejects the null hypothesis and suggests at least one of $\rho_1, \dots, \rho_m$ is not zero.
**Partial autocorrelation function:**
The partial autocorrelation at lag $k$, $\alpha_k$, measures the link that remains once the lags in between are held fixed. It is the coefficient on $X_{t-k}$ in the regression
$$X_t = c + a_1 X_{t-1} + \dots + a_{k-1} X_{t-k+1} + \alpha_k X_{t-k} + u_t,$$

## ARMA
**AR(p)** Autoregressive:
$$X_t=c+\phi_1X_{t-1}+...+\phi_pX_{t-p}+\varepsilon_t$$
**MA(q)** Moving average:
$$X_t=\mu+\varepsilon_t+\theta_1\varepsilon_{t-1}+...+\theta_p\varepsilon_{t-p}$$
**ARMA(p, q)** combines both:

$$X_t = c + \phi_1 X_{t-1} + \dots + \phi_p X_{t-p} + \varepsilon_t + \theta_1 \varepsilon_{t-1} + \dots + \theta_q \varepsilon_{t-q}.$$

| | ACF | PACF |
|---|---|---|
| AR(p) | decays gradually | zero beyond lag $p$ |
| MA(q) | zero beyond lag $q$ | decays gradually |
| ARMA(p, q) | decays gradually | decays gradually |
This can be derived by considering AR(1) and MA(1). 
**AR(1)** Given $X_t=\phi X_{t-1}+\varepsilon_t$ where $|\phi|<1$, we can write
$$ X_t=\varepsilon_t+\phi \varepsilon_{t-1}+\phi^2 \varepsilon_{t-2}+\phi^3\varepsilon_{t-3}...$$
- Variance: $\gamma_0=\sigma^2(1+\phi^2+\phi^4+...)=\frac{\sigma^2}{1-\phi^2}$
- ACF: $\gamma_k=\phi \gamma_{k-1}$. So $\rho_k=\phi^k$
- PACF: $\alpha_1=\phi$, $\alpha_k=0$ for $k\geq 2$
MA(1) Given $X_t=\varepsilon_t+\theta \varepsilon_{t-1}$, we can write
$$
X_t=\theta X_{t-1}-\theta^2X_{t-2}+\theta^3X_{t-3}-\theta^4X_{t-4}+...
$$
- Variance: $\gamma_0=\sigma^2(1+\theta^2)$
- ACF: $\gamma_1=\theta \sigma^2$ and $\gamma_k=0$ for $k\geq 2$ since shocks are uncorrelated
- PACF: as shown by equation above
## ARIMA
**Augmented Dickey-Fuller(ADF**) tests whether unit roots exist. It runs regression
$$\Delta X_t = c + \gamma X_{t-1} + \delta_1 \Delta X_{t-1} + \dots + \delta_m \Delta X_{t-m} + u_t.$$
If $\gamma=0$, unit roots exist. By testing against some particular distribution whether $\gamma<0$, small p-value reject the null hypothesis which states that the unit root exist.
Some intuition: If $\gamma<0$, the price level is pulled back. When $X_{t-1}$ is high, change may be negative and when $X_{t-1}$ is low, change may be positive. If $\gamma=0$, a special case is for AR(1) where $\phi=1$(Verify in such case $\gamma=0$)

A natural question is the $\gamma$ would be difference for different $m$, how we gonna determine $m$?
**Akaike information criterion (AIC)** find the balance between likelihood and complexity.
$$
\mathrm {AIC}=-2\log L+2k
$$
Where $\log L$ is the maximized log likelihood for the model we are fitting.
> [!question] What is maximized log likelihood

This can also be used to determine $p$ and $q$ apart from simply looking at ACF and PACF trend.

## Forecasting
A fitted model forecasts $X_{n+h}$, $h$ periods after the last observation $X_n$, by its conditional expectation given the data up to $n$. 
For an AR(1) with mean $\mu$, $X_t - \mu = \phi (X_{t-1} - \mu) + \varepsilon_t$; applying this $h$ times gives
$$X_{n+h} - \mu = \phi^h (X_n - \mu) + \varepsilon_{n+h} + \phi\,\varepsilon_{n+h-1} + \dots + \phi^{h-1} \varepsilon_{n+1}.$$
The future shocks are unknown and have mean zero, so
$$\hat X_{n+h} = \mu + \phi^h (X_n - \mu), \qquad \mathrm{Var}(X_{n+h} - \hat X_{n+h}) = \sigma^2 (1 + \phi^2 + \dots + \phi^{2(h-1)}) = \sigma^2\, \frac{1 - \phi^{2h}}{1 - \phi^2}.$$
For a **random walk with drift**, $X_t = X_{t-1} + \mu + \varepsilon_t$, the same argument gives
$$\hat X_{n+h} = X_n + h\mu, \qquad \mathrm{Var}(X_{n+h} - \hat X_{n+h}) = h\,\sigma^2.$$

With normal shocks, a 95% **prediction interval** is $\hat X_{n+h} \pm 1.96$ standard errors. It accounts for the future shocks only, not for the error in the estimated parameters.

## Conditional variance
In previous section, we assume variance to be normally distributed, but one thing we can do is to change it to conditional variance.
$$r_t = \mu + \varepsilon_t, \qquad \varepsilon_t = \sigma_t z_t,$$
where $\sigma_t^2=\mathrm {VAR}(r_t \mid \text{past})$ and $z_t$ are independent follows $\mathcal N(0,1)$
- ARCH(1): $\sigma_t^2=\omega+\alpha\varepsilon^2_{t-1}$ A large shock yesterday will increase the variance regardless of sign.
- GARCH(1,1): $\sigma_t^2=\omega+\alpha \varepsilon^2_{t-1}+\beta\sigma^2_{t-1}$ Substituting repeatedly shows that $\sigma_t^2$ depends on all past squared shocks with weights $\alpha, \alpha\beta, \alpha\beta^2, \dots$, so one extra parameter gives the model a long memory.
Properties about GARCH(1,2):
- Uncorrelated shock. Multiplying $z$ makes $\varepsilon_t$ always white noise.
- Unconditional Variance stays the same. $\bar\sigma^2=\frac{w}{1-\alpha-\beta}$ by applying unconditional expectation.
- Persistent. Consider  $E[\sigma_t^2-\bar\sigma^2\mid\mathcal F_{t-2}] =(\alpha+\beta)(\sigma_{t-1}^2-\bar\sigma^2).$ Applying this recursively, we have $\mathbb{E}[\sigma_{t+h}^2 \mid \text{past}] - \bar\sigma^2 = (\alpha + \beta)^{h-1} (\sigma_{t+1}^2 - \bar\sigma^2)$ suggesting that the gap shrinks by the factor $(\alpha+\beta)$ each day