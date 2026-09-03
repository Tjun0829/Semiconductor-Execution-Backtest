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
