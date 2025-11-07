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
