# AI Audit Toolkit — COMPAS Recidivism Algorithm

A fairness audit and bias detection analysis of the COMPAS recidivism 
algorithm, with an EU AI Act compliance assessment.

---

## What this is

COMPAS is a proprietary risk scoring algorithm used by US courts to predict 
whether a defendant will reoffend within two years. It was used in bail, 
sentencing, and parole decisions affecting thousands of people.

In 2016, ProPublica found that COMPAS was nearly twice as likely to 
incorrectly label Black defendants as high risk compared to white defendants. 
This audit reproduces and extends that finding, demonstrates why the disparity 
is mathematically inevitable, and assesses what EU AI Act compliance would 
have required.

---

## Key findings

- Black defendants face a **1.9x higher false positive rate** than white 
  defendants — incorrectly labelled high risk when they would not reoffend
- White defendants face a higher false negative rate — incorrectly labelled 
  low risk when they would reoffend
- This disparity persists in independently trained models that **do not use 
  race as a feature**, indicating proxy discrimination through `priors_count`
- The disparity is mathematically inevitable given differing base rates 
  (Black: 52.3%, White: 39.1%) — the Chouldechova impossibility theorem 
  applies directly
- COMPAS would be **non-compliant** with Articles 9, 10, 11, and 14 of the 
  EU AI Act

---

## Repository structure
