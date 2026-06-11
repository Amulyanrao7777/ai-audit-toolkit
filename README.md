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
5,278 defendants after applying ProPublica's original filtering criteria (arrest within 30 days of screening). Primary analysis restricted to Black and white defendants (n=5,278) due to insufficient sample sizes for other racial groups.

## Performance
| Metric | Caucasian | African-American | Disparity |
|--------|-----------|-----------------|-----------|
| Positive Rate | 0.421 | 0.583 | 1.385 |
| True Positive Rate | 0.580 | 0.721 | 1.243 |
| False Positive Rate | 0.220 | 0.423 | 1.923 |
| False Negative Rate | 0.420 | 0.279 | 0.664 |
| Accuracy | 0.671 | 0.655 | 0.976 |

## Key Findings
1. Black defendants are **1.9x more likely** to be incorrectly labelled high risk when they will not reoffend
2. White defendants are more likely to be incorrectly labelled low risk when they will reoffend
3. These disparities persist in independently trained models that do not use race as a feature, indicating proxy discrimination through `priors_count`
4. The disparity is mathematically inevitable given differing base rates (Black: 0.523, White: 0.391) — the Chouldechova impossibility theorem applies directly

## Limitations
- Rearrest is used as a proxy for reoffending — it reflects policing patterns, not just behaviour
- Analysis restricted to Black and white defendants due to sample size constraints
- COMPAS training data and algorithm are proprietary and cannot be independently audited
- Results are specific to Broward County, Florida and may not generalise

## Ethical Considerations
This system makes predictions that directly affect human liberty. The 1.9x false positive disparity means Black defendants who would not reoffend are systematically more likely to be detained, given harsher sentences, or denied parole. This constitutes algorithmic harm at scale. The proprietary nature of the algorithm prevents meaningful contestation by affected individuals.

## EU AI Act Classification
**High-risk AI system** under Annex III, Category 6 — Administration of justice and democratic processes. Full compliance assessment available in `eu_ai_act_assessment.md`.

## Citation
ProPublica. (2016). COMPAS Recidivism Algorithm. https://github.com/propublica/compas-analysis
Chouldechova, A. (2017). Fair prediction with disparate impact. Big Data, 5(2), 153–163.
