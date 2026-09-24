# 3. QUBO and the Ising Model

## 3.1 Binary and spin variables

QUBO variables satisfy

$$
x_i\in\{0,1\},
$$

whereas Ising spins satisfy

$$
s_i\in\{-1,+1\}.
$$

Use

$$
s_i=2x_i-1,
\qquad
x_i=\frac{1+s_i}{2}.
$$

The pairwise product transforms as

$$
x_ix_j=\frac{1+s_i+s_j+s_is_j}{4}.
$$

## 3.2 General transformation

Start from

$$
E(\mathbf{x})=
\sum_i a_i x_i+
\sum_{i<j}b_{ij}x_ix_j+c.
$$

After substitution,

$$
E(\mathbf{s})=C+\sum_i h_i s_i+\sum_{i<j}J_{ij}s_is_j.
$$

For the pairwise-coefficient convention above,

$$
J_{ij}=\frac{b_{ij}}{4},
$$

and

$$
h_i=\frac{a_i}{2}+\frac14\sum_{j\neq i}b_{ij},
$$

with the constant

$$
C=c+\frac12\sum_i a_i+\frac14\sum_{i<j}b_{ij}.
$$

The exact matrix formula depends on how off-diagonal QUBO coefficients are stored, so implementation and documentation must use one convention consistently.

## 3.3 Example

Take

$$
E(x_1,x_2)=3x_1-2x_2+4x_1x_2.
$$

Using $x_i=(1+s_i)/2$,

$$
3x_1=\frac32+\frac32s_1,
$$

$$
-2x_2=-1-s_2,
$$

and

$$
4x_1x_2=1+s_1+s_2+s_1s_2.
$$

Therefore

$$
E=\frac32+\frac52s_1+s_1s_2.
$$

The binary and spin formulations have corresponding minimizing configurations.

## 3.4 Ising Hamiltonian

The classical Ising energy is

$$
H_{\mathrm{Ising}}(\mathbf{s})=
\sum_i h_i s_i+
\sum_{i<j}J_{ij}s_is_j.
$$

In quantum annealing, promote spins to Pauli-$Z$ operators:

$$
s_i\rightarrow\hat\sigma_i^z.
$$

The problem Hamiltonian is

$$
\hat H_P=
\sum_i h_i\hat\sigma_i^z+
\sum_{i<j}J_{ij}\hat\sigma_i^z\hat\sigma_j^z.
$$

For a computational-basis state $|\mathbf{s}\rangle$,

$$
\hat\sigma_i^z|\mathbf{s}\rangle=s_i|\mathbf{s}\rangle,
$$

so

$$
\hat H_P|\mathbf{s}\rangle
=
H_{\mathrm{Ising}}(\mathbf{s})|\mathbf{s}\rangle.
$$

The classical optimization energy is therefore encoded directly in the diagonal spectrum of $\hat H_P$.

## 3.5 Interaction signs

For the convention

$$
J_{ij}s_is_j,
$$

negative $J_{ij}$ favors aligned spins, while positive $J_{ij}$ favors anti-aligned spins.

Always state the Hamiltonian sign convention before interpreting the terms as ferro- or antiferromagnetic.

## 3.6 Degeneracy and symmetry

If

$$
E(\mathbf{s}^{(1)})=E(\mathbf{s}^{(2)})=E^\star,
$$

both are optimal.

Max-Cut has a simple global spin-flip symmetry: exchanging the two partitions can map one optimal configuration to another with the same cut value.

## 3.7 Validation

For every small binary configuration, compute the QUBO energy and its mapped Ising energy. Verify

$$
E_{\mathrm{QUBO}}(\mathbf{x})
=
E_{\mathrm{Ising}}(\mathbf{s})+C.
$$

This should be an automated unit test.

## 3.8 Conceptual chain

$$
\boxed{
\text{QUBO}
\leftrightarrow
\text{Ising}
\leftrightarrow
\text{problem Hamiltonian}
}
$$

This transformation is the bridge between binary optimization and quantum annealing.
