# PhishGuard-ML

**Phishing Website Prediction using Machine Learning**

A supervised machine learning project exploring how website-related features can be used to distinguish between two classes of websites. The notebook implements four ensemble classifiers, compares their predictive performance, and examines feature importance to understand which attributes contribute to model decisions.

## What this project demonstrates

This project goes beyond training a single classifier. It explores the complete workflow of a tabular machine learning problem—from understanding the dataset and examining feature distributions to preprocessing, model evaluation, and feature importance analysis.

Working through the notebook involves several practical ML considerations: separating features from the target, fitting preprocessing only on training data, evaluating different ensemble algorithms on the same test split, and interpreting multiple metrics rather than relying on accuracy alone.

## ML Workflow

The notebook follows this pipeline:

1. **Data loading and inspection** — Load the CSV dataset using Pandas and inspect its shape, columns, data types, and descriptive statistics.
2. **Exploratory Data Analysis** — Analyze target-class distribution, feature histograms, correlations, class-wise feature means, missing values, and potential outliers.
3. **Feature selection** — Remove the `index` column and separate `Result` as the target, leaving 30 input features.
4. **Train–test split** — Reserve 20% of the dataset for evaluation using `random_state=42`.
5. **Feature scaling** — Apply `StandardScaler`, fitting it on the training features before transforming the training and test sets.
6. **Model training** — Train Random Forest, XGBoost, Gradient Boosting, and AdaBoost classifiers.
7. **Model evaluation** — Compare accuracy, precision, recall, F1-score, classification reports, confusion matrices, and ROC-AUC where calculated.
8. **Feature importance** — Rank and visualize the top 15 features reported by each fitted ensemble model.

## Dataset

| Property | Details |
|---|---|
| Dataset size | 11,055 rows |
| Total columns | 32 |
| Input features | 30 |
| Target column | `Result` |
| Train–test split | 80:20 |
| Test samples | 2,211 |
| Random seed | 42 |

The features describe website and URL characteristics, including IP-address usage, URL length, URL shortening, HTTPS-related indicators, domain registration length, redirects, iframe usage, web traffic, and page rank.

The notebook loads the dataset from the Kaggle input path:

`/kaggle/input/phishing-website-dataset/dataset.csv`

The dataset is not included in this repository. Its source and the exact mapping of target labels to phishing and legitimate classes should be verified against the original dataset documentation.

## Tech Stack

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| Jupyter Notebook | Interactive experimentation and analysis |
| Pandas | Dataset loading, inspection, and manipulation |
| NumPy | Numerical operations |
| Matplotlib | Data visualization |
| Seaborn | Statistical plots, heatmaps, and confusion matrices |
| Scikit-learn | Data splitting, scaling, classification, and evaluation metrics |
| XGBoost | Gradient-boosted tree classification |

## Models Implemented

### 1. Random Forest

An ensemble of decision trees trained using bootstrap sampling and feature randomness.

Configuration: `n_estimators=100`, `random_state=42`.

### 2. XGBoost

A gradient-boosted tree model trained sequentially to improve predictions from previous boosting stages.

Configuration: `n_estimators=200`, `max_depth=5`, `learning_rate=0.1`, `subsample=0.8`, `colsample_bytree=0.8`, `eval_metric='logloss'`, `random_state=42`.

The notebook remaps the target label `-1` to `0` for XGBoost.

### 3. Gradient Boosting

A boosting ensemble that adds decision trees sequentially to improve the overall model.

Configuration: `n_estimators=200`, `learning_rate=0.1`, `max_depth=3`, `random_state=42`.

### 4. AdaBoost

An adaptive boosting classifier that adjusts the emphasis on training examples as successive weak learners are fitted.

Configuration: `n_estimators=200`, `learning_rate=0.1`, `random_state=42`.

## Model Performance

The following results are recorded in the notebook for the same 2,211-sample test set.

| Model | Accuracy | Precision | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Random Forest | 96.70% | 96.32% | 97.93% | 97.12% | 96.51%* |
| XGBoost | 96.65% | 96.17% | 98.01% | 97.08% | 99.51% |
| Gradient Boosting | 95.97% | 95.69% | 97.29% | 96.48% | 99.27% |
| AdaBoost | 92.40% | 91.33% | 95.70% | 93.46% | 98.11% |

*The Random Forest ROC-AUC was calculated using hard class predictions rather than prediction probabilities, so it is not directly comparable with the probability-based ROC-AUC values of the other models. It should be recalculated using `predict_proba`.*

**Initial observation:** Random Forest achieves the highest recorded accuracy, while XGBoost achieves the highest recorded recall and F1-score among these four models. These metrics use the notebook's positive-class convention and should not be interpreted as phishing-specific detection rates until the target-label mapping is verified.

## Evaluation and Interpretability

The notebook includes:

- Classification reports with class-wise precision, recall, and F1-score.
- Confusion matrices for each classifier.
- ROC-AUC calculations.
- Feature-importance rankings for all four ensemble models.
- Bar charts of the 15 highest-ranked features per model.

These analyses provide complementary views of model performance and help identify which input features the fitted models rely on. Feature importance indicates predictive contribution within a model; it does not establish causation.

## Running the Project

### Prerequisites

Python 3 and the libraries listed below are required.

### 1. Clone the repository

```bash
git clone https://github.com/bony-thephenomenon/PhishGuard-ML.git
cd PhishGuard-ML
```

### 2. Install dependencies

```bash
pip install numpy pandas matplotlib seaborn scikit-learn xgboost jupyter
```

### 3. Obtain the dataset

Obtain the dataset from its original source and update the CSV path in the notebook to match your local environment.

### 4. Run the notebook

```bash
jupyter notebook
```

Open `phishing-website-prediction-using-machine-learning.ipynb` and execute the cells in order.

## Engineering Considerations and Limitations

- The evaluation uses one random train–test split rather than cross-validation.
- The notebook does not include systematic hyperparameter tuning.
- Standard scaling is applied even though tree-based models generally do not require feature scaling.
- Missing values are inspected, but no separate imputation procedure is implemented.
- Potential outliers are visualized, but no separate outlier-removal procedure is implemented.
- The notebook's Random Forest ROC-AUC calculation needs correction for a fair comparison.
- Dataset provenance, label semantics, and performance on unseen or newer websites need further verification before practical deployment.

## Future Improvements

- Add stratified cross-validation and systematic hyperparameter tuning.
- Correct and standardize ROC-AUC calculations using prediction probabilities.
- Evaluate phishing-specific recall, precision, and false-negative rates after verifying the target labels.
- Save the trained model and preprocessing configuration for reproducible inference.
- Evaluate generalization on a separate dataset containing newer website examples.
- Build a small prediction API or web interface as a separate extension to the notebook.

## Disclaimer

This project is intended for educational and research purposes. A model prediction is not a guarantee that a website is safe or malicious.

## Author

**Bony Biswas**

GitHub: [@bony-thephenomenon](https://github.com/bony-thephenomenon)