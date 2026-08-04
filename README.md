# Quantum Annealing for Optimization Problems

This project demonstrates how real-world combinatorial optimization problems can be mapped into QUBO (Quadratic Unconstrained Binary Optimization) form and solved using quantum annealing techniques, with direct relevance to industrial applications.

Many complex problems in logistics, finance, and engineering can be formulated as optimization tasks. Quantum annealing provides a framework to solve these problems by transforming them into energy minimization problems.

In this project, we:

- Formulate optimization problems as QUBO models
- Solve them using classical and quantum-inspired annealing
- Compare solution quality and performance

---

## Problem Definition

We focus on the **Max-Cut problem**, a fundamental graph partitioning problem:

> Given a graph, divide its nodes into two sets such that the number of edges between the sets is maximized.

This problem appears in:

- Network design  
- Circuit layout  
- Clustering  

---

## Methodology

### 1. QUBO Formulation

We transform the Max-Cut problem into a QUBO model:

- Binary variables represent node assignments
- The objective function encodes edge cuts
- The solution corresponds to minimizing an energy function

---

### 2. Solvers

We implement and compare:

- **Classical simulated annealing** (baseline)
- **Quantum-inspired annealing** (QUBO-based solver)

---

### 3. Evaluation Metrics

We evaluate performance using:

- Energy (objective value)
- Solution quality (cut size)
- Stability across runs

---

## Results

The project includes:

- Graph partition visualizations  
- Energy convergence plots  
- Comparison between classical and annealing approaches  

---

##  Industrial Relevance

Optimization problems like Max-Cut are directly applicable to:

- Logistics and routing optimization  
- Energy grid management  
- Portfolio optimization in finance  

Quantum annealing offers a promising alternative to classical heuristics for tackling these challenges.

---



## Tech Stack

- Python  
- NumPy  
- NetworkX  
- Matplotlib  
- D-Wave Ocean SDK / dimod  

