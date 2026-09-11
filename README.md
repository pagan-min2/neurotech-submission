# Modeling a Spiking Neuron: Parameter Tuning via Numerical Optimization

This repository contains a complete workflow for modeling, simulating, and optimizing parameters for a compact spiking neuron model. The project covers numerical approximation using Euler's method, manual parameter exploration, algorithmic optimization via `scipy.optimize`, and comparative analysis against target electrophysiological data (`spike_data.csv`).

## Model Overview

The underlying system models membrane voltage (v) and internal recovery/adaptation state variables (u and w) governed by the following coupled differential equations:

dv/dt = 0.04v^2 + 5v + 140 - u - w + I(t)
du/dt = a(bv - u)
dw/dt = -kw

When the membrane potential reaches the spiking threshold (v >= 30 mV), a spike is recorded and the variables are reset:
* v = c
* u = u + d
* w = w + e

Where:
* v: Membrane voltage (mV)
* u, w: Internal state variables
* a, b, c, d, e: Parameters optimized in this task
* k: Fixed recovery decay constant (0.05)
* I(t): Input step-current time-series

---

## Project Structure

* **`neuron_parameter_recovery.ipynb`**: The primary interactive workspace containing code blocks and detailed answers for Parts 1, 2A, 2B, and 3.
* **`spike_data.csv`**: Target voltage trace data representing empirical or target neuron activity under a known input current I(t).

---

## Key Methodology & Findings

### Part 1: Numerical Simulation (Euler's Method)
Simulates the continuous differential equations over discrete time steps (0.5 ms) across a 200 ms window with a step current applied from 10 ms to 100 ms.

### Part 2A: Manual Parameter Tuning
Explores how parameters a, b, c, d, and e affect spike frequency, adaptation, and membrane recovery by manual trial and error.
* **a**: Controls recovery time scales and post-stimulation curve dip.
* **b**: Affects sub-threshold fluctuations and spike counts.
* **c**: Resets potential after firing, shifting inter-spike dynamics.
* **d**: Acts as a sensitive spike trigger parameter.
* **e**: Manages long-term adaptation buildup.

### Part 2B: Algorithmic Parameter Tuning (Optimization)
* **Loss Function**: Mean Squared Error (MSE) computed between simulated voltage and target voltage. MSE heavily penalizes larger deviations and prevents positive/negative errors from canceling out.
* **Optimization Algorithm**: Nelder-Mead simplex algorithm implemented via `scipy.optimize.minimize`.

### Part 3: Comparison & Analysis
Compares initial manual estimates against optimized parameters:
* **Closest Match**: Parameter a (initial guess: 0.02, optimized: ~0.0209).
* **Largest Variance**: Parameter c (initial guess: -60.0, optimized: ~-62.57), reflecting subtle effects on post-spike reset behavior that are difficult to eyeball manually.
