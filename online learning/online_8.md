# Adversarial Bandits: OGD

In the online convex optimization setting, we are presented with a sequence of functions
$f_1,...,f_T$ . Importantly, we could compute subgradients $z_t \in \partial f_t(w_t)$, which allowed
us to update our weight vector using online gradient descent (OGD). In the bandit
setting, suppose we can only query function values of $f_t$. Can we still minimize regret?

## method

For a function $f$, define a smoothed version of $f$ as follows:
$$
\tilde{f}(w)=E_{v\sim U_{\le 1}}[f(w+\delta v)]
$$
where $U_{\le 1}$ is the uniform distribution over the **unit ball** ($||v||_2=1$). There are two important properties:

1. we can compute gradient of $\tilde{f}$:
    $$
    \nabla\tilde{f}(w)=E_{v\sim U_{\le 1}}[\frac{d}{\delta}f(w+\delta v)v]
    $$
    This is due to Stokes's theorem. It is easy to check this in the 1-D setting.

2. $\tilde{f}$ well approximate $f$
    $$
    |\tilde{f}(w)-f(w)|\le |E[f(w+\delta v)-f(w)]|\le L\delta
    $$
    where $L$ is the lipschitz constant of $f$

## algorithm (Bandit OGD)

1. for t=1,...,T
   1. set $w_t=\arg\min_{w\in S}||w-\eta\theta_t||_2$
   2. pick random variable $v_t\sim U_1$
   3. predict $\tilde{w_t}=w_t + \delta v_t$
   4. compute $z_t=\frac{d}{\delta}f_t(\tilde{w_t})v_t$
   5. update $\theta_{t+1}=\theta_t-z_t$

## regret

let $B=\max_{u\in S}||u||_2$ be the max magnitude of experts

let $F=\max_{u\in S,t \in [T]}f_t(u)$ be the max function value

The regret is
$$
E[\text{Regret}(u)]=E[\sum_{i=1}^T[f_t(\tilde{w_t})-f_t(u)]] \\
\le 3L\delta T+ \frac{B^2}{2\eta} + \frac{\eta}{2}Td^2(F/\delta + L)^2
$$
where expectation is taken over learner's random choice. By optimizing $\eta$ and $\delta$
$$
\eta = \frac{B}{d(F/\delta +L)\sqrt{T}}\\
\delta = \sqrt{\frac{BdF}{3L}}T^{-1/4}
$$
which yield
$$
E[\text{Regret}]\le \sqrt{12BdFLT^{3/4}+BdL\sqrt{T}}\\
=O(\sqrt{BdFL}T^{3/4})
$$
there is a $d$ since we only get $1/d$ information of the full gradient.

## proof
we have
$$
\sum_{t=1}^TE[\tilde{f_t}(w_t)-\tilde{f_t}(u)]\le \frac{1}{2\eta}||u||_2^2+\frac{\eta}{2}\sum_{t=1}^TE[||d/\delta f_t(\tilde{w_t})v_t||_2^2]
$$
we also have
$$
f_t(\tilde{w_t})\le f_t(w_t)+L\delta\le F+L\delta
$$
plugging this into inequality above:
$$
\sum_{t=1}^TE[\tilde{f_t}(w_t)-\tilde{f_t}(u)]\le \frac{B^2}{2\eta}+\frac{\eta}{2}\frac{d^2}{\delta^2}(F+L\delta)^2
$$
finally we use lipschitz property of $f$
$$
f_t(\tilde{w}_t)-f_t(u)\le f_t(w_t)-f_t(u)+L\delta \le \tilde{f_t}(w_t)-\tilde{f_t}(u)+3L\delta
$$
which completes the proof.