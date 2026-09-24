# 6. Constraints and Penalty Functions

## 6.1 Constrained optimization

A typical problem is

$$
\min_{\mathbf{x}}F(\mathbf{x})
$$

subject to

$$
g_k(\mathbf{x})=0,
\qquad
h_\ell(\mathbf{x})\leq0.
$$

A valid solution must be feasible and optimal within the feasible set.

## 6.2 Equality penalties

For

$$
g(\mathbf{x})=0,
$$

construct

$$
E(\mathbf{x})=F(\mathbf{x})+P[g(\mathbf{x})]^2,
\qquad P>0.
$$

The penalty is zero when the constraint is satisfied and positive when it is violated.

## 6.3 Exactly-one example

For

$$
x_1+x_2+x_3=1,
$$

use

$$
P(x_1+x_2+x_3-1)^2.
$$

Expanding and using $x_i^2=x_i$ gives

$$
P\left[
-x_1-x_2-x_3
+2x_1x_2+2x_1x_3+2x_2x_3+1
\right].
$$

This is a quadratic binary expression.

## 6.4 Choosing $P$

If $P$ is too small, an infeasible state can beat feasible states. If $P$ is very large, it can dominate the objective and create poor coefficient scaling.

For an infeasible configuration $\mathbf y$,

$$
E(\mathbf y)=F(\mathbf y)+P[g(\mathbf y)]^2.
$$

To make it worse than a known feasible optimum $\mathbf x^\star$,

$$
F(\mathbf y)+P[g(\mathbf y)]^2>F(\mathbf x^\star).
$$

Thus, when the numerator is positive,

$$
P>
\frac{F(\mathbf x^\star)-F(\mathbf y)}{[g(\mathbf y)]^2}.
$$

A sufficient global bound can be obtained by maximizing the right-hand side over relevant infeasible states. For large problems, where $F(\mathbf x^\star)$ may be unknown, analytical bounds, relaxations and empirical sweeps can be used.

## 6.5 Penalty sweeps

Study

$$
P\in\{P_1,P_2,\ldots,P_m\}
$$

and measure:

- feasibility rate;
- original objective;
- penalized energy;
- success probability;
- coefficient range.

The goal is a sufficient and well-scaled penalty, not simply the largest possible value.

## 6.6 Inequality constraints and slack variables

For

$$
\sum_iw_ix_i\leq C,
$$

introduce slack $s$:

$$
\sum_iw_ix_i+s=C.
$$

Represent the slack with binary variables,

$$
s=\sum_k a_ky_k,
\qquad y_k\in\{0,1\},
$$

and penalize the equality:

$$
P\left(\sum_iw_ix_i+\sum_ka_ky_k-C\right)^2.
$$

The encoding of $s$ depends on its allowed range.

## 6.7 Knapsack

The knapsack problem is

$$
\max\sum_iv_ix_i
$$

subject to

$$
\sum_iw_ix_i\leq C.
$$

Convert to minimization and use slack variables:

$$
E=-\sum_iv_ix_i
+P\left(\sum_iw_ix_i+\sum_ka_ky_k-C\right)^2.
$$

This illustrates the complete objective-plus-constraint-to-QUBO workflow.

## 6.8 Multiple constraints

For equality constraints $g_k(\mathbf x)=0$,

$$
E(\mathbf x)=F(\mathbf x)+\sum_kP_k[g_k(\mathbf x)]^2.
$$

Different constraints can require different penalty scales.

## 6.9 Feasibility-first post-processing

For every returned sample:

1. decode the binary variables;
2. check the original constraints;
3. mark feasible/infeasible;
4. evaluate the original objective;
5. evaluate the QUBO energy.

Do not identify the lowest QUBO energy with the best original solution without checking feasibility.

## 6.10 Hardware scaling

If objective coefficients are $O(1)$ and penalties are $O(10^4)$, the model can be mathematically valid but badly scaled for a finite coefficient range.

A practical pipeline is

$$
\text{formulation}
\rightarrow
\text{penalty selection}
\rightarrow
\text{coefficient scaling}
\rightarrow
\text{embedding}
\rightarrow
\text{sampling}.
$$

## 6.11 Common mistakes

- Choosing an arbitrary huge penalty.
- Checking only QUBO energy and not feasibility.
- Forgetting the sign change for maximization.
- Comparing differently scaled energies as though their magnitudes were directly comparable.
- Ignoring multiple optimal solutions.

## 6.12 Summary

Penalty methods implement

$$
\boxed{
\text{constrained optimization}
\rightarrow
\text{unconstrained QUBO}
}
$$

through terms such as

$$
P[g(\mathbf x)]^2.
$$

A good penalty is sufficiently strong to enforce feasibility while avoiding unnecessary domination of the original objective.
