# Regret of OMD

## Bregman divergence

let $f$ be a continous convex function. The Bregman divergence between $w$ and $u$ is:
$$
D_f(w||u)=f(w)-f(u)-\nabla f(u)^T(w-u)
$$
which captures the error between $f$ and linear approximations at $u$

Note that $D_f(w||u)\ge 0$ since $f$ is convex.

### examples

- quadratic regularizer: $f(w)=\frac{1}{2}||w||_2^2$, then $D_f(w||u)=\frac{1}{2}||w-u||_2^2$
- entropy regularizer: $f(w)=\sum_i w_i\log w_i$, then $D_f(w||u)=KL(w||u)$

## Regret

assume loss is linear

$$
\text{Regret}(u)\le [\psi(u)-\psi(w_1)]+\sum_{t=1}^T D_{\psi^*}(\theta_{t+1}||\theta_t)
$$

proof:

for the expert:
$$
\psi^*(\theta_{T+1})\ge u\cdot \theta_{t+1}-\psi(u) \\
= \sum_{i=1}^T-u\cdot z_t - \psi(u)
$$
Note that if $u$ is the best expert, the bound is tight.

for the learner:

$$
\begin{aligned}
    \psi^*(\theta_{t+1}) &= \psi^*(\theta_1)+\sum_{t=1}^T[\psi^*(\theta_{t+1})-\psi^*(\theta_t)]\\
    &=\psi^*(\theta_1)+\sum_{t=1}^T[\nabla \psi^*(\theta_t)(\theta_{t+1}-\theta_t)+D_{\psi^*}(\theta_{t+1}||\theta_t)]\\
    &=\psi^*(\theta_1)+\sum_{t=1}^T[-w_t\cdot z_t+D_{\psi^*}(\theta_{t+1}||\theta_t)]
\end{aligned}
$$
subtract them we can get that:
$$
\text{Regret}(u)\le [\psi(u)-\psi(w_1)]+\sum_{t=1}^T D_{\psi^*}(\theta_{t+1}||\theta_t)
$$

## strong convexity

A function $f$ is $\alpha$-strong convex with respect to a norm $||\cdot||$ iff for all $u,v$
$$
D_f(w||u)\ge \frac{\alpha}{2}||w-u||^2
$$

A function $f$ is $\alpha$-strong smooth with respect to a norm $||\cdot||$ iff for all $u,v$
$$
D_f(w||u)\le \frac{\alpha}{2}||w-u||^2
$$

if $f(w)$ is $1/\eta$ strongly convex with respect to a norm $||\cdot||$, then $f^*(\theta)$ is $\eta$-strongly smooth respect to the dual norm $||\cdot||_*$

thus suppose $\psi$ is a $\eta$-strongly convex function, then we have regret:
$$
\text{Regret}(u) \le \psi(u)-\psi(w_1)+\frac{\eta}{2}\sum_{t=1}^T ||z_t||_*^2
$$

## example

entropy regularizer is $\frac{1}{\eta}$-strongly convex to norm $|\cdot|_1$

- quadratic: $\max_u\psi(u)-\min_u\psi(u)\le \frac{1}{2\eta}, ||z_t||_2\le \sqrt{d}$
- entropy: $\max_u\psi(u)-\min_u\psi(u)\le \frac{\log d}{\eta}, ||z_t||_{\infty}\le 1$