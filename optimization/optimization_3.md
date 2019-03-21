# Smooth Strongly Convex Function

## strongly convex

$$
\alpha I_n \le f''(w) \le \beta I_n
$$
and at any stationary point $w$
$$
f(u) \ge f(w) + \frac{\alpha}{2}||u-w||^2
$$

define function class $S_{\alpha}^k$ to be $k$-th order differentiable and $\alpha$-strongly-convex functions. It has several properties:

1. $f(u)\ge f(w)+f'(w)^T(u-w)+\frac{\alpha}{2}||u-w||^2$
2. $\theta f(w)+(1-\theta)f(u) \ge f(\theta w+ (1-\theta)u) + \theta(1-\theta)\frac{\alpha}{2}||u-w||^2$
3. $(f'(w)-f'(u))^T(w-u)\ge \alpha||u-w||^2$

## lower bound

There exists $f$, we have
$$
f(w_T)-f(w^*) \ge \frac{\alpha}{2}(\frac{\sqrt{k}-1}{\sqrt{k}+1})^{2T}||w_0-w^*||^2 \\
||w_t-w^*||^2 \ge (\frac{\sqrt{k}-1}{\sqrt{k}+1})^{2T}||w_0-w^*||^2
$$
converge rate is $O(\exp (-\frac{T}{\sqrt{k}}))$, $k$ is condition number.

## upper bound

Just like convex functions, we can get
$$
f(w_{t+1})-f(w^*)\le \frac{\beta-\alpha}{2}||w_t-w^*||^2-\frac{\beta}{2}||w_{t+1}-w^*||^2
$$
since $f(w_{t+1})-f(w^*)\ge 0$, we have
$$
\frac{\beta-\alpha}{2}||w_t-w^*||^2 \ge \frac{\beta}{2}||w_{t+1}-w^*||^2
$$
which is
$$
||w_{t+1}-w^*||^2 \le (1-\frac{1}{k})||w_t-w^*||^2
$$
we can get
$$
||w_T-w^*||^2 \le (1-\frac{1}{k})^T||w_0-w^*||^2
$$
And plug this into the inequality above,
$$
f(w_T)-f(w^*) \le \frac{\beta-\alpha}{2}||w_{T-1}-w^*||^2 \\
\le \frac{\beta}{2}\exp(-\frac{T}{k})||w_0-w^*||^2
$$
Thus the converge rate is $O(\exp(-\frac{T}{k}))$