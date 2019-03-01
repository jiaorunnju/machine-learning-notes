# Statistical Learning Theory - 4

>This series of blogs are based on statistical learning notes of [CS229T](https://github.com/percyliang/cs229t)

In this blog, we will talk about getting generalization bound via uniform convergence and the Rademacher complexity.

## Uniform Convergence

In post 1, we show that in order to bound the expected risk $L$, we can take the following 2 steps:

- (**Convergence**) For a fixed $h$, show that $\hat{L}(h)$ is close to $L(h)$ with high probability.
- (**Uniform Convergence**) Show that the above holds simutaneously for all $h\in H$. In post 1, we use union bounds.

So why do we need uniform convergence? Because $\hat{L}(\hat{h})$ is not a sum of i.i.d samples. $\hat{h}$ is related to training examples, and we do not know which $h\in H$ we will get after training. So we need a uniform convergence. This is important.

With the fact that:

$$
L(\hat{h})-L(h^*)=[L(\hat{h})-\hat{L}(\hat{h})]+[\hat{L}(\hat{h})-\hat{L}(h^*)]+[\hat{L}(h^*)-L(h^*)]
$$

we can relate expected risk to empirical risk. Note that the second term above is non-positive because $\hat{h}$ is the empirical risk minimizer.

The first term is not a sum of i.i.d examples, so we use uniform convergence to solve it, while we can directly apply concentration inequalities to the last term because $h^{*}​$ is deterministic.

So, we can write the equation above as:

$$
\mathbb{P}[L(\hat{h})-L(h^*)\geq \epsilon] \leq \mathbb{P}[\sup_{h\in H} |L(h)-\hat{L}(h)|\geq \frac{\epsilon}{2}]
$$

because from the event in the first term, we can derive the event in the last term. Then what we need to do is to bound the right-hand-side above.

## Finite Hypothesis Class

Different from that in post 1, here wo do not assume a perfect $h\in H$ exists.

Let $H$ be the finite hypothesis class, and $l$ be the zero-one loss. $\hat{h}$ is the empirical risk minimizer.Then with probability at least $1-\delta$:

$$
L(\hat{h})-L(h^*)\leq \sqrt{\frac{2(\log(|H|)+\log(2/\delta))}{n}}=O\left(\sqrt{\frac{\log(|H|)}{n}}\right)
$$

*proof*: we prove this by control

$$
\sup_{h\in H} |L(h)-\hat{L}(h)|
$$

Because loss $l$ is bounded, by Hoeffding's inequality, for any fixed $h$ we have:

$$
\mathbb{P}[|L(h)-\hat{L}(h)|\geq \epsilon]\leq 2\exp(-2n\epsilon^2)
$$

Since $H$ is finite, then we just use union bound:

$$
\mathbb{P}[\sup_{h\in H} |L(h)-\hat{L}(h)|\geq \frac{\epsilon}{2}]\leq |H|\cdot 2\exp\left(-2n\left(\frac{\epsilon}{2}\right)^2\right)=\delta
$$

We can complete the proof.

Note that the bound now has a $\frac{1}{\sqrt{n}}$ rate because $H$ is not realizable.

## Rademacher Complexity

In the finite hypothesis setting, we can apply union bound to get uniform convergence. But we can not apply union bound to infinite hypothesis class. So we need a more sophisticated way to measure the complexity of hypothesis class. Here, we will derive Rademacher complexity.

### step 1

Let
$$
G_n=\sup_{h\in H}L(h)-\hat{L}(h)
$$

$G_n$ is a random variable that depends on $Z_1,...,Z_n$. We also define $G_n'$ for the negative loss $l'((x,y),h)=-l((x,y),h)$, so that

$$
\mathbb{P}[L(\hat{h})-L(h^*)\geq \epsilon] \leq \mathbb{P}[\sup_{h\in H} |L(h)-\hat{L}(h)|\geq \frac{\epsilon}{2}]\leq \mathbb{P}[G_n\geq \frac{\epsilon}{2}]+\mathbb{P}[G_n'\geq \frac{\epsilon}{2}]
$$

### step 2

Let $g$ be a function such that $G_n=g(Z_1,Z_2,...,Z_n)$. Note that $L$ is bounded, so there must exists a constant so that we can use McDiarmid's inequality.

It is not complex to show that:

$$
|\sup_{h\in H} [L(h)-\hat{L}(h)]-\sup_{h\in H} [L(h)-\hat{L}(h)+\frac{1}{n}(l(Z_i,h)-l(Z_i',h))]|\leq \frac{1}{n}
$$

The first term is exactly $g(Z_1,...,Z_i,...,Z_n)$ and the second term is $g(Z_1,...,Z_i',...,Z_n)$. Thus we can apply McDiarmid's inequality and get:

$$
\mathbb{P}[G_n\geq E[G_n]+\epsilon]\leq \exp(-2n\epsilon^2)
$$

### step 3

Now we need to bound $E[G_n]$, this is difficult since it contains $L(h)$. However, Given another set of training examples $(Z_1',...,Z_n')$ and define $\hat{L}'(h)$ to be the empirical loss on that set, we can have that:

$$
E[G_n]=E[\sup_{h\in H} E[\hat{L}'(h)]-\hat{L}(h)]
$$

Here we just use the fact that the expectation of empirical loss is the expected loss. Further more:

$$
E[G_n]=E[\sup_{h\in H} E[\hat{L}'(h)-\hat{L}(h)|Z_{1:n}]]
$$

Because $\sup$ is a convex function (assume that $H$ is a convex set), by applying Jensen's inequality to the right side above, we get:

$$
E[G_n]\leq E[E[\sup_{h\in H} \hat{L}'(h)-\hat{L}(h)|Z_{1:n}]] = E[\sup_{h\in H} \hat{L}'(h)-\hat{L}(h)]
$$

that is:

$$
E[G_n]\leq E[\sup_{h\in H} \frac{1}{n} \sum_{i=1}^n [l(Z_i',h)-l(Z_i,h)]]
$$

Then, we introduce i.i.d **Rademacher Variables** $\sigma_1,...,\sigma_n$ where $\sigma_i$ is uniform on $\\{-1,+1\\}$. Because $l(Z_i',h)-l(Z_i,h)$ is symmetric around 0, thus multiplying $\sigma_i$ does not change the distribution. So we have:

$$
E[G_n]\leq E[\sup_{h\in H} \frac{1}{n} \sum_{i=1}^n \sigma_i[l(Z_i',h)-l(Z_i,h)]]
$$

By the fact that $\sup_h [a_h-b_h]\leq \sup_h [a_h]+\sup_h [b_h]$ and linearity of expectation, we get:

$$
E[G_n]\leq 2E[\sup_{h\in H}\frac{1}{n}\sum_{i=1}^n \sigma_i l(Z_i,h)]
$$

The right-hand-side is almost the Rademacher complexity.

## Formal Definition

Let $F$ be a set of real-valued functions, define the Rademacher complexity of $F$:

$$
R_n(F)=E[\sup_{h\in H}\frac{1}{n}\sum_{i=1}^n \sigma_i f(Z_i)]
$$

where $Z_i$ are i.i.d samples and $\sigma_i$ are Rademacher variables.

The Rademacher complexity captures how well the best function in $F$ can fit random labels. A more power $F$ can fit better.

Define the empirical Rademacher complexity as:

$$
\hat{R_n}(F)=E[\sup_{h\in H}\frac{1}{n}\sum_{i=1}^n \sigma_i f(Z_i)|Z_{1:n}]
$$

Note that $R_n(F)=E[\hat{R_n}(F)]$

Now we can bound the excess risk: with probability at least $1-\delta$:

$$
L(\hat{h})-L(h^*)\leq 4R_n(F)+\sqrt{\frac{2\log(2/\delta))}{n}}
$$

where $F$ is the loss class, e.g., zero-one loss. The proof is straight forward, just bound $E[G_n]$ with $R_n(F)$.

## Some Properties

- $R_n(\{f\})=0​$
- $R_n(F_1)\leq R_n(F_2)$ if $F_1 \subseteq F_2$
- $R_n(F_1+F_2)=R_n(F_1)+R_n(F_2)$
- $R_n(c\cdot F)=\|c\|R_n(F)$
- $R_n(\phi \circ F)\leq c_{\phi}R_n(F)$, $c_{\phi}$ is the Lipschitz constant of $\phi$
- $R_n(ConvexHull(F))=R_n(F)$ for finite $F$