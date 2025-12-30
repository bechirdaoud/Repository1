
## Dataset name
**BBO Capstone — Query/Observation Logs (Functions 1–8)**

## What this dataset is
A collection of **sequential black-box evaluations** produced during a Bayesian Black-Box Optimisation (BBO) challenge.  
Each record is one query: an input vector `x ∈ [0,1]^d` and a scalar objective value `y` (maximisation framing).

## Folder structure (how the dataset is stored)
All data is stored inside a **Google Colab folder** with two subfolders:

- `initial_data/`  
  Contains the **initial observed tables** provided at the start of the challenge (per function).

- `submission/`  
  Contains **one saved file per submission/round**, kept independently to track the evolution of the optimisation over time.

This design makes it easy to:
1) start from the same initial conditions, and  
2) reproduce or audit what changed at each round.

## What a “row” represents
One row corresponds to **one evaluated point**:
- the first `d` columns are the input features (`x1 … xd`) in `[0,1]`
- the last column is the observed objective value (`y`)

Each function has its own dimensionality `d`:
- Function 1: 2D  
- Function 2: 2D  
- Function 3: 3D  
- Function 4: 4D  
- Function 5: 4D  
- Function 6: 5D  
- Function 7: 6D  
- Function 8: 8D  

## Dataset size
- Per function, the dataset contains the initial observations plus a small number of new points added across rounds.
- Overall, each function ends up with **only tens of rows**, because the evaluation budget is intentionally tight.

## How to reconstruct the full history 
1) Load the initial table for a function from `initial_data/`  
2) Append the points from each file in `submission/` in chronological order  
3) The resulting table is the full query history for that function

## Intended use
- Reproducibility and auditing of BBO decisions over time
- Comparing exploration/exploitation behaviour under strict budgets
- Studying how different surrogate choices affect the search trajectory (GP vs SVM vs NN)

