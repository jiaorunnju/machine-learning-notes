# Statistical Learning Theory - 7

>This series of blogs are based on statistical learning notes of [CS229T](https://github.com/percyliang/cs229t)

We mainly talk about covering numbers in this post.

Use the results in previous posts, we can measure the complexity of finite hypothesis class. For infinite hypothesis classes, we can use shattering coefficient as a good measure since all that matters is the behaviour of a function on a finite set of points. However, shattering coefficient only works for functions that return a finite number of values. Now, what can we use?

## Definitions

### metric space

- A metric space $(X, \rho)$ is defined over a set $F$ with a metric $\rho$
- A metric $\rho$: $X\times X\rightarrow R$, must be non-negative, symmetric, satisfy triangle inequality, and evaluate to 0 iff its arguments are equal.
- If $\rho(f,f')=0$ and $f\neq f'$, then we say $\rho$ is a pseudometric. In this post, we will be working with pseudometric.

Some examples of metrics $\rho$:

- Euclidean distance:
  - For real vectors $X=R^d$
  - metric $\rho$ is
  $$
  \rho(f,f')=\|f-f'\|_2
  $$
- $L_2(P_n)$
  - This is $L_2$ distance with respect to empirical distribution over n data points:
  $$
  P_n=\frac{1}{n} \sum_{i=1}^n \delta_{z_i}
  $$
  - Let $X$ be a family of functions mapping $Z$ to $R$
  - The metric is
  
  $$
  \rho(f,f')=\left( \frac{1}{n} \sum_{i=1}^n (f(z_i)-f'(z_i))^2 \right)^{1/2}
  $$
  - Since we are computing empirical deviation, then we can just view functions as $n$-dim vectors.

### Ball

Let $(X, \rho)$ be a metric space. Then the ball with radius $\epsilon >0$ centered at $f\in X$ is defined as:

$$
B_{\epsilon}(f)=\{ f'\in X: \rho(f,f')\leq \epsilon \}
$$

### Covering Number

The $\epsilon$ covering number of $F$ with respect to $\rho$ is:

$$
N(\epsilon, F, \rho)= \min \{ m: \exists\{ f_1,...,f_m \} \subseteq X, F \subseteq \cup_{j=1}^m B_{\epsilon}(f_j) \}
$$

Intuitively speaking, it is like dig holes of size $\epsilon$ on the groud. The covering number is the minimum number of holes.

## Discretization

Let $F$ be a family of functions mapping $Z$ to $[-1,1]$, the empirical Rademacher complexity of $F$ is bounded as following:

$$
\hat{R_n}(F)\leq \inf_{\epsilon>0}\left(
    \sqrt{\frac{2\log N(\epsilon, F, L_2(P_n))}{n}}+\epsilon
    \right)
$$

In fact the proof is very simple. We give a hint of proof here.

Let $C$ be an $\epsilon$-Covering of $F$. Since the output range of $F$ is bounded, output of $C$ is bounded. So for $C$, using Massart's finite lemma:

$$
\hat{R_n}(C)\leq \sqrt{\frac{2\log N(\epsilon, F, L_2(P_n))}{n}}
$$

Then, for any $f\in F$, the difference between $f$ and a function $c\in C$ is bounded by $\epsilon$, we can complete the proof.

## Conclusion

Note that there is a tradeoff here for $\epsilon$ since we take the $\inf$ over $\epsilon$. The Massart's finite lemma is very useful, all of the proofs we have mad are almost achieved by discretizing the hypothesis space, and bounding the empirical Rademacher complexity.