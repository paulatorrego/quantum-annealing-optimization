# Quantum Annealing for Optimization Problems

## Overview
This project explores how quantum annealing can be used to solve combinatorial optimization problems, focusing on practical applications relevant to industry.

## Motivation
Many real-world problems in logistics, finance, and engineering can be formulated as optimization tasks. Quantum annealing offers a promising approach to tackle these problems by mapping them into energy minimization formulations.

This project demonstrates how classical optimization problems can be translated into QUBO (Quadratic Unconstrained Binary Optimization) form and solved using quantum-inspired methods.

## Problem
We focus on solving:

- Max-Cut problem (graph partitioning)
- Knapsack problem

## Methods

### 1. QUBO Formulation
We convert the optimization problem into a QUBO representation:

- Binary variables
- Quadratic cost function
- Energy minimization objective

### 2. Solvers
We compare different approaches:

- Simulated annealing (classical baseline)
- Quantum annealing (D-Wave Ocean SDK or simulated backend)

### 3. Evaluation
- Solution quality
- Energy convergence
- Comparison between methods

## Results

- Visualization of graph partitions (MaxCut)
- Energy evolution during annealing
- Comparison between classical and quantum approaches

## Tech Stack

- Python
- D-Wave Ocean SDK (or dimod)
- NumPy
- NetworkX
- Matplotlib

## Project Structure
