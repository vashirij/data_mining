# Study Guide — intro_1.ipynb

This study guide covers the material in `intro_1.ipynb` (basic Python: expressions, types, functions, loops, and simple data structures).

## Learning goals

- Understand expressions, variables, and basic arithmetic in Python.
- Recognize and use common built-in functions (abs, max, min, len).
- Work with basic data types: int, float, str, list, tuple, dict.
- Define and call functions; use default arguments and return values.
- Use lambdas for small anonymous functions.
- Iterate over lists with for-loops and use indexing/slicing.
- Use the datetime and time modules for timestamps.

## Walkthrough (key examples)

1. Expressions and variables

- Arithmetic: `24 * 7`, `60 * 60 * 24 * 365` (seconds per year).
- Assignments: `hours_in_a_week = 24 * 7`.

2. Date/time utilities

- `import datetime as dt` and `import time as tm`.
- `tm.time()` returns epoch seconds; `dt.date.today()` returns today's date; `dt.datetime.fromtimestamp(tm.time())` gives current datetime.

3. Types and collections

- Strings and numbers: `type('hi')`, `type(1000.0)`, `type(1000)`.
- Lists and tuples: `u = [1,2,3]` (mutable), `v = (1,2,3)` (immutable). Append to lists with `u.append(4)`.
- Dictionaries to Series in pandas: later notebooks show `pd.Series({'student1':'Matt', ...})`.

4. Functions and lambdas

- Define a function with optional argument:

```python
def adding(x, y, z=None):
    if z is None:
        return x + y
    return x + y + z
```

- Use lambdas for short operations: `f2 = lambda a,b: max(a,b)`.

5. Loops and accumulating

- For loop example that increments items and computes a running sum.
- Avoid shadowing built-ins (don't use variable names like `sum` for production code).

## Exercises (practice)

1. Calculate the number of hours in 10 years and print the result.

2. Write a function that computes the factorial of a number using a loop.

3. Create a list of integers, use a for-loop to square each element and collect results in a new list.

Answers (brief):

1. `hours = 24 * 365 * 10`.
2. ```python
def factorial(n):
    result = 1
    for i in range(2, n+1):
        result *= i
    return result
``` 
3. ```python
xs = [1,2,3]
sq = [x**2 for x in xs]
```

## Tips

- Use `print()` while learning to inspect variables.
- Keep code cells small and focused for easier debugging.
- Use meaningful variable names.

## Commands (environment)

Run these if you need a fresh environment:

```bash
python -m venv .venv
source .venv/bin/activate
pip install pandas numpy matplotlib jupyter
jupyter lab
```

---

If you'd like, I can also convert this into a notebook cell (markdown) in `intro_1.ipynb`.