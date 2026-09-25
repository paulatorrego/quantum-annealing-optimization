# Quantum Annealing Optimization

A computational framework for **combinatorial optimization using QUBO formulations, Ising models, classical annealing, and quantum annealing**.

This repository develops a common mathematical and computational workflow for transforming discrete optimization problems into quadratic binary models and solving them with different optimization methods. The emphasis is on the connection between **combinatorial optimization, mathematical modeling, statistical mechanics, and quantum computing**, while keeping the implementations reusable and directly comparable.

The repository contains explicit QUBO formulations for several representative optimization problems, classical reference solvers, optional D-Wave implementations, benchmarking utilities, and visualization tools.

---

## 1. From combinatorial optimization to QUBO

Many discrete optimization problems can be written as

\[
\min_{x \in \mathcal{X}} f(x),
\]

where the variables describe discrete decisions and \(\mathcal{X}\) contains the allowed configurations.

For binary optimization,

\[
x_i \in \{0,1\},
\]

a particularly important model is the **Quadratic Unconstrained Binary Optimization (QUBO)** problem:

\[
\boxed{
E(x)=
c+
\sum_i a_i x_i
+
\sum_{i<j} b_{ij}x_i x_j
}
\]

or, equivalently,

\[
\boxed{
E(x)=x^TQx+c
}
\]

for an appropriate matrix convention.

The objective is to find

\[
x^\star=\arg\min_{x\in\{0,1\}^N}E(x).
\]

QUBOs are important because a wide range of combinatorial problems can be represented in this form, either directly or by introducing additional binary variables and penalty terms.

The resulting model contains:

- **linear terms** $a_i x_i$, representing individual decisions;
- **quadratic interactions** $b_{ij}x_ix_j$, representing pairwise relationships;
- an optional **constant offset** $c$, which does not affect the location of the optimum.

The implementation in this repository uses the explicit convention

\[
E(x)
=
c+
\sum_i a_i x_i+
\sum_{i<j}b_{ij}x_ix_j,
\]

with each interaction stored only once. This avoids ambiguities associated with factors of two in matrix representations.

---

## 2. Constraints and penalty functions

Real optimization problems are generally constrained:

\[
\min_x f(x)
\]

subject to

\[
g_k(x)=0,
\qquad
h_l(x)\leq 0.
\]

A QUBO is unconstrained, so constraints must be incorporated into the energy function.

For an equality constraint

\[
\sum_i a_i x_i=b,
\]

a quadratic penalty can be introduced:

\[
\boxed{
P\left(\sum_i a_i x_i-b\right)^2
}
\]

where \(P>0\) controls the energetic cost of violating the constraint.

The resulting objective becomes

\[
E_{\mathrm{QUBO}}(x)=
f(x)
+
P\left(\sum_i a_i x_i-b\right)^2.
\]

For sufficiently appropriate penalty strength, feasible configurations are energetically preferred while the original objective determines which feasible configuration is selected.

Inequality constraints can similarly be converted into equality constraints by introducing binary slack variables.

The repository provides reusable penalty constructions rather than implementing these transformations independently for every problem.

---

## 3. QUBO and the Ising model

The QUBO representation has a direct connection to the Ising model used in statistical mechanics and quantum annealing.

The binary variables can be transformed into spin variables through

\[
\boxed{
s_i=2x_i-1
}
\]

with

\[
s_i\in\{-1,+1\},
\qquad
x_i=\frac{1+s_i}{2}.
\]

Using

\[
x_i x_j
=
\frac{1+s_i+s_j+s_is_j}{4},
\]

the QUBO energy can be rewritten as

\[
\boxed{
H(s)=
c'
+
\sum_i h_i s_i
+
\sum_{i<j}J_{ij}s_i s_j.
}
\]

Here:

- $h_i$ are local fields;
- $J_{ij}$ are pairwise spin couplings;
- $c'$ is a constant energy offset.

The two formulations describe the same optimization landscape under the binary-spin transformation.

This repository therefore includes explicit and reversible

\[
\mathrm{QUBO}
\longleftrightarrow
\mathrm{Ising}
\]

conversion utilities, together with exhaustive validation routines for small systems.

---

## 4. Annealing as an optimization strategy

Once an optimization problem has been expressed as an energy function, annealing methods can be used to search its landscape.

### Simulated annealing

Classical simulated annealing introduces a temperature parameter \(T\). A move that changes the energy by

\[
\Delta E=E_{\mathrm{new}}-E_{\mathrm{old}}
\]

is accepted with probability

\[
P_{\mathrm{accept}}=
\begin{cases}
1, & \Delta E\leq0,\\[4pt]
e^{-\Delta E/T}, & \Delta E>0.
\end{cases}
\]

At high temperature, energetically unfavorable transitions can occur frequently, allowing exploration of the landscape. As the temperature decreases, the dynamics increasingly favor low-energy configurations.

The repository includes a self-contained simulated-annealing implementation so that QUBO instances can be studied without requiring quantum hardware.

Additional classical approaches include:

- exhaustive brute-force search for small instances;
- greedy local search;
- tabu search;
- simulated annealing.

These provide useful reference points when evaluating quantum or hybrid approaches.

---

## 5. Quantum annealing

Quantum annealing uses a quantum Hamiltonian whose ground state encodes the solution of the optimization problem.

A standard annealing Hamiltonian can be written schematically as

\[
\boxed{
H(s)=
A(s)H_B+B(s)H_P,
\qquad s\in[0,1],
}
\]

where:

- $H_B$ is a driver Hamiltonian;
- $H_P$ is the problem Hamiltonian;
- $A(s)$ decreases during the anneal;
- $B(s)$ increases during the anneal.

The problem Hamiltonian encodes the Ising/QUBO objective.

In the ideal adiabatic picture, sufficiently slow evolution allows the system to remain close to the instantaneous ground state and finish in a state associated with a low-energy solution of the optimization problem.

Real quantum annealing systems are not purely isolated adiabatic systems: thermal effects, finite annealing time, control errors, embedding, chain breaks, and other hardware effects influence the obtained samples.

Consequently, the repository treats quantum annealing as a **sampling and optimization method whose performance must be measured experimentally**, rather than assuming that it automatically outperforms classical algorithms.

---

# 6. Optimization problems implemented

The repository contains explicit QUBO constructions for several representative combinatorial problems.

### Number Partitioning

Given numbers \(w_i\), divide them into two subsets with approximately equal total weight.

With \(x_i\in\{0,1\}\), the partition imbalance can be written as

\[
\Delta(x)=
\sum_i w_i(2x_i-1),
\]

and the optimization objective becomes

\[
\boxed{
\min_x \Delta(x)^2.
}
\]

This provides a compact example of the direct connection between a combinatorial problem and a quadratic binary energy.

### Max-Cut

Given a graph $G=(V,E)$, divide the vertices into two sets so that the total weight of edges crossing the partition is maximized.

For an edge $(i,j)$, the cut indicator is

\[
x_i+x_j-2x_ix_j.
\]

Therefore maximizing the cut is equivalent to minimizing

\[
\boxed{
E(x)=
-\sum_{(i,j)\in E}
w_{ij}
\left(
x_i+x_j-2x_ix_j
\right).
}
\]

Max-Cut is particularly natural for QUBO and Ising formulations because its graph structure maps directly onto pairwise interactions.

### Knapsack

The binary knapsack problem is

\[
\max_x
\sum_i v_i x_i
\]

subject to

\[
\sum_i w_i x_i\leq C.
\]

The repository converts the capacity constraint into a quadratic penalty using binary slack variables, producing an unconstrained QUBO.

### Graph Coloring

For a graph \(G=(V,E)\), binary variables represent assigning a vertex to a particular color.

One-hot constraints enforce

\[
\sum_c x_{ic}=1,
\]

while quadratic penalties discourage adjacent vertices from receiving the same color.

### Traveling Salesperson Problem

The TSP formulation uses time-indexed binary variables

$$
x_{i,p}=
\begin{cases}
1,&\text{city }i\text{ is visited at position }p,\\
0,&\text{otherwise}.
\end{cases}
$$

The QUBO combines:

- exactly-one constraints for every city;
- exactly-one constraints for every tour position;
- quadratic terms representing travel costs between consecutive positions.

### Scheduling

The scheduling formulation uses binary start-time variables

$$
x_{j,t}=
\begin{cases}
1,&\text{job }j\text{ starts at time }t,\\
0,&\text{otherwise}.
\end{cases}
$$

The QUBO enforces one start time per job and penalizes overlapping jobs on the shared resource. Optional start-time costs can also be included.

---

# 7. Repository architecture

The implementation is organized around five main components:

```text
src/
└── qa_optimization/
    ├── qubo/
    ├── problems/
    ├── solvers/
    ├── benchmarking/
    └── visualization/


# Quantum Annealing & Combinatorial Optimization

A structured learning and research repository for **binary optimization, QUBO models, Ising formulations, simulated annealing, quantum annealing, constraint encoding, embedding, and practical optimization problems**.

The central pipeline is

$$
\text{optimization problem}
\rightarrow
\text{binary formulation}
\rightarrow
\text{QUBO}
\rightarrow
\text{Ising}
\rightarrow
\text{annealing}
\rightarrow
\text{sampling and validation}.
$$

The repository is designed both as a **professional portfolio project** and as a **long-term tutorial/reference** for future quantum-optimization work.

## Learning path

### Beginner

- `docs/01_binary_optimization.md`
- `docs/02_qubo_mathematics.md`
- binary variables
- QUBO
- Number Partitioning
- Max-Cut

### Intermediate

- `docs/03_ising_model.md`
- `docs/04_annealing.md`
- `docs/06_constraints_and_penalties.md`
- QUBO $\leftrightarrow$ Ising
- simulated annealing
- constraints and penalties
- Knapsack
- Graph Coloring

### Advanced

- `docs/05_quantum_annealing.md`
- `docs/07_embedding.md`
- `docs/08_benchmarking.md`
- quantum annealing
- adiabatic evolution
- embedding and chains
- D-Wave/Ocean workflows
- benchmarking and scaling

## Repository structure

```text
quantum-annealing-optimization/
├── README.md
├── LICENSE
├── requirements.txt
├── pyproject.toml
├── docs/
│   ├── 01_binary_optimization.md
│   ├── 02_qubo_mathematics.md
│   ├── 03_ising_model.md
│   ├── 04_annealing.md
│   ├── 05_quantum_annealing.md
│   ├── 06_constraints_and_penalties.md
│   ├── 07_embedding.md
│   └── 08_benchmarking.md
├── src/
│   └── qa_optimization/
│       ├── qubo/
│       ├── problems/
│       ├── solvers/
│       ├── benchmarking/
│       └── visualization/
├── notebooks/
│   ├── 00_getting_started.ipynb
│   ├── 01_mathematical_foundations/
│   ├── 02_core_problems/
│   ├── 03_annealing/
│   ├── 04_quantum_annealing/
│   ├── 05_applications/
│   └── 06_mini_projects/
├── tests/
├── results/
└── references/
```

The separation is intentional:

| Component | Purpose |
|---|---|
| `docs/` | Theory, mathematics and derivations |
| `src/` | Reusable implementations |
| `notebooks/` | Guided experiments |
| `tests/` | Correctness checks |
| `results/` | Reproducible outputs |
| `references/` | Literature and documentation |

## Core mathematical idea

A QUBO is

$$
\min_{\mathbf{x}\in\{0,1\}^N}E(\mathbf{x}),
$$

with

$$
E(\mathbf{x})=
\sum_i a_i x_i+
\sum_{i<j}b_{ij}x_ix_j+c.
$$

Equivalently,

$$
E(\mathbf{x})=\mathbf{x}^{T}Q\mathbf{x}+c,
$$

under a stated matrix convention.

The binary-to-spin transformation used throughout the project is

$$
s_i=2x_i-1,
\qquad
x_i=\frac{1+s_i}{2}.
$$

It produces an Ising energy

$$
H(\mathbf{s})=
\sum_i h_i s_i+
\sum_{i<j}J_{ij}s_is_j+C.
$$

For quantum annealing, the classical problem becomes a problem Hamiltonian, conceptually embedded in

$$
\hat H(s)=A(s)\hat H_B+B(s)\hat H_P,
\qquad
s=\frac{t}{t_f}.
$$

## Problems studied

The planned progression is:

1. Number Partitioning
2. Max-Cut
3. Knapsack
4. Graph Coloring
5. Traveling Salesperson Problem
6. Scheduling
7. Portfolio Optimization

## Solvers and baselines

The repository remains solver-agnostic. It can contain:

- brute-force enumeration for small instances;
- greedy/local-search heuristics;
- simulated annealing;
- tabu search;
- D-Wave quantum annealing;
- hybrid quantum-classical approaches.

The purpose of benchmarking is to characterize behavior on controlled instance families rather than assume a universal winner.

## Recommended notebook structure

```text
1. Problem
2. Mathematical formulation
3. Binary encoding
4. QUBO derivation
5. Implementation
6. Exact/reference solution
7. Classical baseline
8. Annealing experiment
9. Validation
10. Scaling or parameter study
11. Results
12. Conclusions
```

The mathematics should appear **before** the implementation that realizes it.

## Reproducibility

Experiments should report the instance-generation method, problem size, random seeds, number of reads, solver parameters, penalty coefficients, embedding settings, software versions, and hardware information when relevant.

For an optimum $E^\star$, a minimization gap is

$$
\Delta E=E_{\mathrm{best}}-E^\star.
$$

For constrained problems, feasibility should always be reported separately from energy.

## Design philosophy

### Understand the model before using the solver

The difficult part is often translating the original problem into variables, objective terms and constraints.

### Validate small cases exactly

Exhaustive enumeration is an essential tool for checking QUBO derivations and solver/post-processing code.

### Separate formulation from solving

The same QUBO should be usable with different classical and quantum solvers.

### Compare like with like

Use the same instances, objective definition, feasibility rules and appropriate computational budgets.

### Treat hardware effects explicitly

Quantum-annealing experiments should expose embedding, chains, sampling parameters and post-processing rather than hiding them behind one final energy.

## Main references

- A. Lucas, *Ising formulations of many NP problems*, Frontiers in Physics (2014).
- S. Kirkpatrick, C. Gelatt and M. Vecchi, *Optimization by Simulated Annealing*, Science (1983).
- T. Kadowaki and H. Nishimori, *Quantum annealing in the transverse Ising model*, Physical Review E (1998).
- E. Farhi et al., *A Quantum Adiabatic Evolution Algorithm Applied to Random Instances of an NP-Complete Problem* (2001).
- F. Glover, G. Kochenberger and Y. Du, *A Tutorial on Formulating and Using QUBO Models*.
- D-Wave Ocean documentation for BQM, embedding and sampler workflows.

## Final picture

$$
\boxed{
\text{Mathematics}
\rightarrow
\text{QUBO}
\rightarrow
\text{Ising}
\rightarrow
\text{Annealing}
\rightarrow
\text{Quantum Annealing}
\rightarrow
\text{Applications}
\rightarrow
\text{Benchmarking}
}
$$
