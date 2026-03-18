# Sepsis Early Warning System Using Machine Learning

A calibrated, decision-aware early warning system for sepsis that balances **early detection**, **alarm burden**, and **clinical trust** using patient-grouped validation, alert-policy design, temporal instability analysis, and clinician-facing explainability.

## Why this project matters

Most ML projects for healthcare stop at AUROC or AUPRC.  
This project goes further by asking a more realistic question:

> **How should alerts be generated so that sepsis is detected early without overwhelming clinicians with unnecessary alarms?**

That framing turns the project from a pure prediction task into a **decision-support system**.

---

## Final outcome

### Recommended default operating mode — balanced / low-alarm
- **Model:** HistGradientBoostingClassifier
- **Calibration:** Isotonic regression
- **Policy:** First-crossing alert with 6-hour lockout
- **Threshold:** `tau = 0.08579`
- **Sepsis detection rate:** `0.897`
- **Non-sepsis patient false-alert rate:** `0.000`
- **Alarm burden:** `0.867 alerts / 100 patient-hours`
- **Median lead time:** `40 hours`

### Safety-first operating mode — high detection
- **Policy:** First-crossing alert with 6-hour lockout
- **Threshold:** `tau = 0.08105`
- **Sepsis detection rate:** `1.000`
- **Non-sepsis patient false-alert rate:** `0.000`
- **Alarm burden:** `1.640 alerts / 100 patient-hours`
- **Median lead time:** `55 hours`

### Key takeaway
The final system supports **two operating modes**:
- a **balanced mode** for lower alarm burden
- a **safety-first mode** for maximum sensitivity

This makes the project much closer to a real-world healthcare ML decision system than a standard classification notebook.

---

## What makes this different

This project is built around five ideas:

- **Patient-grouped cross-validation** to avoid leakage across patient trajectories
- **Out-of-fold calibrated probabilities** for more reliable risk estimates
- **Alert-policy evaluation** using operational metrics, not just classification metrics
- **Temporal instability analysis** to study whether unstable predictions should affect alerting
- **Clinician-facing explainability** to make alerts more transparent and auditable

---

## Project pipeline

### `01_eda_problem_definition.ipynb`
Exploratory analysis, cohort understanding, and problem framing.

### `02_feature_engineering_temporal.ipynb`
Temporal feature engineering with leakage prevention:
- rolling means / min / max / std
- deltas over time
- previous-value features
- missingness indicators

### `03_groupcv_modeling_oof.ipynb`
Patient-grouped cross-validation and out-of-fold prediction generation.

### `04_calibration_policyA_sweep.ipynb`
Calibration and Policy A evaluation:
- Platt vs isotonic calibration
- reliability analysis
- threshold sweep for first-crossing + lockout policy

### `05_policyB_persistence_sweep.ipynb`
Persistence-based alert policy:
- K-of-M alert logic
- Policy A vs Policy B tradeoff analysis

### `06_final_decision_and_utility.ipynb`
Final operating-point selection:
- constraint-based thresholding
- utility-based thresholding
- final deployable operating modes

### `07_uncertainty_instability_explainability.ipynb`
Research-grade extensions:
- temporal instability metrics
- stability-gated alerting
- global and local explainability artifacts

---

## Modeling summary

### Feature engineering
The model uses engineered temporal features rather than raw vitals alone, including:
- rolling statistics over multiple windows
- short-term and medium-term deltas
- previous observed values
- missingness indicators

### Model comparison
Two baseline model families were evaluated:
- Logistic Regression
- HistGradientBoostingClassifier

The final system uses **HistGradientBoostingClassifier**, which achieved the strongest grouped cross-validation performance.

### Calibration
Calibration was treated as a first-class requirement rather than an afterthought.  
The project compares:
- uncalibrated probabilities
- Platt scaling
- isotonic regression

**Isotonic regression** was selected as the final calibrator.

---

## Evaluation philosophy

Instead of relying only on AUROC/AUPRC, the alerting system is evaluated using metrics that matter operationally:

- **sepsis detection before onset**
- **non-sepsis patient false-alert rate**
- **alerts per 100 patient-hours**
- **median lead time**
- **temporal stability / instability**
- **global and local explainability**

This makes the project more aligned with **clinical decision support** than with leaderboard-style predictive modeling.

---

## Alert policies studied

### Policy A — first crossing + lockout
Alert as soon as calibrated risk crosses the threshold, then suppress repeat alerts for a lockout period.

### Policy B — persistence policy
Require threshold crossing multiple times within a recent window before firing an alert.

### Policy C — stability-gated alerting
Combine threshold crossing with temporal stability constraints so unstable risk trajectories can be treated more cautiously.

## Results and artifacts

The `results/` folder contains:
- grouped CV model metrics
- calibration report
- threshold sweep outputs
- selected operating points
- final decision files
- explainability artifacts

The `figures/` folder contains:
- calibration reliability plots
- ROC / PR baseline figures
- lead-time visualizations
- alert policy tradeoff plots
- cohort-level exploratory figures

---

## Reproducibility

Install dependencies: pip install -r requirements.txt

## Limitations

This repository is intended for research and portfolio purposes, not clinical deployment.

Important limitations:

no external clinical validation

no prospective validation

no clinician user study

no deployment-side integration with hospital workflow

real-world use would require additional safety, fairness, and governance review

## Ethics note

This project uses de-identified public ICU data and does not use patient identity information.
Any real-world deployment of automated clinical alerts should include:

clinician oversight

prospective validation

bias and subgroup analysis

workflow-specific safety review

human-in-the-loop governance

---

## Repository structure

```text
sepsis-early-warning-system/
├── notebooks/
├── results/
├── figures/
├── .gitignore
├── README.md
└── requirements.txt

