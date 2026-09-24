# 1. Binary Optimization

## 1.1 From optimization to binary decisions

Many combinatorial problems consist of yes/no decisions. Represent each decision with

$$
x_i\in\{0,1\}.
$$

For $N$ variables,

$$
\mathbf{x}=(x_1,\ldots,x_N)\in\{0,1\}^N,
$$

so there are

$$
2^N
$$

possible configurations.

A minimization problem is

$$
\mathbf{x}^\star=\operatorname*{arg\,min}_{\mathbf{x}\in\{0,1\}^N}f(\mathbf{x}).
$$

For maximization,

$$
\mathbf{x}^\star=\operatorname*{arg\,max}_{\mathbf{x}\in\{0,1\}^N}f(\mathbf{x}),
$$

or equivalently minimize $-f$.

## 1.2 Linear and quadratic objectives

A linear binary objective is

$$
f(\mathbf{x})=\sum_i a_i x_i+c.
$$

Pairwise interactions give

$$
f(\mathbf{x})=
\sum_i a_i x_i+
\sum_{i<j}b_{ij}x_ix_j+c.
$$

The product $x_ix_j$ is active only when both decisions are selected, so it can represent interactions, incompatibilities, rewards, graph edges and logical penalties.

Because $x_i\in\{0,1\}$,

$$
x_i^2=x_i.
$$

Thus diagonal quadratic terms reduce to linear terms.

## 1.3 Constraints and feasibility

A constrained problem is

$$
\min_{\mathbf{x}}F(\mathbf{x})
$$

subject to

$$
g_k(\mathbf{x})=0,
\qquad
h_\ell(\mathbf{x})\leq0.
$$

Its feasible set is

$$
\mathcal F=\{\mathbf{x}:g_k(\mathbf{x})=0,\ h_\ell(\mathbf{x})\leq0\}.
$$

The solution must optimize the objective **within** $\mathcal F$. A low-energy infeasible configuration is not a valid solution to the original problem.

## 1.4 Encoding decisions

The encoding should be explicit. For selection,

$$
x_i=1\Rightarrow\text{item }i\text{ selected},
\qquad
x_i=0\Rightarrow\text{item }i\text{ not selected}.
$$

For a binary graph partition,

$$
x_i=0\Rightarrow i\in A,
\qquad
x_i=1\Rightarrow i\in B.
$$

A good encoding makes the original problem easy to recover from a sample.

## 1.5 Example: incompatibility

If two decisions cannot both be selected, add

$$
P x_1x_2,
\qquad P>0.
$$

The forbidden state $(1,1)$ receives an extra energy $P$.

## 1.6 Why binary optimization is useful for annealing

The central modeling chain is

$$
\text{real problem}
\rightarrow
\text{binary variables}
\rightarrow
\text{quadratic objective}
\rightarrow
\text{QUBO/Ising}
\rightarrow
\text{annealing problem}.
$$

The main challenge is often the formulation, not the final sampler call.

## 1.7 Exact search as a validation tool

Brute force checks all $2^N$ states. For example,

$$
2^{20}\approx10^6,
\qquad
2^{40}\approx10^{12}.
$$

This makes exact search impractical for large $N$, but extremely valuable for testing small models.

A robust workflow is

$$
\text{exact small instances}
\rightarrow
\text{validate formulation}
\rightarrow
\text{approximate larger instances}.
$$

## 1.8 Key ideas

- Binary variables encode discrete decisions.
- Quadratic terms encode pairwise interactions.
- Constraints define feasibility.
- QUBO absorbs constraints into the energy.
- Exact enumeration is the strongest small-instance validation tool.
