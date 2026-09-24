# 4. Annealing and Simulated Annealing

## 4.1 Energy landscapes

Once a problem is written as

$$
E(\mathbf{x}),
$$

the configuration space $\{0,1\}^N$ becomes a discrete energy landscape. The goal is

$$
\mathbf{x}^\star=\operatorname*{arg\,min}_{\mathbf{x}}E(\mathbf{x}).
$$

A local minimum is a state whose nearby configurations all have greater or equal energy, even if a lower-energy state exists elsewhere.

## 4.2 Local moves

For a proposed configuration $\mathbf{x}'$, define

$$
\Delta E=E(\mathbf{x}')-E(\mathbf{x}).
$$

If $\Delta E<0$, the move improves the objective. If $\Delta E>0$, accepting it can help escape local minima.

## 4.3 Simulated annealing

Simulated annealing introduces a temperature $T$. The Metropolis acceptance probability is

$$
P_{\mathrm{acc}}=
\begin{cases}
1,&\Delta E\leq0,\\
\exp(-\Delta E/T),&\Delta E>0.
\end{cases}
$$

At high $T$, uphill moves are relatively common. As $T$ decreases, the process becomes increasingly selective.

## 4.4 Cooling schedules

A geometric schedule is

$$
T_k=T_0\alpha^k,
\qquad 0<\alpha<1.
$$

A hyperbolic alternative is

$$
T_k=\frac{T_0}{1+\beta k}.
$$

There is no universal optimal schedule; its effectiveness depends on the landscape and implementation.

## 4.5 Sweeps

A sweep often means approximately one attempted variable update per variable. A run can consist of many sweeps at a sequence of temperatures.

Important parameters include:

- initial temperature;
- final temperature;
- cooling schedule;
- sweeps;
- number of reads;
- random seed.

## 4.6 Stochastic sampling

If a solver is run $R$ times, the outputs are

$$
\mathbf{x}^{(1)},\ldots,\mathbf{x}^{(R)}.
$$

If the optimum is known, the empirical success probability is

$$
\hat p_{\mathrm{success}}=
\frac{N_{\mathrm{optimal}}}{R}.
$$

The full energy distribution is often more informative than a single best sample.

## 4.7 Exact validation

For a small instance,

$$
E^\star=\min_{\mathbf{x}\in\{0,1\}^N}E(\mathbf{x}).
$$

If simulated annealing returns $E_{\mathrm{best}}$, define

$$
\Delta E=E_{\mathrm{best}}-E^\star\geq0.
$$

The optimum is found when $\Delta E=0$.

## 4.8 Parameter studies

A useful experiment varies one parameter while holding the others fixed, for example

$$
N_{\mathrm{sweeps}}\in\{10,100,1000,10000\}.
$$

Record best energy, feasibility, success rate and runtime.

## 4.9 Why simulated annealing belongs in this repository

It is both a useful classical optimizer and an important baseline for quantum annealing. A quantum result should normally be interpreted alongside classical references rather than in isolation.

## 4.10 Summary

The central idea is

$$
\boxed{
\text{high-temperature exploration}
\rightarrow
\text{cooling}
\rightarrow
\text{low-energy sampling}
}.
$$

The method is stochastic, so repeated runs and statistical analysis are essential.
