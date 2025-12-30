## Model / system name
**BBO Capstone Optimiser — Surrogate Model + Acquisition-Based Querying**

## What this is
This is not a single predictive model deployed in production.  
It is a **sequential optimisation system** that proposes the next query point for each function using:
1) a **surrogate model** fit on observed data `(X, y)`, and  
2) an **acquisition rule** to choose the next `x` to evaluate.

## Models used (surrogates)
Across runs, I used multiple surrogate families depending on dimensionality and behaviour:

- **Gaussian Process Regression (GP/GPR)**  
  Used frequently in low-to-mid dimensions for its sample efficiency and uncertainty estimates.

- **Support Vector Machine (SVM) surrogate**  
  Used in some runs, especially in higher dimensions, to test alternatives when GP assumptions or calibration felt less reliable.

- **Neural Network (NN) surrogate**  
  Used in some runs (mainly high dimensional) as a flexible function approximator when strong non-linear patterns were suspected.

## Acquisition and candidate generation (high level)
Typical loop per function and per round:
- Fit the chosen surrogate on the current dataset
- Generate candidate points (often using **Sobol** sequences for space-filling sampling)
- Rank candidates using an acquisition function such as:
  - **UCB (Upper Confidence Bound)** or  
  - **EI (Expected Improvement)**
- Submit the best candidate as the next query

## Input / output
- **Input:** past evaluated points `X ∈ [0,1]^{n×d}` and outputs `y ∈ R^n`
- **Output:** next query `x_next ∈ [0,1]^d` (maximisation)

## Tasks covered (from the project definition)
Eight black-box functions, each returning a scalar value to maximise:
- 2D: Functions 1–2
- 3D: Function 3
- 4D: Functions 4–5
- 5D: Function 6
- 6D: Function 7
- 8D: Function 8

These are framed as maximisation problems, with real-world analogies such as source detection, noisy likelihood maximisation, drug discovery trade-offs, operational optimisation, process yield tuning, and hyperparameter-style optimisation.

## Intended use
- Efficiently propose queries under a strict evaluation limit
- Compare surrogate choices (GP vs SVM vs NN) under the same budget constraints
- Support analysis of exploration–exploitation trade-offs in low-data optimisation

## Limitation (restricted to budget)
- **Tight evaluation budget:** with only a small number of evaluations, the optimiser can be forced into hard trade-offs between following a promising local region and exploring new areas. This becomes more severe as dimensionality grows, because the space is large and only lightly sampled.
