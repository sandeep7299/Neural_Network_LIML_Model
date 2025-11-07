# Census Automation — README

Automates a search on **data.census.gov**, navigates to **S0101 (Age and Sex)** for a target geography, opens **More Tools → Download**, selects a format (CSV by default), and kicks off the file download. The script also handles the occasional survey modal and captures screenshots to help debug.

---

## 1) Prerequisites

- **Python**: 3.10 or newer (3.11+ recommended)
- **Google Chrome** (current stable)
- **OS**: Windows / macOS / Linux

> The script uses `webdriver-manager` to fetch the correct ChromeDriver automatically—no manual driver install needed.

---

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

---

## 3) Run

From inside the project folder:

```bash
python your_script_name.py
```

> Replace `your_script_name.py` with your actual filename (e.g., `census_automation.py`).  
> The script launches Chrome, runs the workflow, and triggers a CSV download by default.

---

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

---

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

---

## 6) Troubleshooting

- **The survey modal blocks the page**  
  Script tries to close it automatically. If it persists, increase wait times or re-run.

- **Element not found / click intercepted**  
  The site is dynamic. Keep waits generous. The script already uses a mix of Selenium and JS clicks.  
  You can bump timeouts in:
  - `click_more_tools(...)`
  - `click_download_from_more_tools(...)`
  - `select_format_and_download(...)`

- **Chrome/driver mismatch**  
  `webdriver-manager` usually handles this. If Chrome just updated, re-run:
  ```bash
  pip install -U webdriver-manager
  ```

- **Warnings like `DEPRECATED_ENDPOINT`**  
  These come from Chrome internals (GCM) and can be ignored—they don’t affect the workflow.

- **No downloaded file**  
  - Confirm the **Download** dialog actually appeared (`download_dialog_open.png`)
  - Check your **Downloads** folder and Chrome download prompt settings
  - Try a different format: `fmt="excel"` or `fmt="zip"`

---

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

---

## 8) Notes

- This script is intended for **automated retrieval of publicly available data**.  
  Be respectful of site load and terms of use.
- If you want CLI arguments (e.g., `--geo`, `--fmt`) later, we can add a tiny wrapper without touching your core logic.

---

## 9) Quick FAQ

**Q: Can I copy just `main()` to a new file and run it?**  
A: Yes—as long as you also copy the helper functions it calls (`make_driver`, `close_survey`, `wait_for_search_results`, `click_more_tools`, `click_download_from_more_tools`, `select_format_and_download`) and the imports/constants at top.

**Q: Where do screenshots go?**  
A: In the working directory (same folder you run the script from).
