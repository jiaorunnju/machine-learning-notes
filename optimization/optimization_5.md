# Subgradient method

## subgradient

subgradient of $f$ at $x$ is a vector $g$ that satisfies the inequality
$$
f(y)\ge f(x)+g^T(y-x)
$$
if $f$ is differentiable, $g$ is $\nabla f(x)$

properties of subgradient:

1. scaling: $\partial (af) = a \partial f$
2. addinf: $\partial (f_1+f_2) = \partial f_1 + \partial f_2$
3. affine: if $g(x)=f(Ax+b)$, then $\partial g(x) = A^T\partial f(Ax+b)$

## optimal condition

for any function $f$, $x$ is optimal for $f$ iff
$$
0\in \partial f(x)
$$

## subgradient method

Like GD, just substitute gradient with subgradient.
$$
w_{t+1} = w_t - \eta_t g_t
$$
where $g_t\in \partial f(w_t)$

Subgradient method is not necessarily a descent method, so we usually choose
$$
f(w_{best}^k)=\min_{i=0...k} f(x_i)
$$

Another difference is subgradient method does not have a certain way to choose step-size.

- fixed step size: $\eta_k = \eta$
- deminishing step size: $\sum_{k=1}\eta_k^2<\infty, \sum_{k=1}\eta_k = \infty$

For constant step size, the subgradient algorithm is guaranteed to converge to within some range of the optimal value,
$$
\lim_{k\to \infty}f_{best}^k-f^* \lt \epsilon
$$
For the diminishing step size rule (and therefore also the square summable but not summable step size rule), the algorithm is guaranteed to converge to the optimal value,
$$
\lim_{k\to \infty}f(w_k)=f^*
$$

## proof
$$
||x_{k+1}-x^*||_2^2=||x_{k}-\eta_kg_k-x^*||_2^2\\
=||x_{k}-x^*||_2^2-2\eta_kg_k(x_k-x^*)+\eta_k^2||g_k||_2^2\\
\le ||x_{k}-x^*||_2^2-2\eta_k(f(x_k)-f^*)+\eta_k^2||g_k||_2^2
$$
where the last line use the definition of subgradient.
Applying the inequality above recursively, we have:
$$
||x_{k+1}-x^*||_2^2\le ||x_1-x^*||_2^2-2\sum_{i=1}^k\eta_i(f(x_i)-f^*)+\sum_{i=1}^k\eta_i^2 ||g_i||_2^2
$$
using $||x_{k+1}-x^*||_2^2 > 0$, we have
$$
2\sum_{i=1}^k\eta_i(f(x_i)-f^*) \le ||x_1-x^*||_2^2 + \sum_{i=1}^k\eta_i^2 ||g_i||_2^2
$$
combine this with 
$$
\sum_{i=1}^k\eta_i(f(x_i)-f^*) \ge \left( \sum_{i=1}^k\eta_i \right)\min_{i=1...k}(f(x_i)-f^*)
$$
we have the inequality
$$
f_{best}^k-f^* \le \frac{||x_1-x^*||_2^2 + \sum_{i=1}^k\eta_i^2 ||g_i||_2^2}{2\sum_{i=1}^k\eta_i}
$$
Assume $||g_i||_2 < G$, we have
$$
f_{best}^k-f^* \le \frac{||x_1-x^*||_2^2 + G^2\sum_{i=1}^k\eta_i^2}{2\sum_{i=1}^k\eta_i}
$$
Thus for fixed step size condition, we can get
$$
f_{best}^k-f^* \le \frac{||x_1-x^*||_2^2 + G^2h^2k}{2hk}
$$
The algorithm return a result that is $G^2h/2$ optimal. If we want an $\epsilon$ optimal result:
$$
\frac{R^2}{2hk}+\frac{G^2h}{2}\le \epsilon
$$
let both parts be $\frac{\epsilon}{2}$, we can get a $O(\frac{1}{\sqrt{T}})$ converge rate.