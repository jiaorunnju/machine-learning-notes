# conjugate gradient method

## problem

solving linear equation:
$$
Ax=b
$$
where $A$ is symmetric and positive-definite

## definition

### conjugate

A matrix $A$ is symmetric and positive-definite. If $u^TAv=0$, we say $u,v$ is conjugate according to $A$. In fact, it means that after a linear transform on $u,v$, they become orthogonal in the new space.

Let $P=\{p_1,p_2,...,p_n\}$ be a set of conjugate vectors with respect to $A$, we can prove they are linear independent, in other words, they are a basis of current space.

### intuition

Since $P$ is a basis, then $x^*$ can be represented by $x^*=\sum_{i=1}^n \alpha_i p_i$, thus
$$
Ax^*=\sum_{i=1}^n\alpha_iAp_i
$$
since when $k\neq i$, $p_k^TAp_i=0$, we have
$$
p_k^TAx^*=\alpha_kp_k^TAp_k
$$
which is
$$
\alpha_k=\frac{\langle p_k,b\rangle}{\langle p_k,p_k\rangle_A}
$$
then we just need to find $n$ conjugate vectors, and then compute $\alpha_k$

## conjugate gradient descent

let
$$
f(x)=\frac{1}{2}x^TAx-x^Tb
$$
set $p_0=b-Ax_0$, the negative gradient of $f(x)$ at $x_0$. It is also called residual at step 0.

Let $r_k$ be the residual at $k$-th step. To get a conjugate vector, we can set
$$
p_k = r_k - \sum_{i<k}\frac{pi^TAr_k}{pi^TAp_i}p_i
$$
(If you multiply $E$ on both side ($A=E^TE$), it is just Gram–Schmidt process in the target space. In fact, conjugate is just orthogonal in the target space. In the target space, the hessian is $I_n$, the gradient is the best direction to descent.)

Then
$$
x_{k+1}=x_k+\alpha_k p_k
$$
where
$$
\alpha_k=\frac{\langle p_k,b\rangle}{\langle p_k,p_k\rangle_A}
$$

## summary

The complexity of conjugate gradinet descent is:

- $n$ iterations most
- $n^2$ per-iteration

we may not need to compute all conjugate vectors. As long as the $r_k$ is small, we can return current result.

Pro.

- just like Newton's method, no line search is needed
- converges in a finite number of steps for quadratic problems
- do not need to compute the inverse