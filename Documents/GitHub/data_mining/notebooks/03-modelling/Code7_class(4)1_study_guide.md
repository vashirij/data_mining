# Study Guide — Code7_class(4)1.ipynb (Advertising / Regression)

This notebook demonstrates a classic advertising dataset workflow: EDA, correlation inspection, simple and multiple linear regression, regularization (Ridge, Lasso), polynomial features, and evaluation/visualization.

## Learning goals

- Load and inspect a tabular dataset (Advertising.csv).
- Visualize relationships between predictors (TV, Radio, Newspaper) and target (Sales).
- Compute correlations and interpret heatmaps and scatterplots.
- Fit simple linear regression (single feature) and multiple linear regression.
- Evaluate model performance (R^2, MSE) and compare train/test scores.
- Apply regularization (Ridge, Lasso) and polynomial feature expansion to address overfitting and multicollinearity.

## Dataset

- Typical columns used in the notebook: `TV`, `Radio`, `Newspaper`, `Sales`.
- The notebook reads `Advertising.csv` and selects these columns.

## Walkthrough (key steps / cells)

1. Imports & data load

- Libraries used: matplotlib, pandas, numpy, seaborn (commented), scikit-learn, statsmodels.
- Read data:

  ```python
  adv = pd.read_csv('Advertising.csv', usecols=[1,2,3,4])
  adv.head()
  ```

2. Quick shape & correlation

- `adv.shape` shows dataset size.
- `adv.corr()` returns Pearson correlations.
- Visualize with heatmap: `sns.heatmap(adv.corr(), annot=True)` to find which features correlate with `Sales`.

3. Pairplot & scatter

- Pairwise scatterplots (pairplot or layered scatter) help spot linear relationships and outliers.
- Example scatter overlay:

  ```python
  plt.scatter(x=adv.TV, y=adv.Sales, ...)
  plt.scatter(x=adv.Radio, y=adv.Sales, ...)
  plt.scatter(x=adv.Newspaper, y=adv.Sales, ...)
  ```

4. Simple linear regression (Radio → Sales)

- Prepare X, y:

  ```python
  X = adv.Radio.values.reshape(-1,1)
  y = adv.Sales.values
  ```

- Train/test split:

  ```python
  X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
  ```

- Fit linear regression and report coefficients, intercept, R^2 (train & test):

  ```python
  lin_reg = LinearRegression().fit(X_train, y_train)
  lin_reg.coef_, lin_reg.intercept_, lin_reg.score(X_train, y_train)
  ```

- Predict and compute MSE:

  ```python
  y_pred = lin_reg.predict(X_test)
  mean_squared_error(y_test, y_pred)
  ```

5. Multiple linear regression (Radio + TV)

- Use `X = adv[['Radio','TV']]` and follow the same split / fit / report flow. Use `statsmodels.OLS` for a detailed summary or scikit-learn's `LinearRegression` for quick training.

6. Full features & per-feature OLS

- Fit OLS models using different subsets (all features, each single feature) to compare explanatory power.

7. Ridge regression

- Standardize features before Ridge. Example pipeline:

  ```python
  scaler = StandardScaler().fit(X_train)
  X_train_scaled = scaler.transform(X_train)
  X_test_scaled = scaler.transform(X_test)
  reg_ridge = Ridge(alpha=100.0).fit(X_train_scaled, y_train)
  ```

- Inspect coefficients and R^2 to see regularization effects.

8. Lasso

- Similar to Ridge but encourages sparse coefficients. Use `MinMaxScaler` or `StandardScaler` before Lasso. Examine number of non-zero features to see feature selection in action.

9. Polynomial features

- Use `PolynomialFeatures(degree=2)` to expand inputs, then fit a linear model on the expanded features. Note risk of overfitting; combine with Ridge to mitigate.

## Key concepts explained briefly

- Correlation vs causation: correlation shows association but doesn't prove causal effect.
- Train/Test split: ensures evaluation on unseen data; use `random_state` for reproducibility.
- R-squared (R^2): proportion of variance explained. Higher is better but can overfit.
- Mean squared error (MSE): lower is better for regression error.
- Regularization (Ridge/Lasso): penalize large coefficients to reduce overfitting; Lasso can zero-out features.
- Polynomial expansion: adds interaction and nonlinear terms; increases feature dimensionality.

## Exercises

1. Inspect correlations and decide which single feature is the best univariate predictor for `Sales`.

2. Fit a multiple regression using all three features (`TV`, `Radio`, `Newspaper`). Compare coefficients and interpret signs.

3. Try Ridge with several `alpha` values (e.g., 0.1, 1, 10, 100). Plot test R^2 vs alpha and identify a good alpha.

4. Fit Lasso and list which features (if any) get zero coefficients. Compare performance to Ridge.

5. Create polynomial features degree=2 with `TV` and `Radio` only. Fit Ridge on these features and compare test R^2 to the linear model.

## Hints / Answers (short)

1. Use `adv.corr()['Sales'].sort_values(ascending=False)` — the top value indicates the strongest linear association.

2. Run `LinearRegression().fit(X_train_all, y_train)` and inspect `lin_reg.coef_` — positive coefficient means that feature increases predicted sales.

3. Use a loop over alphas and `reg_ridge = Ridge(alpha=a).fit(...)` then record `reg_ridge.score(X_test_scaled, y_test)`.

4. Lasso: after fitting, `np.sum(linlasso.coef_ != 0)` returns number of non-zero features.

5. Use `PolynomialFeatures(degree=2)` and `Ridge()`; compare `score()`.

## Practical tips

- Always scale features before regularized regression.
- Check residuals for heteroscedasticity or non-linearity — if present, consider transforming features or trying other models.
- Use `statsmodels` OLS summary to inspect p-values and confidence intervals for coefficients when you need interpretability.
- For reproducible experiments, set `random_state` and log parameters.

## Where to go next

- Try tree-based models (RandomForest, XGBoost) and compare feature importance to linear coefficients.
- Build a small cross-validation loop to compare models properly (use `cross_val_score` or `GridSearchCV`).
- Add simple unit tests for data-loading and preprocessing pipelines.

---

If you want, I can commit this study guide and push it to the repository now, or insert it as a markdown cell at the top of the notebook itself. Which do you prefer?