# Statistical Learning Theory - 8

>This series of blogs are based on statistical learning notes of [CS229T](https://github.com/percyliang/cs229t)

We will talk about algorithmic stability in this post.

## Motivation

Consider the gap between the expected and empirical risk. It is easy to bound it with uniform convergence:

$$
\mathbb{P}[L(\hat{h})-\hat{L}(\hat{h})\geq \epsilon]\leq \mathbb{P}[\sup_{h\in H} L(h)-\hat{L}(h)\geq \epsilon]
$$

Analyzing the excess risk relies on the **ERM**, but the inequality above actually holds for all $h$. It is useful to think about $\hat{h}$ as a function of training examples:
$$
h=A(z_1,...,z_n)
$$
where $A$ is an algorithm.

Uniform convergence is focused on studying the "size" of $H$. What if a learning algorithm does not use all of $H$? For example, what if you use regularization via a penalty term, early stopping, or drop out? They could in principle can return any hypothesis from $H$, but also constrained by the algorithm. So we might expect better generalization in a algorithm-dependent way.

## Uniform Stability

Stability will be measured with respect to the difference in behaviour of an algorithm on a training set $S$ and a perturbed version $S'$.

- $S=(z_1,z_2,...,z_n)$ i.i.d samples
- $S^i=(z_1,...,z_i',...,z_n)$ with an i.i.d copy of the $i$-th sample
- $z_0$ is a new test example

### uniform stability

An algorithm $A:Z^n \rightarrow H$ has uniform stability $\beta$ with respect to loss $l$ if for all training set $S\in Z^n$, $S^i\in Z^n$ and $z_0$:

$$
|l(z_0, A(S))-l(z_0,A(S^i))|\leq \beta
$$

Note that this is a strong condition in that the bound must hold uniformly for all $S,S^i,z_0$

## Generalization Under Uniform Stability

Let $A$ be an algorithm with uniform stability $\beta$. Assume the loss is bounded:
$$
\sup_{z,h}|l(z,h)|\leq M
$$
Then with probability at least $1-\delta$:

$$
L(A(S))\leq \hat{L}(A(S))+\beta + (\beta n+M)\sqrt{\frac{2\log (1/\delta)}{n}}
$$

*proof*: Our goal is to bound $D(S)=L(A(S))-\hat{L}(A(S))$. The heart of the proof is McDiamid's inequality.

### Step 1: Bound the expectation of $D(S)$

By definition:

$$
E[D(S)]=E\left[ \frac{1}{n}\sum_{i=1}^n [l(z_0,A(S))-l(z_i,A(S))] \right]\\
= E\left[ \frac{1}{n}\sum_{i=1}^n [l(z_i',A(S))-l(z_i',A(S^i))] \right] \leq \beta
$$

Note that the second step above is by renaming the variables and that the independence between all $z_i$ or $z_i'$.

### Step 2: Show that $D(S)$ satisfy the bounded difference property

Let $\hat{L}^i$ denote the empirical expectation with respect to $S^i$, we have:

$$
|D(S)-D(S')|\\
=|L(A(S))-\hat{L}(A(S))-L(A(S^i))+\hat{L}^i(A(S^i))|\\
\leq |L(A(S))-L(A(S'))|+|\hat{L}(A(S))-\hat{L}^i(A(S^i))|\\
\leq |L(A(S))-L(A(S'))|+|\hat{L}(A(S))-\hat{L}(A(S^i))|+|\hat{L}(A(S^i))-\hat{L}^i(A(S^i))|\\
\leq 2\beta+ \frac{2M}{n}
$$

### Step 3: Apply McDiamid's inequality

Applying the McDiamid's inequality, we can complete the proof.

Note that for the bound to be not vacuous, we must have $\beta = o(\frac{1}{\sqrt{n}})$. Generally, we will have $\beta = O(\frac{1}{n})$, which guarantees a $O(\frac{1}{n})$ convergence rate.

With the help of stability, we can now analyzing ERM with regularization, since regularization improve the stability of the algorithm.