# Statistical Learning Theory - 1

>This series of blogs are based on statistical learning notes of [CS229T](https://github.com/percyliang/cs229t)

In this blog, we mainly talk about statistical learning theory, which describes how a learning algorithm generalize and how to derive a generalization error bound for an algorithm. This is important because it shows why statistical learning is useful under i.i.d assumption.

## Introduction

The central question in statistical learning is:
> Why does minimizing training error reduce test error?

The answer is not obvious for the training error and testing error are two separate quantities which can in general be arbitrarily far apart. To answer this question, we will use a set of new tools. At the heart of these tools are concentration inequalities and union bound. We will talk about these tools below.

## Formal Setup

In this post, we mainly talk about supervised learning setting. The problem is predicting an output $y\in Y$ given $x\in X$, for example, $X=R^d,Y=\{-1,1\}​$.

- Let $H$ be a set of hypotheses, each $h\in H$ maps $X$ to $Y$, for example, $H=\{ x\to sign(w^Tx), w\in R^d \}$.

- Let $l:(X\times Y)\times H \to R$ be a loss function, for example, $l((x,y),h)=\mathbb{I}[y\neq h(x)]$ is the zero-one loss.

- Let $p^*$ be the underlying distribution of the input-output pair $X\times Y$.

Several definitions below:

- Expected risk
  
  $$
  L(h)=E_{(x,y)\sim p^*}[l((x,y),h)]
  $$

  The expected risk is just an expectation of $h's$ loss with respect to $l$ over distribution $p^*$. Our goal is to get the best $h^*$ which satisfies:

  $$
  h^*=\arg min_{h\in H}L(h)
  $$

  Note that $h^*$ is not a random variable.
- Training example
  Training examples are a set of input-output pairs:

  $$
  (x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),...,(x^{(n)},y^{(n)})
  $$

  which are drawn i.i.d from $p^*$. Note that the training distribution and testing distribution are the same, and this is the source of statistial learning's power.
- Empirical risk
  Empirical risk

  $$
  \hat{L}(h)=\frac{1}{n}\sum_{i=1}^n l((x^{(i)},y^{(i)}),h)
  $$

  This is an unbiased estimation of expected risk. Note that empirical risk itself is a random variable that depends on training examples, so we can use many kinds of concentration inequalities to bound the range of it. By training, we mean to find an expirical risk minimizer (**ERM**):

  $$
  \hat{h}=\arg min_{h\in H} \hat{L}(h)
  $$

We want to get $h^*$, but $L(h)$ is difficult to solve because we do not know the distribution $p^*$. By ERM, our interest is the expected risk of $\hat{h}$, in other words, $L(\hat{h})$.

We are interested in the following two quantities:

- How does the expected and empirical risks compare for the ERM?
  
  $$
  L(\hat{h})-\hat{L}(\hat{h})
  $$

- How will the ERM doing with respect to $h^*$?
  
  $$
  L(\hat{h})-L(h^*)
  $$

  This is also known as **excess risk**

Because $L(\hat{h})$ is a random variable, so we can not bound that deterministically. However, we can give a high probability bound, e.g.

$$
\mathbb{P}[L(\hat{h})-L(h^*) \gt \epsilon] \leq \delta
$$

In fact, this is the general framework we use in analyzing statistical learning algorithms. Now we introduce the **Probably Approximately Correct(PAC)** framework (*Leslie Valient, 1984*).

>A learning algorithm $A$ PAC learns $H$ if for any distribution $p$ over $X\times Y$, $\epsilon > 0$, $\delta > 0$, $A$ returns $\hat{h}$ such that with probability at least $1-\delta$, $L(\hat{h})-L(h^*)\leq \epsilon$, and $A$ runs in $poly(n, size(x),\frac{1}{\epsilon}, \frac{1}{\delta})$ time.

Note that we do not consider the optimization here, in other words, we assume that the loss can be optimized efficently, e.g. convex optimization problems.

## Realizable Finite Hypothesis Class

At the end of this post, we give a simple example of analyze the excess risk. There are two assumptions here:

- $H$ is finite
- there is a $h^*\in H$ with 0 expected risk (this is what the word **realizable** in title means),
  
  $$
  L(h^*)=E_{(x,y)\sim p^*}[l((x,y),h^*)]=0
  $$

Let $l$ be zero-one loss, and $\hat{h}$ be the empirical risk minimizer. Given n training examples, then with probability $1-\delta$,

$$
L(\hat{h}) \leq \frac{\log{|H|}+\log{\frac{1}{\delta}}}{n}
$$

*proof :* what we want to do is to bound the probability of getting a bad(which means perform badly on whole dataset) $\hat{h}$, because we use ERM, so $\hat{h}$ is perfect on training set, which means

$$
\hat{L}(\hat{h})=0
$$

Let the set of all bad hypothesis is $B$

- Step 1
  
  For any bad $h$ with $L(h)>\epsilon$, the probability that $h$ is perfect on training set satisfies:

  $$
  \mathbb{P}[\hat{L}(h)=0]=(1-L(h))^n \leq (1-\epsilon)^n \leq e^{-\epsilon n}
  $$

  where the last step uses $1+x\leq e^x$

- Step 2
  
  Because we do not know which hypothesis we get after learning, here we use a union bound to bound the probability.
  >Union bound: $P(A\cup B)\leq P(A)+P(B)$

  Let $h'$ be the hypothesis we get after learning, then

  $$
  \mathbb{P}[\hat{L}(h')=0] \leq \mathbb{P}[\exists h\in B, \hat{L}(h)=0] \leq \sum_{h\in B} \mathbb{P}[\hat{L}(h)=0]
  $$

  The last term satisfies:

  $$
  \sum_{h\in B} \mathbb{P}[\hat{L}(h)=0] \leq |B|e^{-\epsilon n} \leq |H|e^{-\epsilon n}
  $$

  let the last term be $\delta$, we have:

  $$
  \epsilon = \frac{\log{|H|}+\log{\frac{1}{\delta}}}{n}
  $$

  which completes the proof.

## Conclusion

In this post, we give a simple introduction of statistical learning theory, mainly focused on $PAC$ learning. This is useful. Take the last equation as an example, we can get the following tips:

- If we want to get better performance, e.g., let $\epsilon$ be smaller, we can either increase $n$ or restrict $H$.
- More complex $H$ needs more data! This is why data is more important in the days of deep learning.
- However, restricting $H$ also restrict the search area of hypothesis, which is bad unless you know the underlying true hypothesis. But, who knows that? It is a trade off to be made when designing algorithms.

In the future, we will talk more advanced topics.
