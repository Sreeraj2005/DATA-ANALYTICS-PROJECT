# Project 2: Predicting Income Level with Logistic Regression

A machine learning project that predicts whether a person earns more than $50K per year using census-style attributes such as age, education, occupation, and marital status.

## Dataset

- Source: https://archive.ics.uci.edu/dataset/2/adult
- Kaggle copy: https://www.kaggle.com/datasets/uciml/adult-census-income
- File used here: `adult.csv` (included in this folder)
- Shape: 32,561 rows and 15 columns
- Target variable: `income` (`<=50K` or `>50K`)
- Class balance: about 76% / 24%

## Model

**Logistic Regression** using scikit-learn, chosen because the target is binary.

## What the notebook does

1. **Target and objective:** predicts whether `income > 50K`.
2. **Preprocessing:** missing values are represented as `?`, so they are converted to an "Unknown" category. Capital gain and capital loss are log-transformed because the distributions are highly skewed. Categorical features are one-hot encoded and numeric features are standardized inside a pipeline so the test set is never leaked.
3. **Feature selection:** keeps age, `education.num`, `hours.per.week`, capital gain/loss, `workclass`, `occupation`, `marital.status`, and a `from_US` flag. Drops `education` (duplicate of `education.num`), `fnlwgt` (sampling weight), `relationship` (overlaps with marital status and sex), and `sex` and `race` (to avoid using sensitive attributes to score income). The notebook checks that removing them has little effect on performance.
4. **Split and train:** 80/20 stratified split with `random_state = 42`.
5. **Evaluation:** confusion matrix, precision, recall, F1-score, ROC-AUC, 5-fold cross-validation, and threshold comparison on the test set.
6. **Interpretation:** analyzes odds ratios and explains the model's strongest predictors, reliability, and limitations.

## Results (test set)

| Metric | Score |
|---|---|
| Accuracy | 0.845 (baseline: "always <=50K" = 0.759) |
| Precision | 0.716 |
| Recall | 0.593 |
| F1-score | 0.649 |
| ROC-AUC | 0.896 (5-fold CV mean: 0.897) |

**Main findings**

- Marital status (married), education level, and occupation are the strongest predictors. Hours worked, age, and capital gains also help.
- The model ranks people reasonably well (AUC around 0.90), but it misses roughly 4 in 10 actual high earners at the default 0.5 threshold. Lowering the threshold improves recall at the cost of precision.
- The data comes from 1994, and the model shows association rather than causation. It should not be used to make real-world decisions about people.

## Files

- `Adult ml.ipynb`: the full notebook with visible outputs
- `adult.csv`: the dataset
- `readme.md`: this file

## How to run

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook "Adult ml.ipynb"
```

Keep `adult.csv` in the same folder as the notebook.
