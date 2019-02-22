# Statistical Learning Theory - 6

>This series of blogs are based on statistical learning notes of [CS229T](https://github.com/percyliang/cs229t)

We will talk about norm-constrained hypothesis classes in this post. Norm constraints are widely used in machine learning. However, according to our previous result, the constraints do not change the complexity, which is obviously wrong. We will analyze the Rademacher complexity for $L_2$ and $L_1$ norm.

## $L_2$ Norm

Let
$$
F=\{z\rightarrow w\cdot z: \|w\|_2\leq B_2\}
$$
And assume that
$$
E_{Z\sim p^*}[\|Z\|_2^2]\leq C_2^2
$$
Then we have:

$$
R_n(F)\leq \frac{B_2 C_2}{\sqrt{n}}
$$

*Proof*: By definition

$$
R_n(F)=\frac{1}{n}E\left[\sup_{\|w\|_2\leq B_2} \sum_{i=1}^n \sigma_i(w\cdot Z_i)\right]
$$

By Cauchy-Schwartz applied to $w$ and $\sum_{i=1}^n \sigma_i(w\cdot Z_i)$:

$$
\leq \frac{B_2}{n}E\left[ \|\sum_{i=1}^n \sigma_i Z_i \|_2 \right]
$$

By concavity of $\sqrt{\cdot}$,

$$
\leq \frac{B_2}{n} \sqrt{ E\left[ \|\sum_{i=1}^n \sigma_i Z_i \|_2^2 \right]}
$$

Distribute the sum, the **expectation of cross is zero** since $\sigma_i$ is independent.

$$
= \frac{B_2}{n} \sqrt{ E\left[ \sum_{i=1}^n \|\sigma_i Z_i\|_2^2 \right]}
$$

since $\sigma_i=1$ or $-1$, we have:

$$
= \frac{B_2}{n} \sqrt{ E\left[ \sum_{i=1}^n \| Z_i\|_2^2 \right]}
$$

Then with the bound on $Z_i$, we get:

$$
\leq \frac{B_2}{n}\sqrt{nC_2^2}
$$

which completes the proof.

We can see that the constraint $B$ does constrain the model complexity.

## $L_1$ Norm

Assume that
$$
\|Z_i\|_{\infty}\leq C_{\infty}
$$
Then

$$
R_n(F)\leq \frac{B_1C_{\infty}\sqrt{2\log(2d)}}{\sqrt{n}}
$$

*proof*: The key of proof is to realize that the $L_1$ ball
$$
\{ w: \|w\|_1\leq B_1\}
$$
is the convex hull of the following $2d$ vectors:

$$
W=\cup_{j=1}^d \{B_1e_j, -B_1e_j\}
$$

Since the Rademacher complexity of a class is the same as its convex hull, we just analyze $W$.

$$
R_n=E\left[ \sup_{w\in W} \frac{1}{n} \sum_{i=1}^n \sigma_i(w\cdot Z_i) \right]
$$

Apply Holder's inequality, we have
$$
w\cdot Z_i \leq \|w\|_1\|Z_i\|_{\infty} \leq B_1C_{\infty}
$$

Apply Massart's finite lemma:

$$
R_n(F)\leq \frac{B_1C_{\infty}\sqrt{2\log(2d)}}{\sqrt{n}}
$$

## Remarks

- as $p$ increase, $p$-norm decrease, thus with the same radius, size of ball increase
- thus $L_1$ constraint is more stronger than $L_2$, at the expense of $\log d$
- In fact, $B_1$ bound the number of useful features. Assume we have $s$ useful features. When $s\ll d$, $L_1$ is much more efficient, else use $L_2$ ($B_2=\sqrt{s},C_2=\sqrt{d}$).
- All the bounds above either do not depend on $d$ ($L_2$) or slightly depend on $d$ ($L_1$). **No matter how many features you have, if you regularize well, it will generalize well**.

## Conclusion

In this post, we show that why norm constraints work from a generalization perspective. In addition to convex-relax explanation and graph explanation, this provide another view of norm constraints.