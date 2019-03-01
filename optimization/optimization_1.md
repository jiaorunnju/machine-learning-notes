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

## Kinds of algo

1. zero-order: $O$ returns $f(w)$
2. first-order: $O$ returns $f(w), f'(w)$
3. second-order: $O$ returns $f(w), f'(w), f''(w)$

## Grid Search

let $f$ be a $\beta$-lipschitz function, which is defined on $[0,1]^n$

Then grid search runs like this:

1. split each dimention $[0,1]$ equally into $p+1$ points.
2. find $\hat{w}$ in grid points which minimize $f$
3. return $(\hat{w},f(\hat{w}))$

It is easy to show that: $f(\hat{w})-f(w^*)\leq \frac{\beta}{2p}$. Then let $\frac{\beta}{2p}\leq \epsilon$, we have $p\geq \frac{\beta}{2p}$, which means the complexity of grid search is $O((\frac{\beta}{2p})^n)$

We can also show that: if $O$ is queried less than $(\frac{\beta}{2\epsilon})^n$, there exists a function $f$ that the error is larger than $\epsilon$

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

