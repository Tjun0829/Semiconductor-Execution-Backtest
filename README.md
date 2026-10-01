# Setup

Requires Git and Python 3.10+ (check with `python3 --version`; older versions are past their supported window and VS Code/Jupyter will warn about them).

```bash
cd <local-path>   #DO NOT COPY AND PASTE THIS REPLACE LOCAL PATH WITH YOUR PATHWAY TO THE FOLDER

python3 -m venv .venv --prompt semibacktest
source .venv/bin/activate    # Windows: .venv\Scripts\activate.bat (cmd) / Activate.ps1 (PowerShell)
pip install -r requirements.txt
```

**Each session** (new terminal, not already in the folder):
```bash
cd <local-path>              # same replacement as above
source .venv/bin/activate
```
Prompt should show `(semibacktest)`. Already `cd`'d in? Just run the `source` line.

**Notebooks:** set kernel to `semibacktest`. If missing, run once (env active):
`python -m ipykernel install --user --name semibacktest --display-name "Python (semibacktest)"`

**Troubleshooting:** `ModuleNotFoundError` → env not activated or wrong kernel. New packages added → re-run `pip install -r requirements.txt`.

# Pipeline

Run the notebooks in this order. Each one reads the CSV the previous one wrote.

| Step | Notebook | Reads | Writes | What it does |
|---|---|---|---|---|
| 1 | `dataCollection.ipynb` | Yahoo Finance | `semiconductor_data.csv` | Downloads 2 years of daily prices for 20 semiconductor tickers and cleans them into one table. |
| 2 | `signal.ipynb` | `semiconductor_data.csv` | `signal_picks.csv` | Ranks tickers by 20-day momentum each week; top 5 go long, bottom 5 go short. |
| 3 | `execution.ipynb` | `signal_picks.csv` | `execution_fills.csv` | Sizes each pick at equal dollars and simulates the fill price under all-at-once vs. TWAP, with a square-root market-impact model. |
| 4 | `pnl.ipynb` | `execution_fills.csv` | `pnl_*.csv`, `pnl_equity_curve.png` | Holds each position for the week, closes it, and reports returns, Sharpe, drawdown and execution drag, gross vs. net. |

`CAPITAL_BASE` is set in both `execution.ipynb` and `pnl.ipynb` and must match.

To run everything from the command line (env active):
```bash
jupyter nbconvert --to notebook --execute --inplace dataCollection.ipynb signal.ipynb execution.ipynb pnl.ipynb
```
