# 7. Minor Embedding and Quantum-Annealing Hardware

## 7.1 Logical problem graph

An Ising model

$$
H(\mathbf s)=\sum_ih_is_i+\sum_{i<j}J_{ij}s_is_j
$$

defines a logical graph. Each variable is a vertex and each nonzero $J_{ij}$ is an edge.

Quantum-annealing hardware has its own physical connectivity graph. The logical graph may not fit directly onto it.

## 7.2 Minor embedding

A logical variable can be represented by a connected set of physical qubits:

$$
\text{logical variable}
\rightarrow
\text{physical chain}.
$$

This process is called minor embedding.

## 7.3 Chain coupling

Suppose a logical variable is represented by

$$
s_{i,1},\ldots,s_{i,L}.
$$

A chain term can be written as

$$
H_{\mathrm{chain}}
=-J_F\sum_{\ell=1}^{L-1}s_{i,\ell}s_{i,\ell+1},
\qquad J_F>0.
$$

Under this convention, aligned spins are favored.

## 7.4 Chain strength

The chain strength $J_F$ must balance the original problem couplings.

If it is too weak, chains can break:

$$
s_{i,\ell}\neq s_{i,\ell+1}.
$$

If it is excessively strong, the chain energy can dominate the problem energy scale.

Therefore chain strength should be treated as an experimentally studied parameter.

## 7.5 Chain-break fraction

For a chain of length $L$, a simple local measure is

$$
\mathrm{CB}
=
\frac{1}{L-1}
\sum_{\ell=1}^{L-1}
\mathbf 1[s_{i,\ell}\neq s_{i,\ell+1}].
$$

A complete experiment can report the fraction of logical chains that contain a disagreement. Software packages may define aggregate chain-break metrics differently, so the exact definition should be documented.

## 7.6 Logical versus physical size

If there are $N_{\mathrm{logical}}$ variables but $N_{\mathrm{phys}}$ physical qubits are used, define the overhead

$$
R_{\mathrm{embed}}
=\frac{N_{\mathrm{phys}}}{N_{\mathrm{logical}}}.
$$

Values greater than one are normal.

## 7.7 Dense graphs

A complete logical graph $K_N$ has

$$
M=\frac{N(N-1)}{2}
$$

interactions.

A limited-degree hardware graph cannot directly realize all these couplers, so embedding can require substantial overhead.

For a graph with $N$ vertices and $M$ edges, a useful density is

$$
\rho=\frac{2M}{N(N-1)}.
$$

Embedding studies can therefore vary both size and density.

## 7.8 Runtime components

A realistic end-to-end runtime can be decomposed as

$$
T_{\mathrm{total}}
=
T_{\mathrm{embedding}}
+T_{\mathrm{sampling}}
+T_{\mathrm{postprocessing}},
$$

when these quantities are measured consistently.

Other preprocessing or programming costs should be reported if included in the benchmark.

## 7.9 Physical Hamiltonian

Schematically,

$$
H_{\mathrm{phys}}
=H_{\mathrm{problem}}+H_{\mathrm{chain}}.
$$

The chain terms are an implementation mechanism, not part of the original mathematical objective.

## 7.10 Chain-break post-processing

A broken chain does not define a unique logical variable. Possible resolution methods include:

- majority vote;
- local/energy-based assignment;
- software-provided chain-break methods.

The chosen rule can affect the reported logical solution and must therefore be part of the experiment specification.

## 7.11 Embedding experiment

For each instance, record:

| Quantity | Meaning |
|---|---|
| $N_{\mathrm{logical}}$ | logical variables |
| $M$ | logical interactions |
| $\rho$ | logical density |
| $N_{\mathrm{phys}}$ | physical qubits |
| chain length | physical representation size |
| chain-break fraction | chain consistency |
| embedding time | embedding cost |
| best energy | optimization quality |
| feasibility | constraint satisfaction |

## 7.12 Conceptual separation

Embedding does not redefine the original optimization problem. It maps the logical problem onto available hardware connectivity:

$$
\boxed{
\text{logical optimization model}
\neq
\text{physical embedding}
}.
$$

The logical model defines what should be solved; the embedding defines how the hardware realizes it.
