# BFGS algorithm

## motivation

The cost to compute the inverse of Newton's method is huge, we want to find an
approximation of hessian $H$.

## BFGS

let
$$
s_n = x_n-x_{n-1}\\
y_n = g_n-g_{n-1}
$$
where $g_n$ is the gradient. Then we have
$$
H_ns_n=y_n \\
or\\
H_n^{-1}y_n=s_n
$$
So the BFGS method find an approximation that satisfies the above condition:
$$
\min_{H^{-1}}||H^{-1}-H_{n-1}^{-1}||^2 \\
\begin{aligned}
    \text{s.t. } & H_n^{-1}y_n=s_n\\
    & H^{-1} \text{ is symmetric}
\end{aligned}
$$
The solution of the problem above is:
$$
H_{n+1}^{-1}=(I-\rho_ny_ns_n^T)H_n^{-1}(I-\rho_ns_ny_n^T)+ \rho_ns_ns_n^T
$$
where $\rho_n=(y_n^Ts_n)^{-1}$

Then we can get an algorithm to compute $H_n^{-1}d$ for a direction $d$:

1. $r\leftarrow d$
2. //compute right product
3. for i=n...1
   1. $\alpha_i\leftarrow\rho_is_i^Tr$
   2. $r\leftarrow r-\alpha_iy_i$
4. //compute center
5. $r\leftarrow H_0^{-1}r$
6. //compute left product
7. for i=1..n
   1. $\beta\leftarrow \rho_iy_i^Tr$
   2. $r\leftarrow r+ (\alpha_{n-i+1}-\beta)s_i$
8. return r

which just follow the recusive definition of $H_n$

## L-BFGS

Only store the recent $m$ $s_n$ and $y_n$, which can reduce memory usage.

## complexity

$O(n^2)$ per-iteration.

converge rate: linear to quadratic convergence.