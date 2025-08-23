# Course: MSCS 634 – Advanced Data Mining for Data-Driven Insights and Predictive Modeling
# Deliverable 4: Final Insights, Recommendations, and Presentation

## Objective
The objective of this deliverable is to summarize the full project on the UCI Adult Income dataset and present final insights. I combined cleaning, EDA, regression, classification, clustering, and association rule mining into one clear workflow. I also discuss ethics (fairness, privacy) and give practical recommendations.

This work involved:
- Loading the dataset from the UCI URL and applying strict cleaning.  
- Running EDA and light feature engineering to support modeling.  
- Building regression (hours-per-week) and classification (income) models.  
- Applying K-Means and Agglomerative clustering with PCA visualization.  
- Mining association rules with Apriori and filtering rules related to `>50K` income.  
- Summarizing findings, challenges, and ethical considerations.  

---

## Dataset Summary
- **Source:** UCI Adult Income Dataset  
- **Size:** ~20,302 records after cleaning (originally ~32,561), 15 features  
- **Targets:**  
  - Regression: `hours-per-week`  
  - Classification: `income` (`<=50K` or `>50K`)  
- **Features:** Mix of categorical (education, workclass, occupation, marital-status, native-country, etc.) and numerical (age, education-num, capital-gain, capital-loss, hours-per-week)

---

## Data Preparation
- **Read from URL:** Treat `"?"` as missing; trim spaces; lowercase text.  
- **Imputation & Duplicates:** Impute categorical NAs with mode; drop duplicates.  
- **Type Casting:** Ensure numeric types; fill rare numeric NAs with median.  
- **Outliers:** Strict IQR removal on `capital-gain`, `capital-loss`, and `hours-per-week`.  
- **Final Shape:** About 20,302 × 15.

---

## Models Trained
### Regression (predict `hours-per-week`)
- Linear Regression, Ridge, Lasso  
- Pipeline: `StandardScaler` for numeric + `OneHotEncoder` for categorical (via `ColumnTransformer`)  
- Remove any target-derived features (like `is_high_hours`) to avoid leakage.

### Classification (predict `income`)
- Logistic Regression (baseline), Decision Tree (GridSearchCV), KNN (k sweep)  
- Train/test split 80/20 with stratified sampling  
- Same preprocessing (scaling + one-hot)

### Clustering
- K-Means (k=3) and Agglomerative (n_clusters=3)  
- PCA (2D) for plotting  
- Silhouette score to judge clusters

### Association Rule Mining
- Apriori with `min_support = 0.01`  
- Transaction-style one-hot from key categoricals + engineered flags (`hours>45`, `capital_gain>0`)  
- Filter rules that imply `income >50K`

---

## Evaluation Metrics
- **Regression:** R² (CV), MSE, RMSE  
- **Classification:** Accuracy, Precision, Recall, F1, ROC AUC; confusion matrix and ROC curve  
- **Clustering:** Silhouette score (K-Means, Agglomerative)  
- **Rules:** Support, Confidence, Lift (focus on `>50K` consequents)

**Key results (from notebook):**
- **Regression (hours/week):** CV R² ≈ **0.10–0.11**, RMSE ≈ **3.64** hours (Linear, Ridge, Lasso very close)  
- **Classification (Logistic Regression):** Accuracy **0.849**, ROC AUC **0.878**; class `>50K` recall is lower (common in this dataset)  
- **Clustering:** Silhouette ~**0.139** (K-Means), ~**0.093** (Agglomerative); clusters reflect education/occupation/hour differences  
- **Association Rules:** With `min_support=0.01`, non-empty high-lift rules combining **higher education**, **professional/exec roles**, and **longer hours** → `>50K`

---

## Insights & Observations
- **Income classification is strong** with a simple model (AUC ~0.88). Decision Tree gives similar performance with better interpretability.  
- **Predicting hours/week is weak** using demographics only (CV R² ~0.10). We need richer job-level features.  
- **Clustering** shows meaningful groups even if silhouette is modest; groups differ by education, occupation, and hours.  
- **Association rules** match EDA and clustering: education + professional roles + long hours link to `>50K`. These are descriptive, not causal.

---

## Ethical Considerations
- **Bias & Fairness:** The dataset reflects historical inequality (gender, race). Do not use sensitive features for deployment decisions. Use them only for fairness auditing (compare precision/recall across groups).  
- **Privacy:** Handle demographic data carefully; be clear about purpose.  
- **Cleaning Transparency:** Strict filters change who remains in the data. Document choices and test robustness.

---

## Challenges Encountered
- **Target leakage:** I first used a feature derived from `hours-per-week` in regression; I removed it.
- **Class imbalance:** Managed with stratified split; threshold tuning helps when recall is more important.
- **KNN sensitivity:** Needs careful scaling and k choice; recall is usually lower.
- **Association rules noise:** Many trivial rules at first; filtered by support, confidence, lift.
- **Warnings:** Divide-by-zero warnings in `association_rules` handled by cleaning inf/nan and using reasonable support.

---

## Outcome
- **Classification:** Logistic Regression and a tuned Decision Tree are reliable and easy to explain.  
- **Regression:** Limited predictive power for hours/week; needs better features.  
- **Clustering:** Useful for descriptive grouping and planning.  
- **Pattern Mining:** Apriori reveals human-readable patterns aligned with education, occupation, and hours.

---

## Files Included
- `final_project_deliverable.ipynb` — merged notebook (cleaning, EDA, models, clustering, rules, plots)  
- `README.md` — this summary for Deliverable 4  
- `Final Project Report.docx` — detailed written report with data preparation steps, modeling details, evaluation, insights, ethical considerations, recommendations, and references.
- `video_link.txt` — link to Cumberland sharepoint that contain video for final presentation [Click here to access video](https://cumber-my.sharepoint.com/:v:/r/personal/sdumaru30060_ucumberlands_edu/Documents/MSCS634_Final_Presentation.mp4?csf=1&web=1&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=kaI6JK) 
---
