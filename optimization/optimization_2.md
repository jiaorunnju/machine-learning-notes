# Smooth Convex Optimization

## convex functions

For function class $F$, assume:

1. zero gradient -> global optimal
2. close to non-negative operations
3. linear functions belong to $F$

for $f\in F, w\in R^n$, consider function $\phi(u)=f(u)-f'(w)^Tu$. From assumptions we know that $\phi \in F$. Since $\phi '(w)=0$, $w$ is global optimal. We have:
$$
f(u)-f'(w)^Tu = \phi(u) \geq \phi(w)=f(w)-f'(w)^Tw
$$
which is exactly the first order condition of convex functions:
$$
f(u) \geq f(w)+f'(w)^T(u-w)
$$

Introduce function class $F^k$: $k$-th order differentiable convex functions on $R^n$.

Several equal definitions of $F^1$:

1. for any $w, u$, we have $f(u) \geq f(w)+f'(w)^T(u-w)$
2. for any $w, u$, any $\theta \in [0,1]$, we have $f(\theta w+(1-\theta) u ) \le \theta f(w)+(1-\theta)f(u)$
3. for any $w, u$, we have $(f'(w)-f'(u))^T(w-u)\ge 0$

Function class $F_{\beta}^{k,p}$: $k$-th order differentiable and $p$-th order derivative is $\beta$-lipschitz.

Properties of $F_{\beta}^{1,1}$:

1. $0 \le f(u)-f(w)-f'(w)^T(u-w) \le \beta /2 ||w-u||^2$
2. $f(w)+f'(w)^T(u-w)+\frac{1}{2\beta}||f'(w)-f'(u)||^2 \le f(u)$
3. $\frac{1}{\beta} ||f'(w)-f'(u)||^2 \le (f'(w)-f'(u))^T(w-u)$
4. $(f'(w)-f'(u))^T(w-u) \le \beta ||w-u||^2$
5. $\theta f(w) +(1-\theta)f(u)\ge f(\theta w+(1-\theta)u) + \frac{\theta(1-\theta)}{2\beta}||f'(w)-f'(u)||^2$
6. $\theta f(w) +(1-\theta)f(u)\le f(\theta w+(1-\theta)u) + {\theta(1-\theta)} \frac{\beta}{2} ||f'(w)-f'(u)||^2$

Properties of $F_{\beta}^{2,1}$:

1. $0\le f''(w) \le \beta I_n$

## lower bound of smooth convex functions

show that a function f, with gradient descent, the complexity is 
$$
\min_{1\le t \le k} f(w_t)-f* \ge \frac{3\beta}{32}\frac{||w_0-w^*||^2}{(k+1)^2}
$$

which means the lower bound of convergence rate is $O(\frac{1}{k^2})$

## proof of upper bound

let $f$ be a $\beta$-smooth convex function on a convex set $S$:
$$
\min_{x\in S} f(x)
$$

### optimal condition

a first order optimal condition:
$$
v\in \arg\min_S f(w) \Leftrightarrow -\nabla f(v)^T(w-v) \le 0
$$
which means at $v$, there is no directions that $f$ can decrease.

### projection

let $S\subseteq R^n$ be a convex set. The projection of $u$ onto $S$ is $v$. So for all $w\in S$, we have $(u-v)^T(w-v)\le 0$. Also we have $||\Pi(x)-\Pi(y)||^2\le ||x-y||^2$

### proof

let $w_{t+1} = \Pi(w_t-\eta \nabla f(w_t))$
we have:
$$
f(w_{t+1})-f(u)=f(w_{t+1})-f(w_t)+f(w_t)-f(u) \\
\le \nabla f(w_t)^T(w_{t+1}-w_t)+\frac{\beta}{2}||w_{t+1}-w_t||^2 + \nabla f(w_t)^T(w_{t}-u) \\
=\nabla f(w_t)^T(w_{t+1}-u)+\frac{\beta}{2}||w_{t+1}-w_t||^2
$$
Then according to properties of projection, we have
$$
(w_t-\frac{1}{\beta}\nabla f(w_t) - w_{t+1})^T(u-w_{t+1})\le 0 \\
\Leftrightarrow \nabla f(w_t)^T(w_{t+1}-u)\le \beta(w_t-w_{t+1})
$$
Thus
$$
f(w_{t+1})-f(u) \le \beta (w_t-w_{t+1})^T(w_{t+1}-u)+\frac{\beta}{2}||w_{t+1}-w_t||^2 \\
= (w_t-w_{t+1})^T(w_{t+1}-w_t+w_t-u)+\frac{\beta}{2}||w_{t+1}-w_t||^2 \\
= (w_t-w_{t+1})^T(w_{t}-u)-\frac{\beta}{2}||w_{t+1}-w_t||^2 \\
= \beta \frac{||w_t-w_{t+1}||^2+||w_t-u||^2-||u-w_{t+1}||^2}{2}- \frac{\beta}{2}||w_{t+1}-w_t||^2 \\
=\frac{\beta}{2}||w_t-u||^2-\frac{\beta}{2}||w_{t+1}-u||^2
$$
let $u=w_t$, we have
$$
f(w_{t+1})-f(w_t)\le -\frac{\beta}{2}||w_{t+1}-w_t||^2
$$
multiply $t$ on both side,
$$
(t+1)f(w_{t+1})-tf(w_t) - f(w_{t+1}) \le -\frac{\beta t}{2}||w_{t+1}-w_t||^2 \\
\dots \\
(0+1)f(w_1) - 0f(w_0) - f(w_1) \le -\frac{\beta 0}{2}||w_1-w_0||^2
$$
sum and we can get:
$$
Tf(w_T)-\sum_{t=1}^T f(w_t) \le -\frac{\beta}{2} \sum_{t=0}^{T-1}t||w_t-w_{t-1}||^2
$$
let $u=w^*$, we have
$$
f(w_{t+1}) -f(w^*) \le \frac{\beta}{2}||w_t-w^*||^2-\frac{\beta}{2}||w_{t+1}-w^*||^2 \\
\dots \\
f(w_{1}) -f(w^*) \le \frac{\beta}{2}||w_0-w^*||^2-\frac{\beta}{2}||w_{1}-w^*||^2
$$
sum and we get
$$
\sum_{t=1}^T f(w_t)-Tf(w^*) \le \frac{\beta}{2}||w_0-w^*||^2-\frac{\beta}{2}||w_{T}-w^*||^2
$$
conbine two inequalities above, we get
$$
f(w_T)-f(w^*) \le \frac{\beta ||w_0-w^*||^2}{2T}
$$
Thus with fixed step size $\frac{1}{\beta}$, the converge rate is $O(\frac{1}{T})$