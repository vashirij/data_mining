# Data Mining — Project Guide (CRISP‑DM)

This repository contains example datasets and notebooks used to teach data preprocessing and exploratory data analysis. The README below frames the work in terms of the CRISP‑DM (Cross-Industry Standard Process for Data Mining) lifecycle so you can follow an end-to-end workflow from business understanding through deployment and refinement.

## Table of contents

- Business understanding
- Core concepts in data mining
- Data collection & ingestion
- Data understanding & exploration
- Data preparation (preprocessing & scaling)
- Modeling and evaluation
- Deployment & monitoring
- Repository structure and how to run
- Reproducibility, testing & next steps

## Core concepts in data mining

Below are concise explanations of key topics you should understand and be able to apply in practical projects.

1. Data pre-processing

	- Data acquisition: methods to collect and ingest data (APIs, scraping, batch exports, sensors). Capture provenance (source, timestamp, query). Prefer storing raw snapshots.
	- Data labeling: approaches for supervised tasks — manual annotation, weak supervision, heuristics, and using existing labels. Track label quality and inter-annotator agreement.
	- Data cleaning: remove/handle duplicates, correct typos, standardize formats (dates, categorical names), remove outliers or correct them after domain checks.
	- Data transformation: type conversions, parsing complex fields, normalization, log transforms, one-hot encoding or ordinal coding for categories.
	- Feature engineering: create informative features by aggregating, extracting from text/dates, binning, polynomial interactions, or domain-specific encodings.

2. Classification and regression

	- Supervised learning tasks: classification for categorical targets and regression for continuous targets.
	- Common algorithms: logistic regression, decision trees, random forests, gradient-boosted trees (XGBoost/LightGBM/CatBoost), linear regression, neural networks.
	- Feature-target relationships: inspect feature importance, partial dependence plots, and residuals (for regression) to find biases.

3. Model validation and combination

	- Validation strategies: holdout splits, k-fold cross-validation, stratified sampling, and time-series-aware splits.
	- Metrics: choose metrics tied to the business problem (precision/recall/F1, AUC, RMSE, MAE, calibration).
	- Combination/ensembling: bagging, boosting, stacking, and simple model averaging to improve stability and performance.
	- Hyperparameter search: grid search, random search, and Bayesian optimization; use nested CV when tuning to avoid leakage.

4. Data reduction

	- Clustering: unsupervised grouping (KMeans, DBSCAN, hierarchical clustering) for segmentation and exploratory analysis.
	- Dimensionality reduction: PCA, t-SNE, UMAP for visualization and to reduce noise; beware information loss.
	- Feature selection: filter methods (correlation, mutual information), wrapper methods (recursive feature elimination), and embedded methods (L1 regularization, tree-based importance).

5. Association rule mining

	- Frequent itemset mining and association rules (Apriori, FP-Growth) to extract co-occurrence patterns (market-basket analysis).
	- Interpret rules with support, confidence, and lift; prune redundant and insignificant rules.
	- Applications: recommender systems, cross-sell analysis, and anomaly detection in transactional data.


## 1) Business understanding

Start every analysis by clarifying the business question and success criteria. Example prompts:

- What decision will the model or analysis enable? (e.g., prioritize players to recruit, forecast sales)
- What KPI(s) measure success? (accuracy, recall, revenue uplift)
- What constraints exist? (latency, data privacy, compute budget)

Document assumptions and sketch the end-to-end path from data to decision. Store this in `docs/` or a notebook cell so it's versioned with code.

## 2) Data collection & ingestion

- Sources: CSVs (e.g., `nba_salaries.csv`), public APIs, internal databases.
- Keep raw data immutable: store originals under `data/raw/` (or a cloud bucket) and never overwrite them.
- Use small ingestion scripts (e.g., `scripts/ingest.py`) that validate schema and write canonical CSV/Parquet in `data/processed/`.

Checklist:

- Record provenance (source, date pulled, query/URL).
- Validate types and required columns.
- Sanity-check value ranges and duplicates.

## 3) Data understanding & exploration

- Use Jupyter notebooks for EDA (exploratory data analysis). Notebooks in `Documents/GitHub/data_mining/notebooks/preprocessing/` show examples.
- Key steps: head/tail, describe, null counts, unique counts, distributions, and pairwise relationships.
- Visualize: histograms, boxplots, scatter plots, heatmaps for correlations.

Deliverable: an EDA notebook with findings, open questions, and quick feature ideas.

## 4) Data preparation (preprocessing & scaling)

This is the most time-consuming phase. Typical steps:

- Cleaning: drop or impute missing values, fix types, normalize inconsistent strings (lowercase, trim), remove duplicates.
- Feature engineering: parse dates, extract categories, aggregate granular data, encode categorical variables.
- Scaling: for many algorithms, scale numerical features (StandardScaler, MinMaxScaler) — store scalers using joblib or pickle.
- Pipeline: build a reproducible preprocessing pipeline (scikit-learn Pipelines, or a small function chain) so the same transformations run in training and production.

Example code patterns:

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler

preproc = Pipeline([
	('imputer', SimpleImputer(strategy='median')),
	('scaler', StandardScaler())
])
X_transformed = preproc.fit_transform(X_train)
```

Store preprocessed artifacts under `data/processed/` and persisting transformers under `models/`.

## 5) Modeling and evaluation

- Split data with a reproducible seed (train/val/test) and consider time splits for time-series problems.
- Try simple baselines first (mean predictor, logistic regression) before complex models.
- Evaluate with business-focused metrics. Use cross-validation and report variance.
- Keep experiments reproducible: log hyperparameters and metrics (MLflow, Sacred, or a simple CSV log).

Suggested structure:

- `notebooks/experiments/` — exploratory model notebooks
- `models/` — saved trained models and transformers

## 6) Deployment & monitoring

- Decide deployment pattern: batch job, REST API (FastAPI/Flask), or streaming.
- Containerize the model and dependencies with Docker. Include a simple predict endpoint that loads the saved transformers & model and accepts the same schema as training.
- Monitor: track input distributions, prediction drift, and model performance against ground truth. Set alerts for drift.

Quick Docker pattern (example):

Dockerfile:

```
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY ./service ./service
CMD ["uvicorn", "service.app:app", "--host", "0.0.0.0", "--port", "8080"]
```

## 7) Refinement

- Revisit business objectives and drift signals.
- Retrain on fresh data and increment version numbers. Maintain a clear release checklist for each model version.

## Repo structure (recommended)

```
.
├─ data/
│  ├─ raw/                # original unmodified data dumps
│  └─ processed/          # cleaned and transformed datasets used for modeling
├─ Documents/GitHub/data_mining/notebooks/preprocessing/  # teaching notebooks & study guides
├─ models/                # saved models and scalers (joblib/pickle)
├─ scripts/               # small scripts: ingest, train, evaluate, deploy
├─ service/               # example microservice for predictions (FastAPI)
├─ requirements.txt
├─ README.md
```

## How to run (quick start)

1. Create environment and install deps:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

2. Run EDA / preprocessing notebooks:

```bash
jupyter lab Documents/GitHub/data_mining/notebooks/preprocessing/
```

3. Run training script (example):

```bash
python scripts/train.py --config configs/train.yaml
```

4. Run the prediction service (example):

```bash
docker build -t mymodel:latest .
docker run -p 8080:8080 mymodel:latest
```

## Reproducibility & testing

- Pin versions in `requirements.txt`.
- Add unit tests for preprocessing functions and a CI job that runs them.
- For notebooks, use `nbconvert` to execute them as smoke tests in CI.

## Notes and further reading

- CRISP‑DM: https://www.the-modeling-agency.com/crisp-dm/
- MLflow for experiment tracking: https://mlflow.org/
- Scikit-learn Pipelines: https://scikit-learn.org/stable/modules/compose.html

----

If you want, I will commit this README, or I can tailor sections further (add example scripts, `requirements.txt`, or a template `train.py`).
