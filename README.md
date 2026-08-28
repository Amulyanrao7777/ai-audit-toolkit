# AI Audit Toolkit — COMPAS Recidivism Algorithm

A fairness audit and bias detection analysis of the COMPAS recidivism
algorithm, with an EU AI Act compliance assessment.
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Amulyanrao7777/ai-audit-toolkit/blob/main/audit.ipynb)

---

## What this is

COMPAS is a proprietary risk scoring algorithm used by US courts to predict
whether a defendant will reoffend within two years. It was used in bail,
sentencing, and parole decisions affecting thousands of people.

In 2016, ProPublica found that COMPAS was nearly twice as likely to
incorrectly label African-American defendants as high risk compared to Caucasian defendants.
This audit reproduces and extends that finding, demonstrates why the disparity
is mathematically inevitable, and assesses what EU AI Act compliance would
have required.

---

## Key findings

- African-American defendants face a **1.9x higher false positive rate** (1.92×) than Caucasian
  defendants — incorrectly labelled high risk when they would not reoffend
- Caucasian defendants face a higher false negative rate (0.57×) — incorrectly labelled
  low risk when they would reoffend
- This disparity persists in independently trained models that **do not use
  race as a feature** — 1.91× for logistic regression, 1.37× for random forest —
  indicating proxy discrimination through `priors_count`
- The disparity is mathematically inevitable given differing base rates
  (African-American: 52.3%, Caucasian: 39.1%) — the Chouldechova impossibility theorem
  applies directly
- COMPAS would be **non-compliant** with Articles 9, 10, 11, and 14 of the
  EU AI Act, and only partially compliant with Articles 13 and 15

---

## Repository structure
```
ai-audit-toolkit/
│
├── audit.ipynb                    # Full fairness analysis
├── model_card.md                  # Formal model documentation
├── eu_ai_act_assessment.md        # EU AI Act compliance assessment
└── README.md
```

---

## How to run

Open `audit.ipynb` in Google Colab or Jupyter. All data loads directly
from ProPublica's public repository — no downloads required.

```python
# Data source
url = "https://raw.githubusercontent.com/propublica/compas-analysis/master/compas-scores-two-years.csv"
```

Install dependencies if needed:

```bash
pip install shap
```

---

## What the notebook covers

1. Data loading and cleaning — ProPublica's original filtering criteria
2. Exploratory analysis — score distributions and recidivism rates by race
3. Fairness metrics — demographic parity, FPR, FNR, TPR, precision, accuracy
4. The Chouldechova impossibility theorem — demonstrated with real numbers
5. Independent classifiers — Logistic Regression and Random Forest trained
   without race as a feature
6. Fairness audit of independent models — proxy discrimination demonstrated
7. SHAP explainability — feature importance and individual prediction analysis
8. Cross-model comparison — FPR and FNR disparity across all three models

---

## EU AI Act

COMPAS is a **high-risk AI system** under Annex III of the EU AI Act. Point 8(a) —
administration of justice — is the closer fit since COMPAS is deployed by courts;
point 6(d) — law enforcement risk-of-reoffending assessment — is a genuine alternative
reading. Either way it is high-risk under Article 6(2). The full compliance assessment
in `eu_ai_act_assessment.md` evaluates Articles 9, 10, 11, 13, 14, and 15 against the
audit findings, reflecting the regulation's status as amended by the Digital Omnibus
on AI (Regulation (EU) 2026/1744, in force since 27 July 2026), which deferred the
Annex III compliance deadline to 2 December 2027 without changing the substance of
Articles 9–15.

---

## Reproducibility

Every headline figure in this repository — sample sizes, base rates, the six
fairness metrics for COMPAS and both independent models, and the SHAP feature
ranking — has been independently re-run against ProPublica's public CSV with the
notebook's fixed `random_state=42` and reproduces exactly. As of this update,
`model_card.md`'s Performance table and EU AI Act category label have been
corrected to match the notebook's actual output (a prior version of that table
had mismatched Positive Rate, TPR, FNR, and Accuracy figures, and mislabelled
the Annex III category); `README.md` and `eu_ai_act_assessment.md` were already
consistent with the code.

---

## References

- ProPublica. (2016). Machine Bias. https://github.com/propublica/compas-analysis
- Chouldechova, A. (2017). Fair prediction with disparate impact. Big Data, 5(2).
- Lundberg & Lee. (2017). A unified approach to interpreting model predictions. NeurIPS.
- European Parliament. (2024). Regulation (EU) 2024/1689 — Artificial Intelligence Act.
- European Parliament. (2026). Regulation (EU) 2026/1744 — Digital Omnibus on AI.

---

## Author

Amulya N - Data Science undergrad, RV University, Bangalore.
