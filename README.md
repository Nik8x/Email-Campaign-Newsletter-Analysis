# Email Campaign Analysis

2,240 customers from a marketing campaign dataset — demographics,
spending by category, purchase channel mix, and whether they responded
to the most recent campaign. This project builds the two things an
earlier version reached for but never delivered: a real classification
model predicting campaign response (the earlier version ended
mid-sentence saying "it would be better to build a classification
model" and stopped there), and a customer-segmentation view using
unsupervised clustering.

**Live report:** https://nik8x.github.io/Email-Campaign-Newsletter-Analysis/

## Data

The [Customer Personality Analysis](https://github.com/Abdulraqib20/Customer-Personality-Analysis)
dataset (2,240 customers). Not committed to this repo —
`00_data_setup_eda.ipynb` downloads it automatically on first run (~220KB).

## Notebooks

| Notebook | What it covers |
|---|---|
| `00_data_setup_eda.ipynb` | Downloads and cleans the data, demographic and spending distributions, campaign response rate by education. |
| `01_statistical_testing.ipynb` | Correlation and hypothesis tests: income vs spend, education vs response, kids-at-home vs response. |
| `02_feature_engineering_selection.ipynb` | Mutual information and Random Forest importance for predicting campaign response. |
| `03_model_training_evaluation.ipynb` | Class-weighted Logistic Regression vs Random Forest vs Gradient Boosting, evaluated with PR-AUC/ROC-AUC. |
| `04_clustering.ipynb` | KMeans customer segmentation, checked against real campaign response only afterward. |

Run them in order (00 → 04) — each stage loads the previous stage's saved
results from `outputs/`.

## Setup

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

## Key findings

- **Income and spending are strongly linked** (r = 0.668, p ≈ 0) — the
  highest-income customers spend meaningfully more across every product
  category.
- **Education has a real, if modest, association with campaign
  response** (p = 0.0001) — PhD holders respond at 21.0% vs 3.7% for
  customers with only a Basic education. Having kids at home is
  associated with a significantly *lower* response rate (12.0% vs 17.2%),
  consistent with these campaigns skewing toward discretionary spending.
- **A properly working classifier reaches real performance**: test
  PR-AUC 0.521 (ROC-AUC 0.858) against a 15.0% baseline positive rate —
  the classification model an earlier version of this project reached
  for in its final sentence but never built.
- **Mutual information and Random Forest importance don't fully agree**
  on which features matter most — `TotalSpend` and `Income` rank high by
  both, but `Recency` (Random Forest's #2 feature) and
  `NumCatalogPurchases` (mutual information's #3) trade places between
  the two methods, the same pattern seen in the Framingham Heart Study
  rebuild earlier in this series.
- **Customer segments found with zero response information line up with
  a real response-rate gradient** — the highest-spend, most-engaged
  segment responds at 21.0% vs 10.5% for the lowest-spend segment.

## Future work

- Model campaign-specific response (`AcceptedCmp1`-`AcceptedCmp5`)
  separately rather than only the most recent campaign, to see whether
  the same features predict response consistently across campaigns or
  whether responsiveness itself drifts over time.
- A proper RFM (recency/frequency/monetary) feature set, since the
  current features approximate but don't fully build that framework.
- Investigate the `Complain` flag (0.9% of customers) as a separate,
  rare-event target — likely needs a dedicated imbalance-handling
  approach given how much rarer it is than campaign non-response.

Full detail, code, and every number above are in the notebooks and the
[live report](https://nik8x.github.io/Email-Campaign-Newsletter-Analysis/).
