# Online-to-Batch Conversion

In this section, we will show that low regret implies low expected risk by explicitly
reducing the online setting to the batch setting.

## conversion

1. input $T$ training examples: $z_1, z_2,..., z_T$
2. iterate $t=1,2,...,T$:
   1. reveive $w_t$ from online learner
   2. send loss $f(z_t,w_t)$ to the learner
3. return the average of weight vector $\bar{w}=mean(w_t)$

## Risk

$$
E[L(\bar{w})]\le L(w^*)+ \frac{E[Regret(w^*)]}{T}
$$

proof:

due to convexity of $f$, we have
$$
L(\bar{w})\le \frac{1}{T}\sum_{t=1}^TL(w_t)
$$
which can also be written as:
$$
L(\bar{w})\le \frac{1}{T}\sum_{t=1}^T E[f_t(w_t)|w_t]
$$
add and subtract $L(w^*)$ to the RHS
$$
L(\bar{w})\le L(w^*)+\frac{1}{T}\sum_{t=1}^T (E[f_t(w_t)|w_t]-E[f_t(w^*)])
$$
then take expectations on both sides:
$$
E[L(\bar{w})]\le L(w^*)+\frac{1}{T}\sum_{t=1}^T E[f_t(w_t)-f_t(w^*)]
$$
so we have
$$
E[L(\bar{w})]\le L(w^*)+ \frac{E[Regret(w^*)]}{T}
$$
Note that we use an averaged vector