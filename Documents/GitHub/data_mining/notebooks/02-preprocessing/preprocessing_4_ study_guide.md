# Preprocessing Study Guide — Notebook: preprocessing_4.ipynb

This study guide accompanies `preprocessing_4.ipynb`. It highlights the learning goals, walks through key examples (concatenation, merging, melting, and NumPy concatenation), and provides exercises with hints and answers.

## Learning goals

- Understand how to concatenate DataFrames both row-wise and column-wise using `pd.concat` and `np.concatenate`.
- Learn different types of joins/merges (`inner`, `outer`, `left`, `right`) via `pd.merge` and how `left_on`/`right_on` work.
- Practice reshaping data from wide to long format using `pd.melt` (tidy data principles).
- Handle common preprocessing issues: missing files, missing columns, index alignment, and filling NaNs.

## Quick walkthrough (key cells)

1) Setup and small example DataFrames

- The notebook creates small `pd.Series` objects and builds `df1` and `df2` to demonstrate concatenation/merge behaviors.

2) Concatenation examples

- `pd.concat([df1, df2])` (default axis=0) stacks rows and aligns columns by name.
- `pd.concat([df1, df1], axis=1)` places DataFrames side-by-side (columns duplicated). Use `reset_index()` first if you want to combine by index position.

3) Working with the `nba_salaries.csv` example

- The notebook reads the CSV and drops the `PLAYER` column for some concatenation examples.
- If the file can't be found, adjust the path or move `nba_salaries.csv` to the repository root.

4) Merge examples

- `pd.merge(df3, df4, left_on='Name 1', right_on='Name 2')` shows using explicit keys.
- Experiment with `how='inner'`, `how='outer'` and `df6.fillna(method='pad')` to see how to handle missing values after an outer merge.

5) Melting (tidy data)

- The notebook loads `pew-raw.csv` and demonstrates `pd.melt(df, ['religion'], var_name='income', value_name='freq')` to convert wide to long format.

6) NumPy concatenation

- `np.concatenate([arr1, arr2], axis=0)` stacks arrays vertically; `axis=1` stacks horizontally.

## Exercises (practice)

1. Load `nba_salaries.csv` and print `nba.columns`. Identify if the `PLAYER` column exists. If not, find the correct column holding player names.

2. Using `nba.head(4)`, drop the player-name column safely (use a conditional check) and assign `nba1 = nba.drop(['PLAYER'], axis=1).head(4)` only if the column exists.

3. Concatenate `nba1` with itself along axis=1 and axis=0 — inspect the shapes and column/index alignment.

4. Create two DataFrames with a different set of columns and perform an outer merge. Then practice filling NaNs using forward-fill and backward-fill.

5. Take a small wide DataFrame (like survey counts per year) and use `pd.melt` to tidy it. Then reverse the operation with `pivot_table`.

## Hints and answers

- Exercise 1 hint: Use `print(nba.columns.tolist())`.

- Exercise 2 hint: Wrap the drop in `if 'PLAYER' in nba.columns:` to avoid KeyError. Example:

```python
if 'PLAYER' in nba.columns:
	nba1 = nba.drop(['PLAYER'], axis=1).head(4)
else:
	nba1 = nba.head(4)
	print("'PLAYER' not found; showing first 4 rows")
```

- Exercise 3 answer: `pd.concat([nba1, nba1], axis=1)` doubles columns; `pd.concat([nba1, nba1], axis=0)` stacks rows and doubles the number of rows.

- Exercise 4 hint: After `outer` merge, use `df.fillna(method='pad', inplace=False)` to forward-fill across rows where appropriate.

- Exercise 5 hint: Use `pd.melt(df, id_vars=[...], var_name='variable', value_name='value')` and reverse with `df.pivot_table(index=[...], columns='variable', values='value')`.

## Commands and environment

Recommended environment setup:

```bash
python -m venv .venv
source .venv/bin/activate
pip install pandas numpy matplotlib jupyter
```

Run the notebook:

```bash
jupyter lab  # or jupyter notebook
```

## Next steps (stretch)

- Add a `requirements.txt` with pinned package versions and push it.
- Add small unit tests that use `nbconvert` to execute `preprocessing_4.ipynb` and validate key outputs (e.g., `nba1.shape`).

If you'd like, I can commit this updated README for you and push the change.
