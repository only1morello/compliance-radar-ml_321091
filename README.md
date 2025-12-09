# Compliance Radar Project

**Course:** Machine Learning (2025/2026)  
**Captain:** Arnur Yembergen / 321091  
**Team members:**
- Daniiyar Dussembayev / 303501
- Temirlan Zorky bayev / 322371

## 1. Introduction: The Problem
In corporate governance, "compliance risk" is often a lagging indicator; you only find out something is wrong after an audit fails. The goal of this project is to predict high-risk departments *before* an audit occurs. 

By analyzing internal data (team structure, operational metrics, reporting delays), we built a machine learning framework to flag departments that are likely to fall into the top 25% of risk exposure.

## 2. Methodology & Design Choices

### Data Preparation
The dataset (`org_compliance_data.db`) was realistic but messy.
- **Target Definition:** We didn't just predict the raw score. We converted the problem into a classification task. Departments with an `overall_risk_score` in the top 75th percentile were labeled as **High Risk (1)**.
- **Handling Missing Data:** About ~40% of rows had missing target values, which we dropped to ensure training quality. For feature columns, we used median imputation (to avoid outlier influence) and filled categorical gaps with "Unknown".
- **Preventing Data Leakage:** This is crucial. We deliberately removed audit scores (`audit_score_q1`, etc.) and the final compliance score from the training features. In a real-world scenario, we wouldn't know these scores at the moment of prediction.

### Modeling Strategy
We compared three approaches to find the best balance between complexity and performance:
1.  **Logistic Regression:** A linear baseline to see if the problem is simple.
2.  **Random Forest:** An ensemble method to capture non-linear dependencies.
3.  **Gradient Boosting:** To squeeze out maximum performance.

We used **80/20 stratified splitting** to maintain the ratio of risky departments in the test set.

## 3. Results

The tree-based models significantly outperformed the linear baseline. Random Forest provided the most stable results after hyperparameter tuning.

| Model | F1-Score | ROC-AUC |
|-------|----------|---------|
| Logistic Regression | ~0.872 | ~0.978 |
| **Random Forest** | **~0.919** | **~0.988** |  😮
| Gradient Boosting | ~0.919 | ~0.994 |

*See `images/model_comparison.png` for a visual benchmark.*

### Why are departments risky? (Interpretation)
The model didn't just predict; it explained. The Feature Importance analysis highlights that:
1.  **Operational & Financial Risk Exposure** are the strongest predictors.
2.  **Past Violations** are a major red flag (history repeats itself).
3.  **Reporting Gaps** also play a significant role.

This confirms the hypothesis: delays in reporting and previous minor violations are early warning signs of systemic compliance failure.

*See `images/feature_importance.png` for detailed drivers.*

## 4. Conclusion
We successfully built a predictive radar. The model can identify high-risk departments with >95% reliability using only leading indicators. For the business, this means we can prioritize audits for specific teams rather than checking everyone randomly, saving resources and reacting faster.

## 5. How to Reproduce
1.  Clone the repository.
2.  Install dependencies: `pip install -r requirements.txt`.
3.  Run the `compliance_radar_main.ipynb` notebook.
