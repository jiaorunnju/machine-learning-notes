# Adversarial Bandits: expert advice

In online learning with expert advice, on each iteration, after we choose one of the $d$
experts/actions, nature reveals the loss vector $z_t$ for every single action.

However, in many applications such as clinical trials or advertisement placement, packet routing, we only get to observe the loss of the action we took, not the losses of the actions we didn't take.

This setting is known as the multi-armed bandit problem, which is a type of online learning problem with partial feedback.

This problem is much more difficult. Intuitively, the learner should choose actions that we expect to yield low loss, but it must choose which actions to explore to get more information about the losses. Thus, the multi-armed bandit problem exposes the challenges of the classic exploration/exploitation tradeoff.

## method

now we want to use an unbiased estimator of $z_t$ to help learning:
$$
\hat{z}_{t,a}=\\
\begin{cases}
    \frac{z_{t,a}}{w_{t,a}} &\text{if } a=a_t\\
    0 &\text{otherwise}
\end{cases}
$$

## regret of Bandit-EG

$$
E[\text{Regret}(u)]\le 2\sqrt{d\log(d) T}
$$
which has a $\sqrt{d}$ factor compared to full information cases. since each iteration it reveals $1/d$ information compared with full information setting.

## proof

if EG is run with the unbiased estimator $\hat{z_t}$, then
$$
\begin{aligned}
    E[\text{Regret}(u)] &\le \frac{\log d}{\eta} + \eta \sum_{t=1}^T E \left[ \sum_{a=1}^dw_{t,a}\hat{z}_{t,a}^2 \right] \\
    & = \frac{\log d}{\eta} + \eta \sum_{t=1}^T E \left[ E[ \sum_{a=1}^dw_{t,a}\hat{z}_{t,a}^2]|w_t \right]
\end{aligned}
$$
where the last term
$$
E\left[ \sum_{a=1}^dw_{t,a}\hat{z}_{t,a}^2 |w_t \right]=\sum_{a=1}^dw_{t,a}^2(z_{t,a}^2/w_{t,a}^2)\le d
$$
we have
$$
E[\text{Regret}(u)] \le \frac{\log d}{\eta} + \eta \sum_{t=1}^T\sum_{a=1}^d E[z_{t,a}^2]
$$
we can complete the proof