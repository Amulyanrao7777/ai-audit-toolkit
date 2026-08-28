# Model Card — COMPAS Recidivism Algorithm

## Model Details
- **Model name:** COMPAS (Correctional Offender Management Profiling for Alternative Sanctions)
- **Developer:** Northpointe (now Equivant)
- **Model type:** Proprietary risk scoring algorithm
- **Version audited:** Two-year recidivism score
- **Audit conducted by:** Amulya N, RV University, June 2026

## Intended Use
- **Primary use:** Predict likelihood of reoffending within two years to inform bail, sentencing, and parole decisions in US courts
- **Intended users:** Judges, probation officers, court administrators
- **Out-of-scope uses:** Any use outside the US criminal justice context for which it was designed. Not validated for use across different jurisdictions, time periods, or populations

## Training Data
COMPAS is proprietary — training data is not publicly disclosed. This audit uses the ProPublica dataset of 7,214 defendants from Broward County, Florida (2013–2014), obtained directly from ProPublica's public repository.

## Evaluation Data
5,278 defendants after applying ProPublica's original filtering criteria (arrest within 30 days of screening) and restricting to Black and white defendants, the two groups with sufficient sample size for reliable estimation (3,175 Black, 2,103 white).

## Performance
Computed directly from `audit.ipynb`, `fairness_report()` output on the full N=5,278 sample:

| Metric | Caucasian | African-American | Disparity |
|--------|-----------|-------------------|-----------|
| Positive Rate | 0.331 | 0.576 | 1.74 |
| True Positive Rate | 0.504 | 0.715 | 1.42 |
| False Positive Rate | 0.220 | 0.423 | 1.92 |
| False Negative Rate | 0.496 | 0.285 | 0.57 |
| Precision (PPV) | 0.595 | 0.650 | 1.09 |
| Accuracy | 0.672 | 0.649 | 0.97 |

Disparity = African-American rate / Caucasian rate. 1.0 = parity.

## Key Findings
1. Black defendants are **1.9x more likely** to be incorrectly labelled high risk when they will not reoffend (FPR disparity 1.92×)
2. White defendants are more likely to be incorrectly labelled low risk when they will reoffend (FNR disparity 0.57×)
3. These disparities persist in independently trained models that do not use race as a feature (LR: 1.91×, RF: 1.37×), indicating proxy discrimination through `priors_count`
4. The disparity is mathematically inevitable given differing base rates (Black: 0.523, White: 0.391) — the Chouldechova impossibility theorem applies directly

## Limitations
- Rearrest is used as a proxy for reoffending — it reflects policing patterns, not just behaviour
- Analysis restricted to Black and white defendants due to sample size constraints
- COMPAS training data and algorithm are proprietary and cannot be independently audited
- Results are specific to Broward County, Florida and may not generalise

## Ethical Considerations
This system makes predictions that directly affect human liberty. The 1.9x false positive disparity means Black defendants who would not reoffend are systematically more likely to be detained, given harsher sentences, or denied parole. This constitutes algorithmic harm at scale. The proprietary nature of the algorithm prevents meaningful contestation by affected individuals.

## EU AI Act Classification
**High-risk AI system** under Annex III. Point 8(a) — administration of justice, AI systems used by or on behalf of a judicial authority — is the closer fit, since COMPAS is deployed by courts rather than law-enforcement agencies. Point 6(d) — law enforcement, "assessing the risk of a natural person offending or re-offending" — describes materially the same function and is a genuine alternative reading. Either classification triggers high-risk obligations under Articles 8–15. See `eu_ai_act_assessment.md` §1 for the full discussion, and §2–7 for the article-by-article assessment.

## Citation
ProPublica. (2016). COMPAS Recidivism Algorithm. https://github.com/propublica/compas-analysis
Chouldechova, A. (2017). Fair prediction with disparate impact. Big Data, 5(2), 153–163.
