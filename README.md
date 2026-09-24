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
