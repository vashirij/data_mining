# Data Mining — Example Notebooks

This repository contains example data and Jupyter notebooks used for a data mining / data cleaning lecture series. The notebooks demonstrate common dataframe operations such as concatenation, merging, melting (tidy data), and basic NumPy operations.

## Repository structure

- `nba_salaries.csv` — Example dataset of NBA salaries used in the notebooks.
- `Documents/GitHub/data_mining/notebooks/preprocessing_4.ipynb` — Jupyter notebook covering dataframe concatenation and merging, plus examples using the `nba_salaries.csv` file.

## Quick start

1. Open the repository in VS Code (or Jupyter Lab/Notebook).
2. Install dependencies (recommended to use a virtual environment):

```bash
python -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt || pip install pandas numpy matplotlib
```

3. Launch Jupyter and open the notebook(s):

```bash
# from repository root
jupyter lab  # or jupyter notebook
```

4. In `preprocessing_4.ipynb` run the cells in order. The notebook expects `nba_salaries.csv` to be in the repository root. If you get a FileNotFoundError, check the path in the notebook cell that loads the CSV.

## Notes & troubleshooting

- If `nba_salaries.csv` is not found, confirm it's located at the repository root. In the notebook, the cell reads it using an absolute path like `/workspaces/data_mining/nba_salaries.csv`. Modify the path if you moved the file.
- If you see errors about missing columns (for example, dropping `PLAYER`), inspect `nba.columns` to find the correct column names.

## Suggested improvements

- Add a `requirements.txt` with pinned package versions.
- Add a small test script to validate notebooks run end-to-end (e.g., `nbconvert` + execute).

## Contact

If this repository was shared with you, ask the author for dataset provenance and licensing details if you plan to reuse or publish results.
