# Statistical Learning Theory - 9

>This series of blogs are based on statistical learning notes of [CS229T](https://github.com/percyliang/cs229t)

In this post, we present a summary of posts before.

## Summary

### Goals

The main focus of this series of posts is to study the **excess risk**
$$
L(\hat{h})-L(h^*)
$$
of the **empirical risk minimizer** $\hat{h}$. In paticular, we want that the probability at least $1-\delta$, the excess risk is upper bounded by something that depends on the complexity of the learning problem and $n$, the number of i.i.d training samples. (Another goal is to bound the gap between empirical risk and expected risk)

The excess risk of often within a factor of two of the difference between empirical and expected risk:
$$
\sup_{h\in H}|L(h)-\hat{L}(h)|
$$
which suggests using uniform convergence tools to bound it.

### Results on excess risk

- Realizable, finite hypothesis class:
  $$
  O(\frac{\log |H|}{n})
  $$
- Finite hypothesis class:
  $$
  O(\sqrt{\frac{\log |H|}{n}})
  $$
- Shattering coefficient and VC-dimention:
  $$
  O(\sqrt{\frac{\log s(H, n)}{n}})=O(\sqrt{\frac{VC(H)\log n}{n}})
  $$
- $L_2$ norm constrained-kernels:
  $$
  O(\frac{B_2 C_2}{\sqrt{n}})
  $$
- $L_1$ norm constrained-sparsity:
  $$
  O(\frac{B_1 C_{\infty}\sqrt{\log d}}{\sqrt{n}})
  $$
- Non-decreasing functions (via covering number):
  $$
  O(\sqrt{\frac{\log n}{n}})
  $$

### Technical tools

#### Tail bounds

- How much do random variables deviate from their mean? We generally look for sharp concentration bounds, which means that the probability of deviation by a constant decays **exponentially** fast as a function of $n$:
  $$
  P[G_n-E[G_n]\geq \epsilon]\leq c^{-n\epsilon^2}
  $$
- When $G_n$ is an average of i.i.d sub-gaussian variables $Z_i$, we can bound the moment generation function and get the desired bound (**Hoeffding's inequality** for bounded variables). Sub-gaussian variables include gaussian and bounded random variables, but not Laplace random variables, which has heavy tails.
- When $G_n$ is the result of applying a function with bounded difference to i.i.d variables, then **McDiamid's inequality** gives us the same bound.

#### Complexity control

- For a single hypothesis, we can directly apply the tail bound. However, we seek for **uniform convergence** for all $h\in H$
- Finite hypothesis class: just the number of hypothesis
- For infinite hypothesis class, the key intuition is that the complexity of $H$ is described by **how it acts on $n$ data points**. Formally, symmetrization (introduce ghost dataset and Rademacher variables) reveals this.
- The complexity is the **shattering coefficient** $s(H, n)$ (technically of the loss class $A$). By **Sauer's lemma**, the shattering coefficient can be upper bounded using **VC-dimension** $VC(H)$.
- Rademacher complexity $R_n(H)$ measures how well $H$ can fit random binary labelings. It is nice because of the numerous compositional properties (convex-hull, Lipschitz composition, etc.)
  - By **Massart's finite lemma**, we can relate $R_n(H)$ to shattering coefficient.
  - For $L_2$ norm constrained linear functions, we can use linearity and Cauchy-Schewartz to analyze kernels.
  - For $L_1$ norm constrained linear functions, we can use the fact that the $L_1$ polytope is really simple as it only has $2d$ vertices.

### Other paradigms

We also introduce **algorithmic stability**. Let $A$ be an algorithm that maps data to hypothesis. $A$ could be **ERM** or not.

Typical uniform convergence bounds yield results that depend on the complexity of the entire hypothesis class:

$$
L(A(S))-\hat{L}(A(S))\leq SomeFunction(H)
$$

Algorithmic stability allows us to obtain a bound that depends on properties of algorithm $A$ (i.e., its stability $\beta$) rather than $H$:

$$
L(A(S))-\hat{L}(A(S))\leq SomeFunction(A)
$$

Of course, this just bounds the gap between the expected and empirical risk, not the excess risk, which needs uniform convergence.
