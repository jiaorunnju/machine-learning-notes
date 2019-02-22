# Statistical Learning Theory - 5

>This series of blogs are based on statistical learning notes of [CS229T](https://github.com/percyliang/cs229t)

In this blog, we will talk about another complexity measurement -- VC dimention. We will show that VC dimention is related to Rademacher complexity.

## Massart's Finite Lemma

- Through out this lemma, condition on $Z_1,...,Z_n$ (treat them as constant)
- Let $F$ be a finite set of functions
- Let $M^2$ be a bound on the second moment:
  
  $$
  \sup_{f\in F} \frac{1}{n} \sum_{i=1}^n f(Z_i)^2 \leq M^2
  $$
  
- Then the empirical Rademacher complexity is upper bounded:
  
  $$
  \hat{R_n}(F)\leq \sqrt{\frac{2M^2\log |F|}{n}}
  $$

We omit the proof here. This lemma gives an upper bound on the Rademacher complexity of a finite function class. Note that the complexity converge to 0 since it will be much more difficult to fit random labels when $n$ is larger.

## Finite Hypothesis Class

Combine the lemma above and the result in last post, we can get:

$$
L(\hat{h})-L(h^*)\leq \sqrt{\frac{32\log |H|}{n}} + \sqrt{\frac{2\log(2/\delta)}{n}}=O\left(\sqrt{\frac{\log(|H|)}{n}}\right)
$$

Since the empirical Rademacher complexity is bounded, then its expectation is bounded. We can get a result of the same order as before.

## Shattering Coefficient

But how to bound an infinite class of functions?

Massart's finite lemma gives a bound on the empirical Rademacher complexity, which only depends on data points. If an infinite class of functions $F$ that acts the same as a finite function class $F'$, then the two function classes have the same empirical Rademacher complexity.

All that matters as far as empirical Rademacher complexity is concerned is the behaviour of a function class on $n$ data points. This is the key.

Now we define shattering coefficient(growth function).

- Let $F$ be a family of function that map $Z to a finite set, e.g., {0, 1}
- The shattering coefficient is the maximum number of behaviours over $n$ points:
  
  $$
  s(F,n)=max_{z_1,...,z_n}|\{[f(z_1),...,f(z_n)]: f\in F\}|
  $$

If $F$ contains boolean functions, we have $M=1$ and:

$$
\hat{R_n}(F)\leq \sqrt{\frac{2\log s(F,n)}{n}}
$$

Now we can apply Massart's finite lemma to infinite function class with finite shattering coefficient. Note that we want $s(F,n)$ grow sub-exponentially with $n$; otherwise the Rademacher complexity will not go to 0, and we will not obtain uniform convergence.

We mainly talk about loss class above. For the hypothesis class, it's the same since there's a bijection between loss and hypothesis.

## VC Dimension

The VC dimension of a family of functions $H$ with boolean outputs is the maximum number of points it can shatter:

$$
VC(H)=\sup\{n:s(H,n)=2^n\}
$$

Intuition: the VC dimension is the maximun number of points which can be remembered by some $h\in H$.

To show that a class $H$ is with VC dimension $d$:

- show that no $d+1$ points can be shattered by $H$.
- there exists d points can be shattered by $H$.

## Sauer's Lemma

For a class $H$ be a class with VC dimension d. Then we have:

$$
s(H,n)\leq \sum_{i=0}^d  {n\choose i}
$$

when $n \leq d$: it's $2^n$;
when $n>d$: it's ${(\frac{en}{d})^d}$

*Sketch of proof*: use mathematical induction. When $n>d$ ($n\leq d$ is easy), for $n+1$ examples, we consider the label for the $(n+1)th$ sample.

- if $(n+1)th$ sample takes just one label, then the maximum number of labels is $s(H, n)$.
- if $(n+1)th$ sample takes two labels, the maximum number of assignments to labels is $2*s(H,n)$. However, half of them have appeared in condition 1, so remove them.

Overall, we can complete the induction.

Plug this into the result before, we get:

$$
\hat{R_n}(h) \leq \sqrt{\frac{2VC(H)(\log n+1)}{n}}
$$

hence we can get an error bound on VC dimension.