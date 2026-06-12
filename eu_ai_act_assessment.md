# EU AI Act Compliance Assessment - COMPAS Recidivism Algorithm

**Assessed by:** Amulya N, RV University  
**Date:** June 2026  
**Regulation reference:** Regulation (EU) 2024/1689 — Artificial Intelligence Act  
**Disclaimer:** This is an independent academic audit. It does not constitute legal advice.

---

## 1. Risk Classification

**Classification: HIGH-RISK**

Under Annex III, Article 6(2), COMPAS falls under Category 6:

> *"AI systems intended to be used by competent authorities or on their behalf for making or assisting in making decisions on matters relating to... the administration of justice and democratic processes."*

Specifically - bail decisions, sentencing recommendations, and parole assessments are explicitly within scope. This is not a borderline case. COMPAS is one of the clearest examples of a high-risk AI system under the Act's own language.

**Implications of high-risk classification:**
All obligations under Articles 8–15 apply in full. These include risk management, data governance, technical documentation, transparency, human oversight, accuracy, and robustness requirements.

---

## 2. Article 9 - Risk Management System

**Requirement:** The provider must establish, implement, document, and maintain a risk management system throughout the AI system's lifecycle. Risks must be identified, estimated, evaluated, and mitigated.

**Assessment: NON-COMPLIANT**

Findings from this audit:

- Black defendants face a **1.9x higher false positive rate** than white defendants. This constitutes an identified, measurable, and unmitigated risk of discriminatory harm.
- The Chouldechova impossibility theorem demonstrates that this disparity is mathematically inevitable given differing base rates. No risk management documentation acknowledges this constraint or describes how Northpointe has responded to it.
- Rearrest is used as a proxy for reoffending. This introduces systematic measurement error that disproportionately affects defendants from over-policed communities. No mitigation is documented.
- The algorithm is proprietary. No public risk management documentation exists that would allow independent verification of Article 9 compliance.

**What compliance would require:**
- Documented identification of the false positive disparity as a known risk
- A stated policy on which fairness definition was prioritised and why
- Mitigation measures or explicit acknowledgement that mitigation is mathematically constrained
- Periodic re-evaluation of risk as deployment context changes

---

## 3. Article 10 — Data and Data Governance

**Requirement:** Training, validation, and testing datasets must be relevant, representative, free of errors, and complete. Appropriate data governance practices must be in place. Datasets must be examined for possible biases.

**Assessment: NON-COMPLIANT**

Findings:

- Training data is not publicly disclosed. Independent verification of representativeness is impossible.
- The Broward County dataset used in this audit shows significant racial imbalance in base rates (Black: 52.3%, White: 39.1%). Whether this imbalance was examined during development is unknown.
- `priors_count` — the most influential feature per SHAP analysis - is a known proxy for race due to differential policing patterns. No documentation indicates this was identified or addressed during data governance.
- Ground truth label (`two_year_recid`) is defined as rearrest, not reoffending. This introduces label bias that was not disclosed in deployment contexts.

**What compliance would require:**
- Public disclosure of training data composition and governance practices
- Documented examination of proxy variables for racial correlation
- Consideration of alternative ground truth definitions
- Bias testing across all protected characteristics before deployment

---

## 4. Article 11 — Technical Documentation

**Requirement:** Technical documentation must be drawn up before the system is placed on the market and kept up to date. It must demonstrate compliance with the Act's requirements and provide authorities with the information needed to assess conformity.

**Assessment: NON-COMPLIANT**

- No public technical documentation exists for COMPAS.
- The algorithm's weights, training procedure, feature engineering decisions, and validation methodology are proprietary and undisclosed.
- Courts and defendants who were subject to COMPAS scores had no access to documentation that would allow them to understand or contest the basis of decisions made about them.

**What compliance would require:**
- A complete model card (see `model_card.md` in this repository for a reference implementation)
- Documentation of intended purpose, performance metrics disaggregated by demographic group, known limitations, and appropriate use contexts
- Version control and change documentation

---

## 5. Article 13 — Transparency and Provision of Information

**Requirement:** High-risk AI systems must be designed and developed to ensure sufficient transparency that deployers can interpret the system's output and use it appropriately. Instructions for use must accompany the system.

**Assessment: PARTIALLY COMPLIANT**

- COMPAS does provide a numeric score and a categorical label (Low/Medium/High). This is a minimal form of transparency.
- However, the basis for the score is not disclosed. A defendant or judge cannot determine which factors drove a particular score or by how much.
- SHAP analysis in this audit demonstrates that `priors_count` and `age` are the dominant features. This information was not available to courts using the system.
- No instructions for use address the known false positive disparity or advise deployers on how to account for it.

**What compliance would require:**
- Per-prediction explanations accessible to affected individuals
- SHAP or equivalent explainability layer integrated into the system output
- Explicit disclosure of known performance disparities across demographic groups in instructions for use

---

## 6. Article 14 — Human Oversight

**Requirement:** High-risk AI systems must be designed to allow effective human oversight. Natural persons must be able to understand the system's capacities and limitations, detect and address dysfunctions, and refrain from using outputs when appropriate.

**Assessment: NON-COMPLIANT IN PRACTICE**

- Formally, judges retain final decision-making authority. A human is technically in the loop.
- In practice, research shows judges defer heavily to algorithmic scores — particularly in high-caseload environments. The score functions as a decision rather than an input.
- No training or guidance was provided to judges on the false positive disparity. A judge cannot exercise meaningful oversight of a risk they are unaware of.
- The proprietary nature of the algorithm makes it impossible for a judge to interrogate or contest a specific score.

**What compliance would require:**
- Mandatory training for deployers on system limitations and known disparities
- Interface design that presents the score as one input among many, not a recommendation
- A documented process for contesting or overriding scores
- Logging of cases where human judgment diverged from algorithmic output

---

## 7. Article 15 — Accuracy, Robustness, and Cybersecurity

**Requirement:** High-risk AI systems must achieve appropriate levels of accuracy, robustness, and cybersecurity. Performance must be consistent and errors must be minimised.

**Assessment: PARTIALLY COMPLIANT**

- Overall accuracy of approximately 65% is modest for a system used in liberty-affecting decisions.
- Performance is not consistent across demographic groups — the false positive rate for Black defendants (42.3%) is nearly double that for white defendants (22.0%).
- No public documentation of robustness testing across different jurisdictions, time periods, or demographic subgroups exists.

**What compliance would require:**
- Disaggregated performance metrics published and maintained
- Defined minimum acceptable performance thresholds per demographic group
- Regular re-evaluation as population demographics and policing patterns change

---

## 8. Summary Table

| Article | Requirement | Assessment |
|---------|-------------|------------|
| Art. 9 | Risk management | Non-compliant |
| Art. 10 | Data governance | Non-compliant |
| Art. 11 | Technical documentation | Non-compliant |
| Art. 13 | Transparency | Partially compliant |
| Art. 14 | Human oversight | Non-compliant in practice |
| Art. 15 | Accuracy and robustness | Partially compliant |

---

## 9. Conclusion

COMPAS, as deployed in US courts, would not meet the requirements of the EU AI Act if it were subject to its jurisdiction. The most critical gaps are the absence of public technical documentation, the unmitigated and undisclosed false positive disparity, and the practical failure of human oversight in high-caseload deployment contexts.

These are not incidental gaps. They reflect a fundamental tension between proprietary AI systems and the Act's transparency and accountability requirements. The EU AI Act's position is clear: systems making decisions that affect human liberty cannot be black boxes. Accountability requires documentation, and documentation requires disclosure.

The mathematical constraints identified by Chouldechova mean that perfect fairness is not achievable. But the Act does not require perfection — it requires that known risks be identified, documented, and governed by deliberate human choices. That standard, COMPAS does not meet.

---

## References

- European Parliament. (2024). Regulation (EU) 2024/1689 — Artificial Intelligence Act.
- Angwin, J. et al. (2016). Machine Bias. ProPublica.
- Chouldechova, A. (2017). Fair prediction with disparate impact. Big Data, 5(2), 153–163.
- Lundberg, S., Lee, S. (2017). A unified approach to interpreting model predictions. NeurIPS.
- Gebru, T. et al. (2018). Datasheets for datasets. Communications of the ACM.
