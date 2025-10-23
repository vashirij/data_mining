# Study Guide — intro_2.ipynb

This study guide covers the material in `intro_2.ipynb` (Python fundamentals, NumPy basics, and an introduction to Pandas Series/DataFrames).

## Learning goals

- Reinforce Python basics: expressions, variables, and built-in functions.
- Use datetime/time utilities for timestamps.
- Create and manipulate NumPy arrays (`np.array`, `np.arange`, `np.linspace`, `np.zeros`, `np.ones`, `np.eye`).
- Understand array shapes and basic matrix slicing.
- Work with Pandas Series and DataFrames: creating Series, indexing with `.loc` and `.iloc`, and basic aggregations.

## Walkthrough (key examples)

1. Python basics & date/time

- Repeat of arithmetic, variable assignment, and `tm.time()`, `dt.date.today()` examples.

2. NumPy essentials

- Create arrays: `ar = np.array(x)` where `x` is a Python list.
- `np.arange(0,12,3)` produces `[0,3,6,9]`.
- Elementwise operations: `nr**2` squares elements.
- Useful constructors: `np.linspace`, `np.zeros`, `np.ones`, `np.eye`.
- Matrix operations and slicing, boolean indexing: `a[a < 1] = 0`.

3. Pandas Series

- Create Series from lists or dicts: `pd.Series(names)` or `pd.Series({'student1':'Matt',...})`.
- Indexing: `u.loc['student1']` (label) and `u.iloc[2]` (positional).
- Basic aggregation: `np.sum(u2)` on numeric Series.

4. DataFrame intro (preview)

- The notebook shows building a `DataFrame` from several `Series` (Course details) and using `df.head()`, `df.loc[...]`, and `df.T`.

## Exercises (practice)

1. Use NumPy to create a 3x3 identity matrix and replace the diagonal with 7.

2. Build a Pandas Series from a dict of your choosing and access an element using `.loc`.

3. Create a 1-D NumPy array and demonstrate slicing to get the last two elements.

Hints/Answers:

1. ```python
m = np.eye(3)
np.fill_diagonal(m, 7)
```
2. ```python
s = pd.Series({'a':1, 'b':2})
s.loc['a']
```
3. ```python
arr = np.array([1,2,3,4])
arr[-2:]
```

## Tips

- When working with NumPy, check `.shape` and use vectorized ops instead of Python loops for performance.
- With Pandas Series, prefer `.loc` for label-based indexing and `.iloc` for integer positions.

## Commands

```bash
python -m venv .venv
source .venv/bin/activate
pip install pandas numpy matplotlib jupyter
jupyter lab
```

---

If you want, I can add these study guides as markdown cells at the top of the corresponding notebooks.