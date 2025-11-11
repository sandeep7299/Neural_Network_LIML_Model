# Neural Network LIML Model — Simulation Comparisons

This repository contains simulation notebooks comparing LIML, MLE, and NN-LIML
estimators for both linear and nonlinear data generating processes (DGPs).

Files
- `NN-CODE-UPDATED__executed_20251106_172624.ipynb` — Linear DGP simulations and
  comparisons (LIML, MLE, Linear-NN). Includes simulation runner, plotting, and
  summary tables.
- `merge_UPDATED__executed_20251106_193640.ipynb` — Nonlinear DGP simulations
  (orthogonalized nonlinear instruments). Contains the NN L2 + LIML L1 pipeline
  (cross-fit NN first stage, LIML second stage), summaries, and bias-only plots.

Quick start
1. Create a Python environment and install the dependencies (recommended):

   pip install -r requirements.txt

2. Open and run the notebooks with Jupyter (Notebook or Lab):

   jupyter lab   # or `jupyter notebook`

3. In each notebook, the main runner block calls the simulation harness:
   - Linear notebook: `run_all_simulations(...)`
   - Nonlinear notebook: `run_all_simulations_nonlinear(...)`

Notes & recommendations
- The notebooks are computationally intensive. Reduce `NUM_SIMS` and/or the
  `NS` values (sample sizes) to speed up runs locally.
- The NN training uses PyTorch and will use CUDA if available. Adjust
  `NNConfig.device` in the notebook if you want to force CPU/GPU.
- If you plan to reproduce results, set a fixed seed (noted at the top of the
  notebooks) and install the packages in `requirements.txt`.

Suggested minimal requirements (see `requirements.txt`):
- numpy, pandas, matplotlib, scipy, torch, jupyter

If you'd like, I can:
- add/verify a `requirements.txt` in this folder,
- add a `LICENSE` file (MIT/Apache), or
- move notebooks into a `notebooks/` subfolder and add a small runner script.

---
Generated and added by automation to accompany the notebooks in
`C:\Users\mudhu\Downloads`.
# Neural Network + LIML / MLE — Linear comparison

This repository contains the reproducible notebook and supporting files for the linear comparison study of three estimation methods:

- LIML (Limited Information Maximum Likelihood)
- MLE (Maximum Likelihood Estimation)
- Linear-NN (a linear neural-network-enhanced estimator: NN L2 for first stage + LIML L1 for structural stage)

Contents
- `NN-CODE-UPDATED__executed_20251106_172624.ipynb` — the main Jupyter notebook. It implements the data generating process (DGP), estimation routines (LIML, MLE, Linear-NN), a simulation harness, and plotting utilities (bias-only grouped boxplots and summary tables).
- `.gitignore` — project ignore rules.
- `requirements.txt` — (if present) lists Python packages used to run the notebook.

Quick usage
1. Create and activate a virtual environment (optional but recommended).

   PowerShell example:
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

2. Launch Jupyter and open the notebook:
```powershell
jupyter notebook NN-CODE-UPDATED__executed_20251106_172624.ipynb
```

3. Notebook notes:
- The notebook relies on numpy, pandas, scipy, matplotlib and PyTorch. `requirements.txt` in this repo contains compatible versions.
- For speed while exploring, reduce `NUM_SIMS` and the `NS` list in the notebook before running full experiments.
- The `NNConfig` dataclass at the end of the notebook controls NN training (epochs, batch size, device). By default the code chooses CUDA if available.

Recommended environment (minimum)
- Python 3.9+ (3.10/3.11 recommended)
- numpy
- pandas
- scipy
- matplotlib
- torch (PyTorch)
- jupyter

Acknowledgements
This code is intended for reproducible simulation and method comparison. If you use it in a paper or report, please cite appropriately.

If you'd like additional edits (short README section per-figure, add LICENSE, or reorganize the repo), tell me which change and I will update and push it.
