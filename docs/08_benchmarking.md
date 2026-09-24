# 8. Benchmarking Quantum and Classical Optimization

## 8.1 Why benchmarking matters

A low-energy solution is not, by itself, evidence of good solver performance. A useful benchmark requires a controlled problem family, appropriate baselines, reproducible settings, clear metrics and statistical analysis.

## 8.2 Problem families

For random graphs, one example is

$$
G\sim\mathcal G(N,p),
$$

where $N$ is the number of vertices and $p$ the edge probability.

For every instance, store its size, structure, weights and random seed. Use multiple instances at each size because equal-size instances can have very different difficulty.

## 8.3 Exact references

For small instances,

$$
E^\star=\min_{\mathbf x\in\{0,1\}^N}E(\mathbf x)
$$

can be obtained by exhaustive enumeration.

This is useful for validating the formulation, QUBO implementation, solver and post-processing.

For larger instances, clearly distinguish exact optima from reference solutions or best-known solutions.

## 8.4 Objective quality

For minimization,

$$
E_{\mathrm{best}}=\min_rE(\mathbf x^{(r)}).
$$

If the optimum is known,

$$
\Delta E=E_{\mathrm{best}}-E^\star\geq0.
$$

For maximization,

$$
\Delta F=F^\star-F_{\mathrm{best}}\geq0.
$$

## 8.5 Optimality gap

A normalized gap can be defined as

$$
g=\frac{E_{\mathrm{best}}-E^\star}{S},
$$

where $S$ is an explicitly defined problem scale. Using $|E^\star|$ blindly is problematic when $E^\star=0$, so the denominator should be chosen deliberately.

## 8.6 Success probability

If the optimum is known,

$$
\hat p_{\mathrm{success}}
=\frac{N_{\mathrm{optimal}}}{N_{\mathrm{reads}}}.
$$

For independent reads, a simple standard-error approximation is

$$
\sigma_{\hat p}
\approx
\sqrt{\frac{\hat p(1-\hat p)}{N_{\mathrm{reads}}}}.
$$

For small samples or extreme probabilities, use an appropriate binomial confidence interval instead.

## 8.7 Feasibility rate

For constrained problems,

$$
p_{\mathrm{feasible}}
=\frac{N_{\mathrm{feasible}}}{N_{\mathrm{reads}}}.
$$

This should be reported separately from objective quality.

## 8.8 Runtime

Possible timing quantities include

$$
T_{\mathrm{solve}},
\qquad
T_{\mathrm{sampling}},
\qquad
T_{\mathrm{total}}.
$$

For quantum hardware, embedding, programming, annealing, readout and classical post-processing may be distinct components. A comparison is meaningful only when the included components are clearly defined.

## 8.9 Reads and computational budget

A solver using

$$
R=10^5
$$

reads has more sampling opportunities than one using

$$
R=10^2.
$$

Therefore reads are an experimental resource and should be reported as part of the computational budget.

## 8.10 Classical baselines

Useful references include:

- exact enumeration for small cases;
- greedy methods;
- local search;
- simulated annealing;
- tabu search;
- other appropriate classical heuristics.

The baseline should be strong enough to make the comparison meaningful, not selected merely because it is easy to beat.

## 8.11 Repeated experiments

For measurements $X_1,\ldots,X_R$,

$$
\bar X=\frac1R\sum_{r=1}^RX_r,
$$

and

$$
\sigma_X=
\sqrt{\frac{1}{R-1}\sum_{r=1}^R(X_r-\bar X)^2}.
$$

For skewed distributions, medians and quantiles can be more informative than the mean.

## 8.12 Scaling studies

A scaling study can use

$$
N\in\{5,10,20,40,80,160\}.
$$

For every $N$:

1. generate multiple independent instances;
2. solve each with the selected methods;
3. use comparable budgets;
4. record quality, feasibility and runtime;
5. aggregate over the instance ensemble.

## 8.13 Instance variability

Let

$$
I_N=\{I_{N,1},\ldots,I_{N,K}\}
$$

be the ensemble of instances at size $N$. Report distributions across the ensemble rather than relying on a single graph.

## 8.14 Quantum-annealing benchmark checklist

Record:

- logical variables and couplers;
- QUBO/Ising convention;
- coefficient scaling;
- penalty coefficients;
- embedding method;
- physical-qubit count;
- chain lengths and chain breaks;
- annealing time;
- number of reads;
- post-processing;
- best energy;
- feasibility;
- success probability when known;
- timing components;
- hardware and software versions.

## 8.15 Example table

| Method | $N$ | Reads | Best energy | Gap | Feasible rate | Success rate | Runtime |
|---|---:|---:|---:|---:|---:|---:|---:|
| Exact | 20 | — | $E^\star$ | 0 | 1.00 | 1.00 | $T_{\mathrm{exact}}$ |
| Simulated annealing | 20 | 1000 | $E_{\mathrm{SA}}$ | $\Delta E_{\mathrm{SA}}$ | $p_{\mathrm{SA}}$ | $q_{\mathrm{SA}}$ | $T_{\mathrm{SA}}$ |
| Quantum annealing | 20 | 1000 | $E_{\mathrm{QA}}$ | $\Delta E_{\mathrm{QA}}$ | $p_{\mathrm{QA}}$ | $q_{\mathrm{QA}}$ | $T_{\mathrm{QA}}$ |

The timing definition must be stated explicitly.

## 8.16 Claims to avoid

Do not conclude that quantum annealing is universally faster, always finds the optimum, or automatically gives an asymptotic advantage from a small benchmark.

A rigorous conclusion is specific to the tested instance family, solver configurations, computational budgets and metrics.

## 8.17 Reproducibility checklist

- [ ] instance family defined;
- [ ] seeds stored;
- [ ] objective documented;
- [ ] constraints documented;
- [ ] penalty values reported;
- [ ] QUBO convention documented;
- [ ] solver parameters stored;
- [ ] number of reads reported;
- [ ] independent repetitions performed;
- [ ] exact solutions used where feasible;
- [ ] embedding information recorded;
- [ ] runtime definition explicit;
- [ ] software versions recorded;
- [ ] hardware information recorded where relevant.

## 8.18 Final principle

A rigorous benchmark follows

$$
\boxed{
\text{controlled instances}
\rightarrow
\text{multiple solvers}
\rightarrow
\text{consistent metrics}
\rightarrow
\text{repeated experiments}
\rightarrow
\text{statistical analysis}
}.
$$

For quantum annealing, expose the complete pipeline:

$$
\text{formulation}
\rightarrow
\text{embedding}
\rightarrow
\text{sampling}
\rightarrow
\text{post-processing}
\rightarrow
\text{validation}.
$$
