# PhishGuard-ML

A machine-learning project for classifying websites as **phishing** or **legitimate** using pre-extracted website security features. The notebook explores the dataset, trains four classification models, evaluates their performance, and compares feature importance.

## Overview

Phishing websites imitate trusted websites to trick people into revealing passwords, financial details, and other sensitive information. This project experiments with supervised machine-learning models that learn patterns from labelled website features.

> **Scope:** This notebook classifies rows of an existing, feature-based dataset. It does not currently extract features from a raw URL, expose an API, or provide a deployed website scanner.

## Dataset

- **File used by the notebook:** `dataset.csv`
- **Notebook path:** `/kaggle/input/phishing-website-dataset/dataset.csv`
- **Rows:** 11,055
- **Columns:** 32 in total — an `index` column, 30 feature columns, and the `Result` target column.
- **Target:** `Result`, with labels `-1` and `1`.

The dataset is attached to the Kaggle notebook as an input. The path above is a Kaggle runtime path, not a public URL that the Python code downloads directly. Kaggle makes attached datasets available under `/kaggle/input/`. See the [Kaggle Notebooks documentation](https://www.kaggle.com/docs/notebooks) for details.

The repository does not need to commit the dataset CSV; keep large dataset files out of Git unless there is a specific reason and permission to redistribute them.

## Workflow

1. Load the dataset with pandas.
2. Inspect the dataset shape, column names, data types, descriptive statistics, and missing values.
3. Explore the data with target-class counts, feature histograms, a correlation heatmap, grouped feature means, and boxplots.
4. Separate features (`X`) from the target (`y`), dropping `index` and using `Result` as the label.
5. Split the data into training and test sets using an 80/20 split and `random_state=42`.
6. Fit `StandardScaler` on the training features and transform both training and test features.
7. Train and evaluate four classifiers.
8. Plot confusion matrices and inspect feature importance for the tree-based models.

### Preprocessing Details

- **Feature/target separation:** `index` and `Result` are removed from the feature matrix; `Result` is the target.
- **Train/test split:** 80% training and 20% testing, with `random_state=42`.
- **Feature scaling:** `StandardScaler` is fitted on the training set and applied to the test set.
- **Missing values:** The notebook checks missing-value counts; the saved output shows zero missing values in all columns.
- **XGBoost labels:** For XGBoost only, target label `-1` is mapped to `0`, giving the model labels `0` and `1`.

The notebook does not currently implement a separate imputation step, categorical encoding step, hyperparameter search, or cross-validation.

## Models

| Model | Configuration in the Notebook |
|---|---|
| Random Forest | `n_estimators=100`, `random_state=42` |
| XGBoost | `n_estimators=200`, `max_depth=5`, `learning_rate=0.1`, `subsample=0.8`, `colsample_bytree=0.8`, `eval_metric="logloss"`, `random_state=42` |
| Gradient Boosting | `n_estimators=200`, `learning_rate=0.1`, `max_depth=3`, `random_state=42` |
| AdaBoost | `n_estimators=200`, `learning_rate=0.1`, `random_state=42` |

## Evaluation

The notebook evaluates the models using accuracy, precision, recall, F1-score, ROC-AUC, a classification report, and confusion matrices. The following values are taken from the notebook's saved outputs.

| Model | Accuracy | Precision | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Random Forest | 96.70% | 96.32% | 97.93% | 97.12% | 96.51%* |
| XGBoost | 96.65% | 96.17% | 98.01% | 97.08% | 99.51% |
| Gradient Boosting | 95.97% | 95.69% | 97.29% | 96.48% | 99.27% |
| AdaBoost | 92.40% | 91.33% | 95.70% | 93.46% | 98.11% |

\*The Random Forest ROC-AUC in the notebook is calculated from predicted class labels rather than predicted probabilities. For a more comparable ROC-AUC evaluation, calculate it from `predict_proba(... )[:, 1]` as done for the other models.

These are the results stored in the current notebook; they may change if the data, environment, or code changes. A high score on this dataset does not guarantee equivalent performance on unseen real-world websites.

## Exploratory Analysis and Feature Importance

The notebook includes:

- Target class distribution
- Feature histograms
- Feature correlation heatmap
- Mean feature values grouped by target
- Missing-value checks
- Boxplots for outlier inspection
- Feature-importance plots for Random Forest, XGBoost, Gradient Boosting, and AdaBoost

## Tech Stack

- Python
- Jupyter Notebook / Kaggle Notebooks
- pandas and NumPy
- Matplotlib and Seaborn
- scikit-learn
- XGBoost

## Run the Notebook

### On Kaggle

1. Open the notebook in Kaggle.
2. Attach the dataset containing `dataset.csv` through the **Add Input** control.
3. Confirm that the mounted file path matches `/kaggle/input/phishing-website-dataset/dataset.csv`.
4. Run the notebook cells from top to bottom.

### In a Local Environment

The `/kaggle/input/...` path exists in the Kaggle runtime and will not normally exist on a Windows computer. To run locally, download the dataset from its source and update the `pd.read_csv(...)` path to the local CSV location. The dataset is not downloaded automatically by the current code.

Install the main dependencies with:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost jupyter
```

Then launch Jupyter and open `phishing-website-prediction-using-machine-learning.ipynb`.

## Current Limitations

- Uses precomputed dataset features rather than extracting features from a submitted URL.
- No web app, REST API, or deployment is included in the current notebook.
- No cross-validation or hyperparameter tuning is implemented.
- Scaling is applied to tree-based models as well, although these models generally do not require standardization.
- The reported metrics are measured on one held-out split and should not be treated as a guarantee of production performance.

## Future Improvements

- Add cross-validation and systematic hyperparameter tuning.
- Compare ROC-AUC using predicted probabilities consistently across models.
- Save the best-performing model and preprocessing pipeline.
- Build a separate feature-extraction pipeline for raw URLs, then validate it on fresh, unseen data.
- Add an inference API or user interface only after the end-to-end pipeline is implemented.

## Repository Structure

```text
PhishGuard-ML/
├── phishing-website-prediction-using-machine-learning.ipynb
└── README.md
```

## Author

**Bony Biswas**

GitHub: [@bony-thephenomenon](https://github.com/bony-thephenomenon)
