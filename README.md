# Sepsis Early Warning System

Calibrated early warning system for sepsis with alarm-fatigue-aware alert policies, lead-time analysis, temporal instability analysis, and clinician-facing explainability.

## Project structure
- `notebooks/` — end-to-end 7-notebook pipeline
- `results/` — saved metrics, sweeps, and final operating points
- `figures/` — key evaluation and calibration plots

## Highlights
- Patient-grouped cross-validation with out-of-fold predictions
- Isotonic calibration for reliable risk scores
- Policy-based alerting with alarm burden and lead-time analysis
- Stability-gated alerting and explainability analysis