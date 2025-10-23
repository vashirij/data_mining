# Study Guide — preprocessing_3.ipynb

This study guide walks through the key topics covered in `preprocessing_3.ipynb` (Introduction to Python, NumPy basics, and Pandas fundamentals). Use this as a compact reference and practice resource.

## Learning goals

- Understand basic Python expressions, variables, and built-in functions.
- Work with date/time modules (`datetime`, `time`).
- Know Python data types: str, float, int, list, tuple, dict.
- Write small functions and use lambdas.
- Iterate with loops and use list indexing and slicing.
- Use NumPy to create arrays, shapes, and perform elementwise operations.
- Build and query Pandas Series and DataFrames; select, filter, and aggregate data.
- Read CSV files and perform simple preprocessing (drop columns, sort, filter, groupby).

## Walkthrough (by notebook section)

1) Python basics

- Expressions and variables:
  - `hours_in_a_week = 24 * 7`
  - Use `print()` or evaluate the variable to inspect.
- Built-ins: `abs()`, `max()`, `min()`.
- Date/time: `import datetime as dt; import time as tm`.
  - `tm.time()` gives epoch seconds; `dt.date.today()` returns current date.

2) Types and collections

- Strings vs numbers: `type('hi')`, `type(1000)`, `type(1000.0)`.
- Lists (mutable) and tuples (immutable): `u = [1,2,3]`, `v = (1,2,3)`.
- Dicts to Series: `pd.Series({'student1':'Matt', ...})`.

3) Functions and lambdas

- Named functions:
  ```python
  def adding(x,y,z=None):
      if z is None:
          return x+y
      return x+y+z
  ```
- Lambdas: `f2 = lambda a,b: max(a,b)`.
- Assign functions to variables: `f = adding; f(6,3)`.

4) Loops and indexing

- List indexing: `x[0]`, `x[-1]`.
- For loops and accumulation patterns; be careful about variable shadowing (don't name variables `sum`).

5) NumPy essentials

- Create arrays: `np.array`, `np.arange`, `np.linspace`, `np.zeros`, `np.ones`, `np.eye`.
- Shapes and length: `arr.shape`, `len(arr)`.
- Elementwise ops: `nr**2` squares elements.
- Matrix slicing and boolean assignment: `a[a < 1] = 0`.

6) Pandas: Series and DataFrame

- Create Series: `pd.Series(names)` or from dicts.
- Indexing Series: `u.loc['student1']`, `u.iloc[2]`.
- Create DataFrames from Series list and use `df.loc[...]`, `df.T`.
- Copy before modifying: `copy_df = df.copy()` then mutate safely.
- Conditional selection:
  - `df[df['Final score']>85]` or `df.where(df['Final score']>85).dropna()`.

7) Reading and inspecting CSVs

- `flowers = pd.read_csv('flowers.csv')` then inspect columns `flowers.columns` and specific columns like `flowers['Petals']`.
- Dropping columns: `df2 = flowers.drop(['Color'], axis=1)` doesn't modify original unless assigned back.

8) Sorting and groupby

- `movies.sort_values('Gross', ascending=False)` sorts by a column.
- `movies.groupby('Studio').agg({'Gross': np.average})` aggregates per group.
- Use `groupby().size()` or `groupby().agg(...)` for counts and summaries.

9) NBA salaries example

- Load the dataset: `nba = pd.read_csv('nba_salaries.csv')`.
- Filter rows, sort, or set index:
  - `nba.where(nba['SALARY ($M)']>10).dropna()`
  - `nba.where(nba['PLAYER']=='LeBron James').dropna()`
  - `nba1 = nba.where(nba['TEAM']=='New York Knicks').dropna()`
- Use `df.copy()` before destructive operations and `df.rename(columns={...})` to rename columns.

## Common pitfalls & tips

- File paths: If you get FileNotFoundError, confirm the CSV is in the notebook working directory, or use an absolute path such as `/workspaces/data_mining/nba_salaries.csv`.
- Dropping columns: wrap in a conditional to avoid KeyError:
  ```python
  if 'PLAYER' in nba.columns:
      nba1 = nba.drop(['PLAYER'], axis=1).head(4)
  else:
      nba1 = nba.head(4)
  ```
- Avoid naming variables `sum` or `list` which shadow built-ins.
- After `where(...)` use `.dropna()` to remove rows that don't match the condition.

## Exercises (practice with answers)

1) Inspect `flowers.csv` and print the first 5 rows. Answer: `pd.read_csv('flowers.csv').head()`.

2) Create a DataFrame of student scores and select students with scores > 85. Answer:
```python
students = pd.DataFrame({
    'Name': ['Matt','Jack','Nancy'],
    'Score': [92,88,84]
})
students[students['Score']>85]
```

3) Load `nba_salaries.csv` and find the player with the maximum salary. Answer:
```python
nba = pd.read_csv('nba_salaries.csv')
max_salary = nba['SALARY ($M)'].max()
nba.loc[nba['SALARY ($M)'] == max_salary]
```

4) Using the `movies` DataFrame, compute the average gross per studio and plot the top 10. Answer:
```python
avg = movies.groupby('Studio')['Gross'].mean().sort_values(ascending=False).head(10)
avg.plot(kind='bar')
```

5) Convert a small wide DataFrame to long format using `pd.melt` and back with `pivot_table` (see notebook example).

## Commands (environment)

```bash
python -m venv .venv
source .venv/bin/activate
pip install pandas numpy matplotlib jupyter
jupyter lab
```

## Where to go next

- Turn notebook sections into small unit tests that run via `nbconvert --to notebook --execute` to validate outputs.
- Add a `requirements.txt` for reproducibility.

---

If you'd like, I can commit this study guide in the preprocessing folder and push it to GitHub (I will).