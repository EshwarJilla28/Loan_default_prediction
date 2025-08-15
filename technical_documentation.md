# Technical Documentation: Loan Default Prediction & Risk Analytics Platform

---
## Table of Contents
1. Introduction
2. High-Level Architecture
3. Data Pipeline
4. Advanced Feature Engineering
5. Business Cost Modeling
6. ML Modeling Framework
7. Risk Segmentation Logic
8. Explainability and Compliance Framework
9. Extension Guide for Developers
10. Deployment and Scaling Roadmap
11. Testing and Monitoring
12. References & Further Reading

---

## 1. Introduction

This technical guide details the system behind the Loan Default Prediction & Risk Analytics Platform. It is intended for ML engineers and software developers—especially those aiming to extend, scale, or productionize the solution for financial services firms.

---

## 2. High-Level Architecture

- **Input:** Raw loan application data files (CSV or database)
- **Processing:** Modular notebook-driven ETL and computation pipeline
- **Output:** Enhanced datasets, trained profit-optimised ML models, portfolio segmentations, explainability results
- **Artifacts:** `data/processed/`, `models/`, `reports/figures/`, code in `notebooks/`

**Pipeline Stages:**
1. Data Foundation
2. Feature Engineering
3. Cost-sensitive Modeling
4. Risk Segmentation
5. Explainability & Compliance

---

## 3. Data Pipeline (Phase 1)

- Load all raw inputs via pandas, verify schema per the dictionary.
- Data cleaning: impute missing, outlier rejection (Z-score, quantiles), type consistency.
- Target variable: default binary label extraction and class balancing checks.
- Save clean baseline data to `data/processed/baseline_data.csv`.

---

## 4. Advanced Feature Engineering (Phase 2.1)

- **Domain features:**
  - Payment capacity: DTI, debt-service-coverage, payment-shock, cash-flow stress test
  - Credit intelligence: Utilization, anomaly detection, risk-tiers
  - Employment: Human capital index, education premium, stability score
  - Loan structure: Opportunity index, pricing efficiency, term-risk premium, risk flags
- Use vectorized pandas/numpy ops for speed; group-by medians for risk scores per category.
- Outputs: 50+ engineered features in `enhanced_features_dataset.csv`

---

## 5. Business Cost Modeling (Phase 2.2)

- **False Negative Cost:** Loss due to undetected default (typically 65% principal)
- **False Positive Cost:** Opportunity cost of rejecting good customer (~3% principal)
- **True Positive Revenue:** Realised profit from approved good loans (~8% principal)
- Compute these per-loan, then aggregate for business impact analysis.
- Implement cost-sensitive training via sklearn/LightGBM class weights (TP/FP/FN)

---

## 6. ML Modeling Framework (Phase 2.2, 2.4)

- **Model choices:** LightGBM for core, Decision Trees/Logistic Regression for interpretability
- **Training:** Robust train/validation/test split (stratified), missing value imputation
- **Threshold optimization:** Grid search over probability thresholds, select one maximizing net profit
- Store model and configuration in `models/`

---

## 7. Risk Segmentation Logic (Phase 2.3)

- Segment users by multidimensional scoring: Stability, profitability, complexity
- Rule-driven business segments (Premier, Prime Plus, Stable Core, etc.)
- Segment-specific decision thresholds and pricing logic for maximum profitability and minimal risk concentration
- Output segmentation assignments for portfolio analysis and dashboarding

---

## 8. Explainability & Compliance Framework (Phase 2.4)

- Feature importance: built-in (LightGBM), model-agnostic (permutation)
- Extracted rules: interpretable shallow tree, logistic regression coefficients
- Bias audits: group-by rates per Education (and other protected fields)
- Artifact generation: model-card JSON, business rules TXT, importance CSV
- Compliance pointers: FCRA/ECOA checks (adverse-action factors)

---

## 9. Extension Guide for Developers

**Plug in new data:** Update ETL and feature notebook (handle new columns carefully)
**Switch models:** Drop-in replacements supported, provided input features compatible (see notebook 03)
**Add real-time API:** Wrap model in FastAPI, serialize with joblib/pickle, validate input schema, add request logging
**Scale to Big Data:** Use Dask or Spark for pandas/numpy, persistent storage on S3/Blob, move artifact output to cloud
**Expand Explainability:** Integrate SHAP, add dashboarding via Streamlit/Gradio
**Enterprise Monitoring:** Add drift detection, automated reporting, outlier monitors, catastrophic error handling

---

## 10. Deployment & Scaling Roadmap

- **Containerize:** Dockerfile with all dependencies
- **CI/CD:** Github Actions, pytest suite, static code analysis, test data replay
- **Cloud deploy:** AWS ECS/Fargate, Azure Web Apps; secure secrets handling
- **Observability:** InfluxDB/Loki for logging and metrics, Grafana for live dashboards

---

## 11. Testing and Monitoring

- Unit + integration tests (pytest, principal test cases per module)
- Holdout test-set performance tracking (auto-gen metrics on deploy)
- Continuous monitoring: prediction logs, business profit impact, fairness drift
- Automated rollback on failure, fail-safe on catastrophic prediction error

---

## 12. References & Further Reading

- [Sklearn Documentation](https://scikit-learn.org/)
- [LightGBM Official](https://lightgbm.readthedocs.io/)
- [Regulatory: FCRA/ECOA Guides](https://www.consumerfinance.gov/)
- [Academic: Explainable AI in Finance](https://arxiv.org/abs/1909.08053)

---

## Contact

Raise issues in repo or connect via author handle.
