Here you go — **copy-paste ready** README for your repo.
(Just paste this into `README.md` at the root.)

---

# Neural_Network_LIML_Model

End-to-end code and notebooks for comparing **MLE**, **LIML with linear first stage**, and **NN-LIML** on both **simulated** and **real Spanish dairy farm** data.
The repo includes parity-fair first-stage setups (same instruments (Z) for linear and NN), cross-fitted out-of-fold predictions, likelihood-based second stage, and publication-ready result tables/plots.

---

## 🔎 What this project does (TL;DR)

* Builds a **rich instrument set (Z)** (prices, ownership, COOP, year/region dummies, squares & interactions).
* Trains **two first-stage models** to predict endogenous regressors ((\text{LABOR}, \text{COWS}, \text{FEED}, \text{ROUGHAGE})):

  * OLS (linear)
  * MLP (neural net) with ELU, dropout, and learned residual covariance.
* Uses the **same (Z)** for both models and **K-fold cross-fitting** → out-of-fold residuals (\hat\eta) and covariance (\widehat\Sigma_{\eta\eta}).
* Runs **LIML (L1-only)** on the structural SFA with a **control-function** correction using (\widehat\Sigma_{\eta\eta}).
* Compares **MLE**, **LIML–Linear**, **NN–LIML**: coefficients, standard errors (OPG), variance components, TE means, and first-stage (R^2).

---

## 📁 Repository structure

```
Neural_Network_LIML_Model/
├─ LINEAR_NN-CODE.ipynb                # First-stage linear vs NN + diagnostics
├─ MLE_vs_LIML_vs_NN_LIML (1).ipynb    # End-to-end real-data comparison & tables
├─ NON-LINEAR-LIML-MODEL               # Nonlinear DGP notebook (simulation)
├─ REAL_DATA_SIM.ipynb                 # Linear DGP simulation (parity setup)
├─ Traditional_sim.ipynb               # Traditional MLE/LIML simulation baseline
├─ SPANISH DATASET.xlsx                # (Small sample example) — replace if large
└─ .gitignore
```

> If your dataset is large (>100 MB), **don’t commit it**. Put it outside Git and point the notebooks to the local path, or link to the data source in this README.

---

## ⚙️ Environment & setup

### Option A — conda (recommended)

```bash
conda create -n nnliml python=3.10 -y
conda activate nnliml
pip install numpy pandas scipy scikit-learn matplotlib jupyter
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121  # or cpu wheels
```

### Option B — pure pip

```bash
python -m venv .venv
source .venv/bin/activate  # (Windows: .venv\Scripts\activate)
pip install -U pip
pip install numpy pandas scipy scikit-learn matplotlib jupyter torch torchvision
```

Launch Jupyter:

```bash
jupyter notebook
```

---

## ▶️ How to run

### 1) Real-data end-to-end comparison

Open **`MLE_vs_LIML_vs_NN_LIML (1).ipynb`** and run cells top to bottom.
You’ll get:

* First-stage OOF (R^2) (linear vs NN) per endogenous regressor.
* (\widehat\Sigma_{\eta\eta}) eigen spectrum + condition numbers.
* Unified **Est & St.Err** table across **MLE / LIML–Linear / NN–LIML**.
* Optional elasticities plot with 95% CI (OPG).

**Switches you can edit inside the notebook:**

* `K_FOLDS`, `NN_EPOCHS`, `NN_KFOLDS`
* Symmetric shrinkage on (\widehat\Sigma_{\eta\eta}): `USE_SYMMETRIC_SHRINK = True/False`, `RHO_SHRINK`
* Include/exclude YEAR dummies **in X** (kept in **Z** by default).

  * SFA structural **X** typically **excludes YEAR** unless you interpret them as time effects; **Z** can include YEAR dummies safely.

### 2) Simulations

* **`REAL_DATA_SIM.ipynb`** — NN Implemented DGP parity: MLE / LIML / NN-LIML side-by-side.
* **`NON-LINEAR-LIML-MODEL`** — nonlinear DGP (orthogonalized nonlinearity); includes boxplots and RMSE tables.
* **`Traditional_sim.ipynb`** — baseline reference for MLE/LIML.

---

## 🧠 Model details (quick notes)

* **First stage (NN)**: MLP with ELU, dropout, learned lower-triangular (L) → (\Sigma = LL^\top).
  **Loss**: multivariate **Gaussian NLL** on ((X_2 - \hat X_2)) with small weight decay; trained with **K-fold cross-fit** to get OOF residuals.
* **First stage (Linear)**: OLS on the same (Z), K-fold OOF predictions.
* **Second stage (LIML L1-only)**: maximizes the **LIML log-likelihood** conditional on (\widehat\Sigma_{\eta\eta}), estimating (\beta), (\sigma_v), (\lambda_c), and (\Sigma_{\eta v}) via (\Sigma_{\eta\eta}^{-1}\Sigma_{\eta v}).
* **SFA distributional assumptions**: (v\sim\mathcal N(0,\sigma_v^2)), (u\sim | \mathcal N(0,\sigma_u^2) |), (y = X\beta + v - u).
* **SEs**: Outer-product-of-gradients (**OPG**) for (\beta) blocks.

---

## 🧪 Reproducibility switches

* `BASE_SEED` controls NumPy/Torch RNG.
* `USE_SYMMETRIC_SHRINK` + `RHO_SHRINK` stabilize (\widehat\Sigma_{\eta\eta}) (Ledoit–Wolf-style ridge to identity).
  Use this **symmetrically** for linear and NN to keep the comparison fair.
* When toggling YEAR effects **in X**, keep YEAR in **Z** so instruments remain rich and comparable.

---

## 📊 Typical outputs

* **OOF (R^2)**: NN first stage usually improves by **3–8 pts** over OLS (varies by regressor; COWS/FEED often gain most).
* **Structural elasticities**: LIML models (Linear/NN) shift coefficients relative to MLE when endogeneity is present; **NN-LIML** can shrink bias further when (Z\to X_2) mapping is nonlinear.
* **Variance components**: (\sigma_c) (composite symmetric part), (\lambda=\sigma_u/\sigma_c), (\sigma_u^2), (\sigma_v^2), **TE mean**.

> If your NN-LIML diverges from expectations, check: (1) YEAR in **X** vs only in **Z**, (2) (\widehat\Sigma_{\eta\eta}) conditioning (turn on shrink), (3) NN cross-fit is truly OOF, (4) instrument list consistency across models, (5) scaling/standardization.

---

## 📄 Data

* Example file: `SPANISH DATASET.xlsx` (columns used in notebooks are listed in-notebook).
* Required columns (core): `MILK, LABOR, COWS, FEED, LAND, DEPREC, FARMEXP, YEAR, COOP, PMILK, PFEED, LANDOWN` (+ optional `COUNTY`, `ZONE`).
* Derived: `ROUGHAGE = FARMEXP + DEPREC`.

> ⚠️ If you use a larger/full dataset, keep it out of Git and set the path at the top of the notebook.

---

## 🛠 Troubleshooting

* **SEs look unstable** → enable `USE_SYMMETRIC_SHRINK=True`, set `RHO_SHRINK∈[0.1,0.3]`.
* **NN doesn’t outperform OLS** → increase `NN_EPOCHS`, tune dropout, check normalization, verify cross-fit is OOF (no leakage).
* **Large gaps between Est and St.Err columns in LaTeX** → tighten LaTeX column spacing (`\tabcolsep`) or use `S` columns from `siunitx`.
* **Different results from paper** → verify YEAR in X vs Z, and that both first-stage models use identical (Z).

---

## 🔗 How to cite (example)

If you use this code in academic work, please cite the repo:

```
Sandeep (2025). Neural_Network_LIML_Model: Parity-fair comparison of MLE, LIML, and NN-LIML.
GitHub repository: https://github.com/sandeep7299/Neural_Network_LIML_Model
```

(And cite standard SFA / control-function / LIML references as appropriate.)

---

## 📫 Contact

Questions or bugs? Open a GitHub **Issue** or ping me via the email on my profile.

---

**Pro tip:** add screenshots/plots to an `assets/` folder and reference them in the README:

```markdown
![Unified Est & SE table](assets/est_table.png)
![First-stage OOF R^2](assets/first_stage_r2.png)
```

---
