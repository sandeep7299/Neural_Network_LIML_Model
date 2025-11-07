# Census Automation — README

Automates a search on **data.census.gov**, navigates to **S0101 (Age and Sex)** for a target geography, opens **More Tools → Download**, selects a format (CSV by default), and kicks off the file download. The script also handles the occasional survey modal and captures screenshots to help debug.


## 1) Prerequisites


> The script uses `webdriver-manager` to fetch the correct ChromeDriver automatically—no manual driver install needed.


## 2) Setup

### A. Clone/copy the project
Place the script files in a folder (e.g., `census-automation/`).

### B. Create a virtual environment (recommended)

**Windows (PowerShell)**
```powershell
python -m venv .venv
.venv\Scripts\Activate
```

**macOS / Linux**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### C. Install dependencies

If you have a `requirements.txt`:
```bash
pip install -r requirements.txt
```

Or install directly:
```bash
pip install selenium webdriver-manager
```


## 3) Run

From inside the project folder:

```bash
python your_script_name.py
```

> Replace `your_script_name.py` with your actual filename (e.g., `census_automation.py`).  
> The script launches Chrome, runs the workflow, and triggers a CSV download by default.


## 4) What the script does

1. Opens `https://data.census.gov/`
2. Types **“Corpus Christi CCD, Nueces County, Texas”** (default) into the main search bar and submits
3. Waits for search results and navigates to **ACSST5Y2023.S0101**
4. Opens **More Tools → Download**
5. Selects **CSV** (default) and clicks **DOWNLOAD DATA**
6. Saves helpful screenshots:
   - `search_results.png` – results page verification
   - `download_dialog_open.png` – confirms the download dialog is open
   - On errors: `search_error_<timestamp>.png` and `page_source_<timestamp>.html`

**Download location:** your browser’s default download folder (e.g., `~/Downloads` or `C:\Users\<you>\Downloads`).


## 5) Configuration (edit in the script)

At the top of the file, you’ll find these knobs:

```python
HOME = "https://data.census.gov/"
GEO_QUERY = "Corpus Christi CCD, Nueces County, Texas"   # change the geography
```

Inside `main()`, the code navigates to:
```python
table_id = "ACSST5Y2023.S0101"                           # change the table if needed
```

To change the download format, update the call:
```python
select_format_and_download(driver, fmt="csv")            # "csv" | "excel" | "zip"
```


## 6) Troubleshooting

  Script tries to close it automatically. If it persists, increase wait times or re-run.

  The site is dynamic. Keep waits generous. The script already uses a mix of Selenium and JS clicks.  
  You can bump timeouts in:
  - `click_more_tools(...)`
  - `click_download_from_more_tools(...)`
  - `select_format_and_download(...)`

  `webdriver-manager` usually handles this. If Chrome just updated, re-run:
  ```bash
  pip install -U webdriver-manager
  ```

  These come from Chrome internals (GCM) and can be ignored—they don’t affect the workflow.

  - Confirm the **Download** dialog actually appeared (`download_dialog_open.png`)
  - Check your **Downloads** folder and Chrome download prompt settings
  - Try a different format: `fmt="excel"` or `fmt="zip"`


## 7) Project structure (suggested)

```
census-automation/
├─ your_script_name.py
├─ README.md
└─ requirements.txt
```

**requirements.txt**
```
selenium>=4.21
webdriver-manager>=4.0
```


## 8) Notes

  Be respectful of site load and terms of use.


## 9) Quick FAQ

**Q: Can I copy just `main()` to a new file and run it?**  
A: Yes—as long as you also copy the helper functions it calls (`make_driver`, `close_survey`, `wait_for_search_results`, `click_more_tools`, `click_download_from_more_tools`, `select_format_and_download`) and the imports/constants at top.

**Q: Where do screenshots go?**  
A: In the working directory (same folder you run the script from).

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

Contributing / next steps
- Add a `LICENSE` to make reuse explicit (e.g., MIT).
- If you want, I can add a small `run_simulations.py` script to run the notebook code from the command line and save outputs.

Acknowledgements
This code is intended for reproducible simulation and method comparison. If you use it in a paper or report, please cite appropriately.

If you'd like additional edits (short README section per-figure, add LICENSE, or reorganize the repo), tell me which change and I will update and push it.
