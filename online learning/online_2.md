# Follow The Regularized Leader

## FTRL

let $\phi$ be a regualarizer, we choose $w_t$ with
$$
w_t = \arg\min_{w\in S} \psi(w) + \sum_{i=1}^{t-1}f_i(w)
$$

## linear optimization

let $\psi(w)=\frac{1}{2\eta}||w||_2^2,f_t(w)=wz_t$, then
$$
w_t=\arg\min_{w\in S}(\frac{1}{2\eta}||w||_2^2-w\cdot \theta_t)
$$
where $\theta_t=-\sum_{i=1}^{t-1}z_t$, which is the negative sum of gradients $z_i$.

If $S$ is $R^d$, then $w_t=\eta\theta_t$ or
$$
w_t=w_{t-1}-\eta z_t
$$

If $S$ is not $R^d$, then we need projection onto $S$
$$
w_t = \Pi(\eta\theta_t)
$$
which is called **lazy projection** since we only project when we need to output a $w$ to predict.

Note that:

- in batch learning, regularizer is part of objective, while in online learning, objective is regret.
- in online learning, the regularizer is used to define updates.

### Regret

$$
\text{Regret}(u)\le \frac{1}{2\eta}||u||_2^2 + \eta \sum_{t=1}^T||z_t||_2^2
$$

proof:

we can view FTRL as a FTL with the first loss is $\psi$. Then we have
$$
\psi(w_0)-\psi(u) + \sum_{t=1}^T[f_t(w_t)-f_t(u)] \le \psi(w_0)-\psi(w_1) + \sum_{t=1}^T[f_t(w_t)-f_t(w_{t+1})]
$$
since $\psi(w_1)<0$
$$
\sum_{t=1}^T[f_t(w_t)-f_t(u)] \le \frac{1}{2\eta}||u||_2^2 + \sum_{t=1}^T[f_t(w_t)-f_t(w_{t+1})]
$$
Now we need to bound the last term:
$$
\begin{aligned}
    f_t(w_t)-f_t(w_{t+1})&=z_t(w_t-w_{t+1})\\
    &=||z_t||_2||w_t-w_{t+1}||_2 \text{  [Cauchy-Schwartz]}\\
    &=||z_t||_2||\Pi(\eta\theta_t)-\Pi(\eta\theta_{t+1})||_2\\
    &=||z_t||_2||\eta\theta_t-\eta\theta_{t+1}||_2\\
    &=\eta||z_t||_2^2
\end{aligned}
$$
where we use that projection decrease distances. We can now complete the proof by plugging this into the inequality above.

## OGD

Many loss functions in reality is not linear, we can run FTRL on linear approximations of these functions.

Here is OGD:

1. let $w_1=0$
2. for t=1,...,T
   1. predict $w_t$ and receive $f_t$
   2. take subgradient $z_t\in \partial f_t(w_t)$
   3. if $S$ is $R^d$, $w_{t+1}=w_{t}-\eta z_t$
   4. if $S$ is not $R^d$, $w_{t+1}=\Pi(\eta\theta_{t+1}), \theta_{t+1}=\theta_t-z_t$

Regret analysis:

Due to convexity, we have
$$
f_t(u)\ge f_t(w_t)+z_t^T(u-w_t) 
$$
which is
$$
f_t(w_t)-f_t(u)\le z_tw_t-z_tu
$$
assume $||z_t||_2\le L$, $||u||_2\le B$, by setting $\eta=1/\sqrt{T}$, we have
$$
\text{Regret}\le BL\sqrt{2T}
$$

From above we can see that the regret of OGD with convex losses is bounded by linear functions. Or in other words, linear functions are the hardest to optimize using online convex optimization.