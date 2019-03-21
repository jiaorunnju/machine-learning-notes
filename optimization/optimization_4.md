# Accelerated Gradient Methods

## summary of GD

- smooth convex function:
  - upper bound: $O(\frac{1}{T})$
  - lower bound: $O(\frac{1}{T^2})$
- smooth strongly convex function:
  - upper bound: $O(\exp(-\frac{T}{k}))$
  - lower bound: $O(\exp(-\frac{T}{\sqrt{k}}))$

Does there exists an algorithm whose upper bound matches lower bound?

## accelerated gradient method

Three obervations about GD:

1. slow when step size is small
2. vibrate when step size is large
3. not converge if step size is larger than some threshold

### momentum

like a ball rolling down from a hill, the speed is accumulated during optimization.
$$
d_t = \gamma d_{t-1} + f'(w_{t-1}) \\
w_t = w_{t-1} - \eta d_t = w_{t-1} - \eta f'(w_{t-1}) - \eta\gamma d_{t-1}
$$
which go annother step compared with GD

### Nesterov accelerated gradient

$$
d_t = \gamma d_{t-1} + f'(w_{t-1}-\eta \gamma d_{t-1}) \\
w_t = w_{t-1} - \eta d_t = w_{t-1} - \eta\gamma d_{t-1}- \eta f'(w_{t-1}-\eta\gamma d_{t-1})
$$
if we let $u_{t-1} = w_{t-1}-\eta\gamma d_{t-1}$, we have
$$
w_t = u_{t-1} - \eta f'(u_{t-1})\\
u_t = w_t - \eta\gamma d_{t} = w_t + \gamma(w_t-w_{t-1})
$$

then another form of NAG is
$$
d_t = \gamma d_{t-1} + f'(u_{t-1}) \\
w_t = u_{t-1} - \eta f'(u_{t-1})\\
u_t = w_t - \eta\gamma d_{t} = w_t + \gamma(w_t-w_{t-1})
$$
add the last two equations, we get
$$
u_t = u_{t-1} - \eta \bar{d_t}\\
\bar{d_t} = \gamma d_t + f'(u_{t-1})
$$
thus
$$
\bar{d_t}-\gamma \bar{d_{t-1}} = f'(u_{t-1})+\gamma (f'(u_{t-1})-f'(u_{t-2}))
$$
so NAG can be viewed as the following two steps:
$$
\bar{d_t} = \gamma \bar{d_{t-1}}+f'(u_{t-1})+\gamma(f'(u_{t-1})-f'(u_{t-2})) \\
u_t = u_{t-1} - \eta \bar{d_t}
$$
compared to momentum, NAG's update direction contains annother term $\gamma(f'(u_{t-1})-f'(u_{t-2}))$, this is the change of gradient. To some degrees NAG uses second order informations.

## Forms of NAG

### normal form

1. input: T
2. initialize: $w_0, \theta_0 >0, u_0=w_0$
3. for t=0,1,...
   1. compute $f'(u_t)$ and $w_{t+1}=u_t-\frac{1}{\beta}f'(u_t)$
   2. compute $\theta_{t+1}\in [0,1]$ satisfy $\theta_{t+1}^2=(1-\theta_{t+1})\theta_t^2+\frac{\theta_{t+1}}{k}$
   3. compute $u_{t+1}=w_{t+1}+\frac{\theta_t(1-\theta_t)}{\theta_{t+1}+\theta_t^2}(w_{t+1}-w_t)$
4. output: $w_T$

### smooth strongly convex

1. input: T
2. initialize: $w_0, u_0=w_0$
3. for t=0,1,...
   1. compute $f'(u_t)$ and $w_{t+1}=u_t-\frac{1}{\beta}f'(u_t)$
   2. compute $u_{t+1}=w_{t+1} + \frac{\sqrt{k}-1}{\sqrt{k}+1}(w_{t+1}-w_t)$
4. output: $w_T$

### smooth function

1. input:T
2. initialize: $w_0, \lambda_0 = \frac{\sqrt{5}+1}{2}, u_0=w_0$
3. for t=0,1,...
   1. compute $f'(u_t)$ and $w_{t+1}=u_t-\frac{1}{\beta}f'(u_t)$
   2. compute $\lambda_{t+1}=\frac{1+\sqrt{1+4\lambda_t^2}}{2}$
   3. compute $u_{t+1}=w_{t+1}+\frac{\lambda_t-1}{\lambda_{t}+1}(w_{t+1}-w_t)$
4. output: $w_T$

## summary

NAG can achieve best convergence rate with first order information.