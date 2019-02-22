# Statistical Learning Theory - 3

>This series of blogs are based on statistical learning notes of [CS229T](https://github.com/percyliang/cs229t)

In this blog, we will continue topics on concentration inequalities.

## McDiarmid's Inequality

McDiarmid's inequality is a generalization of Hoeffding's inequality, where we want to bound not the average of random variable $X_1,...,X_n$, but any function on $X_1,...,X_n$ satisfying an appropriate bounded differences condition.

Let $f$ be a function satisfying the following bounded differences condition:

$$
|f(x_1,...,x_i,...,x_n)-f(x_1,...,x_i',...,x_n)| \leq c_i
$$

for all $i$ and $x_i$ (modifying one coordinate does not change $f$ too much). And let $X_1,...,X_n$ be independent random variables.Then we have:

$$
\mathbb{P}[f(X_1,...,X_n) \geq E[f(X_1,...,X_n)]+\epsilon] \leq \exp\left(\frac{-2\epsilon^2}{\sum_{i=1}^n c_i^2}\right)
$$

If we make $f(x_1,...,x_n)=\frac{1}{n}\sum_{i=1}^n x_i$, and $a_i\leq x_i \leq b_i$, then we get the Hoeffding's inequality. Note that $f$ can be rather complex here, as long as it satisfies the bounded differences condition.

## Proof of McDiarmid's Inequality

The proof is complex. We will first introduce martingale, then use it to prove the inequality.

### Martingale

(discrete-time) Martingale is a sequence of random variables $X_1,...,X_n,...$ that satisfies for any $n$:

- $E[X_n] < \infty$
- $E[X_{n+1}\|X_1,...,X_n]=X_n$

which says that given all prior states, the expectation of next state is similar to the most recent state (can not predict future).

More generally, a sequence of random variables $Z_0,Z_1,...,Z_n$ is a martingale sequence with respect to another sequence of random variables $X_1,...,X_n$ iff $Z_i$ is a function of $X_{1:i}$, $E[\|Z_i\|]<\infty$, and $E[Z_i\|X_{1:i-1}]=Z_{i-1}$

Define $D_i=Z_i-Z_{i-1}$, we call $D_{1:n}$ a martingale difference sequence with respect to $X_{1:n}$, another way to write is $E[D_i\|X_{1:i-1}]=0$. We can view the sequence of $D_i$ as a generalization of independent variables. An example of martingale is random walk with the same probability and step-size on all directions.

Note that here we do not need $X_{1:n}$ to be i.i.d.

### Sub-Gaussian Martingale

Let $Z_0,Z_1,...,Z_n$ be a martingale sequence with respect to $X_1,...,X_n$, and Suppose that each $D_i=Z_i-Z_{i-1}$ is conditionally sub-gaussian with parameter $\sigma^2$, that is

$$
E[e^{tD_i}|X_{1:i-1}]\leq exp(\sigma_i^2 t^2/2)
$$

Then, $Z_n-Z_0=\sum_{i=1}^n D_i$ is sub-gaussian with parameter $\sigma^2=\sum_{i=1}^n \sigma_i^2$

*proof*: we can prove by induction on $n$:

$$
E[\exp(t(Z_n-Z_0))] =E[\exp(tD_n)\exp(t(Z_{n-1}-Z_0))]
$$

By conditioning on $X_{n-1}$, we get

$$
original = E[E[\exp(tD_n)\exp(t(Z_{n-1}-Z_0))|X_{n-1}]] \leq \exp(\sigma_n^2 t^2/2)E[\exp(t(Z_{n-1}-Z_0))]
$$

Follow this, and we can complete the proof. The key is that given $X_{1:n-1}$, $D_i$ is sub-gaussian.

### Proof

We construct a particular type of martingale called **Doob martingale**:

$$
Z_i=E[f(X_1,X_2,...,X_n)|X_{1:i}]
$$

The extremes are: $Z_0=E[f(X_1,...,X_n)]$ and $Z_n=f(X_1,...,X_n)$. We can think of this martingale as exposing more info about $f$ over time.

Our goal is to show that we have a sub-gaussian martingale. We want to prove that given $X_{1:i-1}$, $D_i$ is bounded, so it is sub-gaussian.

$D_i=Z_i-Z_{i-1}$, where

$$
Z_i=E[f(X_1,X_2,...,X_n)|X_{1:i}]
$$

$$
Z_{i-1}=E[f(X_1,X_2,...,X_n)|X_{1:i-1}]
$$

The difference is how they deal with $X_i$.

Let

$$
L_i = \inf_x E[f(X_{1:n})|X_{1:i-1},X_i=x]-Z_{i-1}
$$

$$
U_i = \sup_x E[f(X_{1:n})|X_{1:i-1},X_i=x]-Z_{i-1}
$$

Note that $L_i \leq D_i \leq U_i$

Let $x_L$ and $x_U$ correspond to the x's achieving $L_i$ and $U_i$, then

$$
f(X_{1:i-1},x_L,X_{i+1:n})-f(X_{1:i-1},x_U,X_{i+1:n})\leq c_i
$$

Then

$$
U_i-L_i=E[f(X_{1:i-1},x_U,X_{i+1:n})|X_{1:i-1}]-E[f(X_{1:i-1},x_L,X_{i+1:n})|X_{1:i-1}]
$$

because all the $X_i$ are independent, so the distribution of $X_{i+1:n}$ is the same for both $X_i=x_L$ and $X_i=x_U$. Then we have:

$$
U_i-L_i=E[f(X_{1:i-1},x_L,X_{i+1:n})-f(X_{1:i-1},x_U,X_{i+1:n})|X_{1:i-1}] \leq c_i
$$

the expectation is taken over $X_{i+1:n}$. Thus we prove that $D_i$ is sub-gaussian with parameter $\frac{c_i^2}{4}$ conditioned on $X_{1:i-1}$.

Use the properties of sub-gaussian variables, we can complete the proof.