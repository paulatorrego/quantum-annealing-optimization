# 2. QUBO Mathematics

## 2.1 Definition

QUBO means **Quadratic Unconstrained Binary Optimization**:

$$
\min_{\mathbf{x}\in\{0,1\}^N}E(\mathbf{x}),
$$

with

$$
E(\mathbf{x})=
\sum_i a_i x_i+
\sum_{i<j}b_{ij}x_ix_j+c.
$$

The model is unconstrained because explicit constraints have been absorbed into the objective, usually through penalties.

## 2.2 Matrix form

A common representation is

$$
E(\mathbf{x})=\mathbf{x}^TQ\mathbf{x}+c.
$$

With a symmetric $Q$, one convention is

$$
E(\mathbf{x})=\sum_iQ_{ii}x_i+\sum_{i<j}Q_{ij}x_ix_j+c.
$$

Other conventions distribute a quadratic coefficient between $Q_{ij}$ and $Q_{ji}$. Code must document which convention it uses.

## 2.3 Why binary variables simplify quadratics

Since

$$
x_i^2=x_i,
$$

squared binary variables are already linear. Therefore the useful terms are constants, linear terms and pairwise products.

Higher-order terms such as

$$
x_ix_jx_k
$$

are not QUBO terms and require reduction, auxiliary variables or another formulation if a pure quadratic model is required.

## 2.4 Constant offsets

For any constant $c$,

$$
\operatorname*{arg\,min}_{\mathbf{x}}[E(\mathbf{x})+c]
=
\operatorname*{arg\,min}_{\mathbf{x}}E(\mathbf{x}).
$$

The optimizer is unchanged, although absolute energies are shifted.

## 2.5 Constraint penalties

For an equality

$$
g(\mathbf{x})=0,
$$

use

$$
E(\mathbf{x})=F(\mathbf{x})+P[g(\mathbf{x})]^2,
\qquad P>0.
$$

For example,

$$
x_1+x_2+x_3=1
$$

gives

$$
P(x_1+x_2+x_3-1)^2.
$$

Expanding and using $x_i^2=x_i$,

$$
P\left[
-x_1-x_2-x_3
+2x_1x_2+2x_1x_3+2x_2x_3+1
\right].
$$

Thus an equality constraint becomes a quadratic binary expression.

## 2.6 Example

Consider

$$
E=2x_1-3x_2+x_3+4x_1x_2-2x_1x_3+5x_2x_3.
$$

Its linear coefficients are

$$
a=(2,-3,1),
$$

and its pairwise coefficients are

$$
b_{12}=4,\quad b_{13}=-2,\quad b_{23}=5.
$$

There are only $2^3=8$ configurations, so exhaustive validation is immediate.

## 2.7 QUBO construction workflow

1. Define $x_i\in\{0,1\}$.
2. Write the original objective $F(\mathbf{x})$.
3. Write every constraint explicitly.
4. Add penalty terms.
5. Expand the expression.
6. Collect constants, linear terms and quadratic terms.
7. Build $Q$.
8. Validate every small configuration against the original problem.

## 2.8 Maximization

For

$$
\max F(\mathbf{x}),
$$

use

$$
\min[-F(\mathbf{x})].
$$

The sign must be tracked when interpreting energies.

## 2.9 Scaling

Multiplying by $\alpha>0$ gives

$$
E'(\mathbf{x})=\alpha E(\mathbf{x}),
$$

and preserves the minimizer:

$$
\operatorname*{arg\,min}E'=\operatorname*{arg\,min}E.
$$

However, hardware and numerical solvers have finite coefficient ranges, so scaling can affect practical performance.

## 2.10 BQM viewpoint

A Binary Quadratic Model can represent either binary variables $x_i\in\{0,1\}$ or spins $s_i\in\{-1,+1\}$. This abstraction lets the same optimization model be passed to multiple samplers.

The repository should therefore keep **model construction** separate from **solver execution**.

## 2.11 Validation

For small instances, verify

$$
E_{\mathrm{original}}(\mathbf{x})
=
E_{\mathrm{QUBO}}(\mathbf{x})
$$

or, when an offset is present,

$$
E_{\mathrm{original}}(\mathbf{x})
=
E_{\mathrm{QUBO}}(\mathbf{x})-C.
$$

Checking every state is stronger than checking only the optimum.

## 2.12 Summary

The core transformation is

$$
\boxed{
\text{problem}
\rightarrow
\text{binary encoding}
\rightarrow
\text{objective + penalties}
\rightarrow
\text{quadratic polynomial}
\rightarrow
\text{QUBO}
}.
$$

The essential skill is deriving the QUBO faithfully from the original problem.
