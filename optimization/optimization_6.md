# Proximal Gradient Method

## problem

$$
\min_x f(x)=g(x)+h(x)
$$
where $g(x)$ is differentiable while $h(x)$ is not.

## proximal mapping

for functions like $f(x)=g(x)+h(x)$, where $g$ is differentiable while $h$ is not, we can define proximal mapping
$$
prox_t (x) = \arg\min_z \frac{1}{2t}||z-x||_2^2 + h(z)
$$
and then try to find the optimal value of $prox_t$

- $h(x)=0$, $prox_t(x)=x$
- $h(x)=I_C(x)$, $prox_t(x)=\arg\min_{z\in C} \frac{1}{2t}||z-x||_2^2$, which is in fact projection.
- $h(x)=\lambda||x||_1$, $prox_t(x)=S_{\lambda t}(x)$,
  where
  $$
  S_{\lambda t}(x) = 
  \begin{cases}
      x-\lambda t &\text{if }x \gt \lambda t\\
      0 & \text{if} -\lambda t \le x \le \lambda t\\
      x+\lambda t &\text{if } x \lt -\lambda t\\
  \end{cases}
  $$
  can be proved with $0\in partial gradients$
- $h(x)$ is nuclear norm: just do SVD and perform soft-thresholding on the singular value.

## proximal gradient descent

A step in GD with fixed step size can be viewed as taking the second-orer taylor expansion of $f$
$$
x^+=\arg\min_z f(x)+\nabla f(x)^T(z-x) + \frac{1}{2t}||z-x||^2_2
$$
thus in proximal gradient descent, we still expand $g$ while let $h$ unchanged.
$$
x^+=\arg\min_z f(x)+\nabla f(x)^T(z-x) + \frac{1}{2t}||z-x||^2_2 + h(z)\\
=\arg\min_z \frac{1}{2t}||z-(x-t\nabla g(x))||+h(z)\\
=prox_t(x-t\nabla g(x))
$$

By changing $h$, we can get differnet kinds of algorithm, e.g., GD, projected GD,...

## convergence rate

with fixed step size $t$, we have
$$
f(w_k)-f^* \le \frac{||w_0-w^*||_2^2}{2tk}
$$
we can get a $O(\frac{1}{T})$ rate, which is similar to GD.

## summary

Note that the complexity of $prox_t$ depends on how complex $h$ is. The $prox_t$ should be computed analytically.
