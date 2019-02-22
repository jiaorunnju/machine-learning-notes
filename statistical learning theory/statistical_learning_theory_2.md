# Statistical Learning Theory - 2

>This series of blogs are based on statistical learning notes of [CS229T](https://github.com/percyliang/cs229t)

In this blog, we will talk about concentration inequalities.

## Introduction

Concentration inequalities are powerful tools from probability theory which shows that a combination of independent random variables will concentrate around its expectation. In machine learning, recall from last post, the random variable here is the loss of a hypothesis on training set.

We mainly consider tail bounds here, which is in the form of:

$$
\mathbb{P}[|\hat{\mu}-\mu|\gt \epsilon] \leq SomeFunction(n, \epsilon) = \delta
$$

## Markov's Inequality

Let $Z \geq 0$ be a random variable, then:

$$
\mathbb{P}[Z \geq t] \leq \frac{\mathbb{E}[Z]}{t}
$$

Proof: the proof is simple

$$
\mathbb{I}(Z\geq t) \leq \frac{Z}{t}
$$

Taking the expectation on both sides completes the proof.

Note that the Chebyshev's inequality can be obtained by setting $Z=(X-\mu)^2$. By applying the Chebyshev's inequality to the average of $n$ i.i.d variables ($\hat{\mu}=\frac{1}{n}\sum_{i=1}^n X_i$), we have

$$
Var[\hat{\mu}]=\frac{1}{n} Var[X_1]
$$

This implies a weak concentration because the tail probability is decaying at polynomial rate ($\frac{1}{n}$). This is reasonable because the Chebyshev's inquality can be applied to any random variables, thus it is weak.

## Gaussian Variable

If we restrict the variable to be gaussian, can we get a more tight tail bound?

First, we will introduce the **Moment Generating Function**

>moment generating function (MGF): For a random variable $X$, the MGF of $X$ is :
$$
M_X(t)=E[e^{tX}]
$$

One way to think about the MGF is in terms of its Taylor expansion:

$$
M_X(t)=1+tE[X]+\frac{t^2}{2}E[X^2]+\frac{t^3}{6}E[X^3]+...
$$

Thus, to get the k-th moment, we just take the k-th derivative evaluated at $t=0$.

Here are some properties of MGF(easy to prove):

- If $X_1$ and $X_2$ are independent, then we have
  
  $$
  M_{X_1+X_2}(t)=M_{X_1}(t)M_{X_2}(t)
  $$

- Applying Markov's inequality to $Z=e^{tX}$, for all $t>0$, we get
  
  $$
  \mathbb{P}[X\geq \epsilon]\leq \frac{M_X(t)}{e^{t\epsilon}}
  $$

Applying the above properties to sample means, for all $t>0$, we get:

$$
\mathbb{P}[\hat{\mu} \geq \epsilon]\leq \left(\frac{M_{X_1}(t)}{e^{t\epsilon}}\right)^n
$$

So, for suitable t ($\frac{M_{X1}(t)}{e^{t\epsilon}} < 1$), the tail probability decays exponentially. Note that the mean of variable $X$ here needs to be 0 to ensure a suitable $t$ exists.

Now we study the gaussian variable.
>Let $X\sim N(0, \sigma^2)$, then $M_X(t)=\exp(\frac{\sigma^2 t^2}{2})$

Applying the inequality above, we get

$$
\mathbb{P}[X\geq \epsilon] \leq \inf_t \exp (\frac{\sigma^2 t^2}{2}-t\epsilon)=\exp\left(\frac{-\epsilon^2}{2\sigma^2}\right)
$$

The last step is done by setting $t=\frac{\epsilon}{\sigma^2}$

## Sub-Gaussian

A zero-mean random variable $X$ is sub-gaussian with parameter $\sigma^2$ if its MGF is bounded as below:

$$
M_X(t)\leq \exp\left(\frac{\sigma^2 t^2}{2}\right)
$$

Then its tail bound satisfies:

$$
\mathbb{P}[X\geq \epsilon]\leq \exp\left(\frac{-\epsilon^2}{2\sigma^2}\right)
$$

which is simple to prove.

The MGF of sub-gaussian variables has the following properties:

- If $X_1$ and $X_2$ are independent sub-gaussian variables with parameters $\sigma_1^2,\sigma_2^2$, then $X_1+X_2$ is a sub-gaussian variable with parameter $\sigma_1^2+\sigma_2^2$
- If $X$ is a sub-gaussian variable with parameter $\sigma^2$, $c>0$, then $cX$ is a sub-gaussian variable with parameter $c^2\sigma^2$

Here are some examples of sub-gaussian variables:

- gaussian variables, which is obvious
- bounded random variables (**Hoeffding's Lemma**): If $a<X<b$, then $X$ is sub-gaussian with parameter $\frac{(b-a)^2}{4}$

With the second example, we can get the famous Hoeffding's inequality:

Let $X_1,X_2,...,X_n$ be $n$ independent variables, each $X_i$ satisfies $a_i<X_i<b_i$, let $\hat{\mu}$ be the sample mean, we have

$$
\mathbb{P}[\hat{\mu}\geq E[\hat{\mu}]+\epsilon] \leq \exp\left(\frac{-2n^2\epsilon^2}{\sum_{i=1}^n (b_i-a_i)^2}\right)
$$

## Conclusion

Now, for sub-gaussian losses, such as zero-one loss used in classification, we can bound the tail well. In fact, Hoeffding's inequality is widely used in machine learning.

However, there are still some problems. In reality, we do not use zero-one loss because it is too hard to optimize. We use some convex loss functions instead, and to ensure a suitable gradient exists, most losses are unbounded. Also, in regression problems, losses are not bounded or even heavy-tailed. These make life harder.

There are some techniques to deal with non sub-gaussian variables, such as truncated estimator or median of mean. If you are interested in these topics, you can search for related papers.