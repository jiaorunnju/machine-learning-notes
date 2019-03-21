# Local Norms

## a tighter bound

assume loss functions are linear and $z_{t,j}\ge 0$ for all $t=1,2,...,T$ and $j=1,2,...,d$
, then the exponentiated gradient achieves the following regret bounds:
$$
\text{Regret}(u)\le \psi(u)-\psi(w_1)+\eta \sum_{i=1}^T\sum_{j=1}^dw_{t,j}z_{t,j}^2
$$
since
$$
\sum_{j=1}^dw_{t,j}z_{t,j}^2\le ||z_t||^2_{\infty}
$$
this allows some components of the loss to be large provided that the corresponding weights are small.

## example (exponentiated gradient in realizable setting)

Assume the loss vector is bounded: $z_t\in [0,1]^d$

Assume there is an expert $u\in \Delta_d$ with zero cumulative loss (realizable)

then
$$
\text{Regret}(u) = \sum_{t=1}^Tw_tz_t-\sum_{t=1}^Tuz_t \le\frac{\log d}{\eta} + \eta \sum_{i=1}^T\sum_{j=1}^dw_{t,j}z_{t,j}^2
$$
where $\sum_{j=1}^dw_{t,j}z_{t,j}^2\le w_tz_t$, by rearanging, we have
$$
\text{Regret}(u) \le \frac{\log d}{\eta (1-\eta)}
$$

## proof

by definition of Bregman divergence, we have
$$
D_{\psi^*}(\theta_{t+1}||\theta_t)=\psi^*(\theta_{t+1})-\psi^*(\theta_{t})-\nabla\psi^*(\theta_t)(\theta_{t+1}-\theta_t)
$$
which is
$$
D_{\psi^*}(\theta_{t+1}||\theta_t)=\psi^*(\theta_{t+1})-\psi^*(\theta_{t})+w_t\cdot z_t
$$
then we have
$$
\begin{aligned}
    D_{\psi^*}(\theta_{t+1}||\theta_t)&=\psi^*(\theta_{t+1})-\psi^*(\theta_{t})+w_t\cdot z_t\\
    &=\frac{1}{\eta}\log \left( \frac{\sum_{j=1}^de^{\eta\theta_{t+1,j}}}{\sum_{j=1}^de^{\eta\theta_{t,j}}} \right) + w_i\cdot z_i\\
    &= \frac{1}{\eta}\log \left( \sum_{j=1}^d w_{t,j}e^{-\eta z_{t,j}} \right)+ w_i\cdot z_i\\
    &\le \frac{1}{\eta}\log \left( \sum_{j=1}^d w_{t,j}(1-\eta z_{t,j}+\eta^2 z_{t,j}^2) \right)+ w_i\cdot z_i\\
    &\le \frac{1}{\eta}\log \left( \sum_{j=1}^d 1-w_{t,j}(\eta z_{t,j}-\eta^2 z_{t,j}^2) \right)+ w_i\cdot z_i\\
    &\le \frac{1}{\eta} \sum_{j=1}^dw_{t,j}(-\eta z_{t,j}+\eta^2 z^2_{t,j})+ w_i\cdot z_i\\
    &=\eta \sum_{i=1}^T\sum_{j=1}^dw_{t,j}z_{t,j}^2

\end{aligned}
$$