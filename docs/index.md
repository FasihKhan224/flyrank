# Predicting Content Decay: A Machine Learning Approach to Content Refresh Prioritization

## Abstract
**Question:** How can we efficiently identify existing web pages that are actively losing organic traffic and require a content refresh? 
**Method:** We transitioned from a static, rule-based flagging system to a Random Forest classification model, utilizing features like content age, historical click volume, search position, and recent traffic drop percentages. 
**Result:** The machine learning model outperformed the hand-written baseline rules, achieving a significantly higher F1-score and effectively separating normal seasonal traffic fluctuations from genuine content decay.

---

## Introduction & Problem Statement
Content teams waste hundreds of hours manually auditing pages or blindly updating content based on arbitrary age thresholds. The decision this work supports is **prioritization**: determining exactly which pages a human writer or editor should spend time updating this month. The cost of getting this wrong is high: false positives waste budget on pages that are performing fine, while false negatives cause us to lose organic market share to competitors.

---

## Data
* **Release:** FlyRank Internship Warehouse 
* **Tables:** `dim_content` (pseudonymized content metadata)
* **Date Windows:** Trained on historical panels up to March 2026, leaving future months as a sealed test set.
* **Exclusions:** Pages with fewer than 30 days of historical data were excluded, as new pages have not established a reliable traffic baseline to measure decay against.

---

## Methodology
* **Task:** Binary Classification.
* **Target Label:** `is_declining` (1 if the page experienced a sustained traffic drop outside of normal variance, 0 otherwise).
* **Features:** `content_age_days`, `recent_traffic_drop_pct`, `past_clicks_30d`, `avg_position`.
* **Baseline:** A hand-written rule triggering a flag if a page was older than 365 days and experienced >10% traffic drop.
* **Validation:** We used an 80/20 train-test split, strictly ensuring that no future window data (target leakage) was included in the training features.

---

## Results
The Random Forest classifier successfully learned the non-linear relationships between a page's age, its historical authority (clicks), and recent trajectory. 
* **Baseline F1-Score:** ~0.60
* **ML Model F1-Score:** ~0.85
By relying on the ML model, the content team can trust that the generated queue is highly precise, reducing the noise of false alarms.

---

## Limitations & Honest Framing
This model is a **decision-support tool**, not a crystal ball. 
* **Observed limitations:** We do not possess data on the subjective *quality* of the content (e.g., readability, formatting, multimedia presence). 
* **Directional language:** The model identifies pages with statistical markers of decay; it cannot guarantee that updating the content will causally reverse the decline, nor does it claim to reverse-engineer Google's algorithm.

---

## Ranked Recommendations (Action Playbook)
Based on the model's probability scores, the workflow is as follows:
1. **Top 10% Probability:** Immediate assignment to editorial for a heavy rewrite (updating facts, expanding sections).
2. **10% - 30% Probability:** Send to SEO team for metadata/internal linking optimization (light refresh).
3. **Bottom 70%:** Protect and monitor. Do not waste editorial resources here.

---

## Reproducibility
All code, queries, and pipelines used to generate this analysis are publicly available in my repository.
* **GitHub Repository:**(https://github.com/FasihKhan224/flyrank/tree/main)

---

## Acknowledgments & Data Credit
Built on the FlyRank ML Internship dataset.
Learn more at [https://flyrank.ai](https://flyrank.ai)
