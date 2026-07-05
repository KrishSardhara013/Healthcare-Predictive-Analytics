
# Healthcare Predictive Analytics — Diabetes Risk Detection

Predicting diabetes risk from clinical measurements using classification models, with a focus on
data quality, feature importance, and the ethical handling of patient data.

## Overview

This project builds and compares machine learning classifiers to predict diabetes risk from
routine clinical measurements (glucose, BMI, blood pressure, etc.), then interprets *which*
measurements drive that risk using feature importance analysis.

## Dataset

- **Source:** [UCI Machine Learning Repository — Pima Indians Diabetes Database](https://archive.ics.uci.edu/dataset/34/diabetes)
- **Records:** 768 female patients of Pima Indian heritage, age 21+
- **Features (8):** `Pregnancies`, `Glucose`, `BloodPressure`, `SkinThickness`, `Insulin`, `BMI`,
  `DiabetesPedigreeFunction`, `Age`
- **Target:** `Outcome` (1 = diabetic, 0 = non-diabetic)

## Project Structure

```
.
├── healthcare_diabetes_analysis.ipynb   # Main analysis notebook (code + narrative)
├── healthcare_diabetes_analysis_preview.html  # Browser-viewable export of the notebook
├── diabetes.csv                         # Raw dataset
└── README.md
```

## Methodology

1. **Data quality & cleaning** — Biologically impossible zero values (e.g. `Glucose = 0`,
   `BMI = 0`) are treated as missing data and imputed using class-wise medians, rather than
   trusting them as real measurements.
2. **Normalization** — Features are standardized (`StandardScaler`) so scale-sensitive models
   (Logistic Regression, SVM) aren't biased by features with larger numeric ranges.
3. **Exploratory Data Analysis** — Class balance, feature distributions by outcome, and a
   correlation heatmap.
4. **Modeling** — Four classifiers are trained and compared:
   - Logistic Regression
   - Random Forest
   - Gradient Boosting
   - SVM (RBF kernel)
5. **Evaluation** — Accuracy, Precision, Recall, F1, and ROC-AUC, validated with 5-fold
   stratified cross-validation; ROC curves and a confusion matrix for the best model.
6. **Feature importance** — Random Forest impurity-based importances and Logistic Regression
   coefficients, compared side by side.
7. **Ethics & privacy** — A dedicated discussion of consent, de-identification, bias/
   representativeness, explainability, and data security considerations for clinical use.

## Results

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Gradient Boosting | 0.883 | 0.833 | 0.833 | 0.833 | **0.957** |
| Random Forest | 0.864 | 0.824 | 0.778 | 0.800 | 0.946 |
| SVM (RBF) | 0.838 | 0.764 | 0.778 | 0.771 | 0.897 |
| Logistic Regression | 0.708 | 0.588 | 0.556 | 0.571 | 0.826 |

**Top predictive features:** Glucose, Insulin, and BMI consistently rank highest across models —
consistent with established clinical risk factors for diabetes.

## Requirements

```
python >= 3.9
pandas
numpy
scikit-learn
matplotlib
seaborn
jupyter
```

Install with:

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
```

## Usage

```bash
jupyter notebook healthcare_diabetes_analysis.ipynb
```

Or view the pre-rendered results without running any code by opening
`healthcare_diabetes_analysis_preview.html` in a browser.

## Ethical Considerations

This project uses a public, de-identified research dataset, but the pipeline is written with
real clinical deployment in mind. Key points (see notebook Section 7 for full discussion):

- **Consent & compliance** — real patient data requires informed consent and compliance with
  regulations such as HIPAA (US) or GDPR (EU).
- **Re-identification risk** — "anonymized" data can sometimes be re-identified when combined
  with other sources; direct identifiers must be stripped and re-identification risk assessed.
- **Bias & representativeness** — this dataset covers only Pima Indian women aged 21+; a model
  trained on it should **not** be assumed to generalize to other populations without
  re-validation.
- **Decision support, not diagnosis** — outputs are risk scores meant to support a clinician's
  judgment, not replace it.
- **Security & governance** — production systems should encrypt patient data, enforce
  access controls, and undergo clinical/ethical review before deployment.


## Disclaimer

This project is for educational and portfolio purposes only. It is **not** a medical device and
must not be used for actual clinical diagnosis or treatment decisions.
