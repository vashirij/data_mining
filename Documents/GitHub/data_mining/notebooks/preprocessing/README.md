# Preprocessing Notebook(s)

This folder contains the preprocessing notebooks for the Data Mining lecture notes. It includes examples for concatenation, merging, and melting (tidy data) using example datasets.

Files:

- `preprocessing_4.ipynb` — Notebook demonstrating dataframe concatenation, merging, and tidy-data operations. The notebook references `nba_salaries.csv` which is expected to be found in the repository root (or update the path within the notebook).

Quick notes:

- Run the notebook cells in order. If you get a FileNotFoundError when reading `nba_salaries.csv`, either move the CSV to the repository root or update the path in the cell that loads the CSV to the correct location.
- If you see errors dropping the `PLAYER` column, inspect `nba.columns` to confirm column names.

Suggested follow-ups:

- Add a `requirements.txt` and/or `environment.yml` for reproducibility.
- Add a short README at repository root pointing to this preprocessing folder (optional).
