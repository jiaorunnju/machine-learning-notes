# Non-linear Optimization

## A common framework for optimization:

1. input start point $w_0$ and $\epsilon$
2. initialize counter $k=0$, and accumulated information set $I_{-1}=\emptyset$
3. call $O$ to query information on $w_k$
4. update $I_k = I_{k-1}\cup (w_k, O(w_k))$
5. use $I_k$ update $w_k$
6. check stop condition. if satisfy, break. if not, continue.

## Complexity of Algorithm

1. number of $O$ is called
2. number of mathematical operations done

Consider 1 more.

## Kinds of algorithms

1. zero-order: $O$ returns $f(w)​$
2. first-order: $O$ returns $f(w), f'(w)$
3. second-order: $O$ returns $f(w), f'(w), f''(w)$

## Grid Search

let $f$ be a $\beta$-lipschitz function, which is defined on $[0,1]^n$

Then grid search runs like this:

1. split each dimention $[0,1]$ equally into $p+1$ points.
2. find $\hat{w}$ in grid points which minimize $f$
3. return $(\hat{w},f(\hat{w}))$

### Upper Bound

It is easy to show that: $f(\hat{w})-f(w^*)\leq \frac{\beta}{2p}$. Then let $\frac{\beta}{2p}\leq \epsilon$, we have $p\geq \frac{\beta}{2p}$, which means the complexity of grid search is $O((\frac{\beta}{2p})^n)$



### Lower Bound

We can also show that: if $O$ is queried less than $(\frac{\beta}{2\epsilon})^n$, there exists a function $f$ that the error is larger than $\epsilon$. Let $p=\frac{\beta}{2\epsilon}$If the number of time $O$ is called is less than $p^n$, then there exists a point $w_0$ that is not visited and there exists a cube B which size is $\frac{1}{p}$, and does not include other grid points. Assume $f=0$ at other grid points, by setting middle point of B, we can prove the lower bound.

Note that the lower bound matches the upper bound, thus grid search is the best algorithm for this kind of problem.

For non-linear optimization problems, it is quite hard to find global optimal. How about find a local optimal?

## Gradient Descent

>intuition: monotonical bounded sequence converges.

If we can build a sequence of montonically decreasing sequence, we will get a local minimum.

Conditions when applying GD:

1. function is differentiable
2. there exists a suitable step size ($\sin (1/x)$ does not exist such a distance when x is near zero), function is lipschitz

Now we introduce function class:
$$
C_{\beta}^{k,p}
$$
which is $k$-th order differentiable and $p$-th order derivative is $\beta$-lipschitz.

$C_{\beta}^{1,1}$ has the following properties:

1. first order derivative exists
2. $|| f'(w)-f'(u) ||_2\leq \beta |w-u|$
3. if second order derivative exists, then $||f''(w)||_2\leq \beta I_n$
4. $|f(u)-f(w)-f'(w)^T(u-w)|\leq \beta/2 ||u-w||^2$

$C_{\beta}^{2,2}$ have the following properties:

1. first and second order derivative exists
2. $|| f''(w)-f''(u) ||_2\leq \gamma ||w-u||$
3. $||f'(u)-f'(w)-f''(w)^T(u-w)||\leq \gamma/2 ||u-w||^2$
4. $|f(u)-f(w)-f'(w)^T(u-w)-0.5(u-w)^Tf''(w)(u-w)|\leq \gamma/6 ||u-w||^3$
5. if $||u-w||=r$, then $f''(w)-\gamma r I_n \leq f''(u) \leq f''(w)+\gamma r I_n$

using constant step size, GD acts like the following:

1. choose a start point
2. $w_{k+1}=w_k-\eta f'(w_k)$

###  Converge Rate

let $u=w-\eta f'(w_k)$
$$
f(u)\leq f(w)+f'(w)^T(u-w)+\frac{\beta}{2}||u-w||^2 \\
= f(w) - \eta ||f'(w)||^2 + \frac{\beta \eta^2}{2}||f'(w)||^2
$$

let $\eta = \frac{1}{\beta}$, which maxmize the equation before, we have
$$
f(u) \leq f(w) - \frac{1}{2\beta}||f'(w)||^2
$$

Thus, whatever step strategy we take, we have
$$
f(w_k) - f(w_{k+1}) \geq \frac{1}{2\beta}||f'(w_k)||^2
$$

sum over k, we have
$$
\frac{1}{2\beta} \sum_{k=0}^T ||f'(w_k)||^2 \leq f(w_0) - f(w^*)
$$
the right hand is a bounded value, which means GD converges to a stationary point. Convergence rate is $O(1/\sqrt{T})$.

Note that this is just a convergence rate on gradient, not on function values.
