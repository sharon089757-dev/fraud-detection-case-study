# Fraud Detection Case Study

A transaction-level fraud detection case study using the synthetic PaySim dataset. The analysis compares an amount-based review rule with logistic regression and gradient boosting, using a chronological train/validation/test split and a daily review capacity of 1,000 transactions.

## Read the analysis

**[Open the notebook](Interchecks_Fraud_Detection.ipynb)** — includes the executive summary, data observations, model comparisons, saved charts, and validation diagnostics. You can review the results directly on GitHub without running the code.

## Main findings

In the out-of-time test period (1,840 labeled fraud cases):

| Review policy | Transactions reviewed | Fraud cases identified |
| --- | ---: | ---: |
| Existing amount rule | 5,231 | 534 |
| Model, matched review workload | 5,231 | 1,656 |
| Model, up to 1,000 reviews/day | 6,272 | 1,839 |

These are **retrospective results on synthetic data**, not a production performance claim. The model relies heavily on origin-balance features, and the dataset has pronounced time and label-distribution shifts. The recommended next step is a controlled shadow pilot, not immediate rule replacement.

## Approach

- Chronological train/validation/test split, with model selection based on validation data.
- Existing rule → logistic regression → gradient boosting.
- Primary metric: fraud identified under a daily review cap; additional metrics include precision, recall, and average precision (PR-AUC).
- Diagnostics: feature ablation, permutation importance, error analysis, review-budget sensitivity, forward-time checks, and operational review-allocation scenarios.

## Reproduce

1. Use Python 3.10+ and install dependencies: `pip install -r requirements.txt`.
2. Open `Interchecks_Fraud_Detection.ipynb` in Jupyter or VS Code.
3. Run cells in order. The notebook uses `kagglehub` to retrieve the PaySim data (`dylanmoraes/credit-card-fraud-dataset`); downloading requires internet access and acceptance of any applicable dataset terms.
4. Full training and diagnostic runs can take substantial time and memory. The committed notebook includes previously executed outputs for review.

## Limitations

The dataset is synthetic. A retrospective daily top-K ranking is not equivalent to real-time fraud review. The final test day is unusually small and entirely fraud-labeled. Additional test-set diagnostics are exploratory and are not an independent second holdout. Model results should be validated with decision-time features and matured labels on new operational data.
