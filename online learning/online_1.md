# Online Convex Optimization

## online learning

online learning can be thought as a game between a learner and nature:

- iterate t=1,...,T
  - learner receives input $x_t$
  - learner outputs prediction $p_t$
  - learner receives loss $l_t$
  - learner updates model

The loss does not to be i.i.d, it can be adverserial.

## regret

the regret with respect to an expert $h\in H$ is:
$$
\text{Regret}(h)=\sum_{t=1}^T l(p_t)-l(h(x_t))
$$
the regret with respect to $H$ is:
$$
\text{Regret}=\sum_{t=1}^T l(p_t) - \min_{h\in H}\sum_{t=1}^Tl(h(x_t))
$$
we want the regret is some sub-linear function of $T$, which means that the regret of each example goes to zero.

## follow the leader

let $f_1,...,f_T$ be the sequence of loss functions played by nature. At the $t$-th iteration, the learner choose $w_t$ which minimizes the cumulative loss:
$$
w_t = \arg\min_{w\in S}\sum_{i=1}^{t-1}f_i(w)
$$
Solving this may be hard, but we will consider some special cases with analytical solutions.

A useful lemma for FTL is:
$$
\text{Regret}(u)=\sum_{t=1}^T[f_t(w_t)-f_t(u)]\le \sum_{t=1}^T[f_t(w_t)-f_t(w_{t+1})]
$$
which can be easily proved by induction.

## quadratic optimization

Assume the loss is quadratic:
$$
f_t(w)=\frac{1}{2}||w-z_t||^2_2
$$
where $||z_t||_2\le L$, then FTL has close form solution:
$$
w_t = \frac{1}{t-1}\sum_{i=1}^{t-1}z_i
$$
the regre is:
$$
\begin{aligned}
f_t(w_t)-f_t(w_{t+1}) &=\frac{1}{2}||w_t-z_t||^2_2-\frac{1}{2}||(1-1/t)w_t+(1/t)z_t-z_t||_2^2\\
&=\frac{1}{2}(1-(1-1/t)^2)||w_t-z_t||^2_2\\
&\le (1/t)||w_t-z_t||^2_2\\
&\le (1/t)4L^2
\end{aligned}
$$
since
$$
\sum_{t=1}^T\frac{1}{t}\le 1+ \int_1^T\frac{1}{t}dt=\log{T}+1
$$
then sum over $t$
$$
\text{Regret} \le 4L^2\log{T}+1
$$
Note that the difference between $w_t$ and $w_{t+1}$ is small (measured by loss). This makes sense because averages are stable.

## linear optimization

let cost function be
$$
f_t(w)=wz_t
$$
and $(z_1,z_2,...,)=(-0.5,1,-1,1,-1,...)$

Then the minimizer computed by FTL is
$$
(w_1,w_2,...)=(0, 1, -1,1,-1,...)
$$
the cumulative loss is $T-1$, and loss of expert $u=0$ is 0, then
$$
\text{Regret}\ge T-1
$$
Note that for linear functions, the $w_t$ and $w_{t+1}$ may not be close to each other. It seems that FTL works if $f_t$ provides stabability.