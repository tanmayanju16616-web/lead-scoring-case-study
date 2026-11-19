# Lead Scoring Model: X Education

A logistic regression model that scores every lead from **0 to 100** by its likelihood of becoming a paying customer, so the sales team can call the best leads first.

## Problem
Only **38.5%** of X Education's leads convert. Sales time is limited, so the goal is to rank leads and focus on the ones most likely to buy.

## Results (held-out test set)

| Metric | Value |
|---|---|
| ROC-AUC | **0.887** |
| 5-fold CV ROC-AUC | **0.906 ± 0.009** |
| Precision / Recall at 0.60 threshold | 80.8% / 64.9% |
| Log loss | 0.42 |

- **The ranking works:** the top-scored decile converts at **91.4%**, the bottom decile at **2.7%**.
- **Business threshold of 0.60:** about 31% of leads are flagged high-potential, and 80.8% of them actually convert.

## Approach
1. **Cleaning:** treated `"Select"` as missing, dropped columns that were mostly empty or near-constant, imputed with the median and an `Unknown` category, and merged rare categories (<1%) into `Other`.
2. **EDA:** univariate, bivariate and correlation analysis against the `Converted` target.
3. **Avoided data leakage:** the `Tags` column gave a 0.976 AUC but records the lead's status after sales follow-up, so I excluded it. The final model uses only information available early in the lead journey.
4. **Feature selection:** reduced multicollinearity with VIF, then checked significance with statsmodels p-values and odds ratios.
5. **Evaluation:** cross-validation, decile analysis and threshold analysis. The threshold was chosen from a business target of at least 80% conversion among selected leads.

## Lead Score Categories
| Score | Category |
|---|---|
| 0–40 | Low |
| 40–60 | Medium |
| 60–80 | High |
| 80–100 | Very High |

## Business Recommendations
- **Need more leads (e.g. internship period):** use threshold **0.4** to maximise recall.
- **Quarterly target already met:** use threshold **0.8** to maximise precision and cut unnecessary calls.

## Tech Stack
Python · pandas · NumPy · scikit-learn · statsmodels · matplotlib · seaborn

## Run It
```bash
git clone https://github.com/tanmayanju16616-web/lead-scoring-case-study.git
cd lead-scoring-case-study
pip install -r requirements.txt
jupyter notebook Lead_Scoring_Assignment.ipynb
```
The dataset (`Leads.csv`) is not included.

## Author
**Anju Kanwar**
