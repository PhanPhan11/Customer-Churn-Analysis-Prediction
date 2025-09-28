
# Customer-Churn-Analysis-Prediction

Predicting and analyzing the reasons for customer churn in the telecommunications industry to help businesses retain customers and optimize their marketing strategies.

# Customer Churn Analysis & Prediction for Proactive Retention
![Churn Overview](IMAGES/Sales Metrics Whiteboard (1).png)

## 1. Overview of Customer Churn Risk Characteristics

### 1.1. Data Overview

* Churned customers (Churn = Yes): **26.5%**
* Loyal customers (Churn = No): **73.5%**

### 1.2. Customer Characteristics

* Gender is not a major differentiating factor.
* Older customers have **2x higher churn risk**.
* Single customers tend to leave more often (possibly more price-sensitive or less attached).
* Customers with dependents (families) are more loyal.

### 1.3. Contract Characteristics

* Customers with **paperless billing** and **month-to-month contracts** have the highest churn risk.
* Long-term contracts (two-year) have only **2.8% churn rate**.

### 1.4. Insights

* Focus on retaining **month-to-month + paperless billing** segment.
* Offer incentives to switch to **1–2-year contracts** (fee discounts, free data).
* Key retention targets:

  * Senior customers
  * Single customers without dependents
  * Customers using electronic (paperless) billing

---

## 2. Relationship Between Services and Churn

### 2.1. Contract & Billing

* Customers using **paperless billing** show significantly higher churn compared to traditional billing.
* **The longer the contract, the lower the churn rate** — strongest correlation found.

### 2.2. Internet & Phone Services

* **Fiber optic** users have the highest churn (~42%) — likely due to higher prices or competition.
* **DSL or no internet** users churn less.
* Phone services show minimal impact compared to internet type.

### 2.3. Payment Methods

* **Electronic check** users churn nearly **3x** more than those on auto-pay.
* Auto-pay through bank or credit card leads to higher customer loyalty.

### 2.4. Insights

* Encourage upgrades to 1–2-year contracts.
* Maintain regular engagement (personalized SMS/email).
* Investigate churn reasons (price, service quality, customer support).
* Promote auto-pay to improve retention.

---

## 3. Customer Lifetime Value (CLV)

### 3.1. CLV-Based Churn Analysis

| CLV Group (VND) | Churn Rate (%) | Observation                          |
| --------------- | -------------- | ------------------------------------ |
| 0 – 1,000K      | ~30–35%        | Low-value customers, high churn risk |
| 1,000K – 3,000K | 20–25%         | Still relatively high risk           |
| 3,000K – 6,000K | 10–15%         | Medium value, more stable behavior   |
| > 6,000K        | <10%           | High-value, loyal customers          |

**Remarks:**

* Churn rate decreases sharply as CLV increases.
* Strong inverse relationship between CLV and churn.
* Low-value customers (CLV < 3M VND) are **priority intervention targets**.

### 3.2. Average Total Charges by Tenure

* Average total charges increase linearly with customer tenure.
* From 0 → 70 months, total charges rise from 0 to over 6,000 (VND x1000).
* After ~60 months, churn probability approaches zero.

### 3.3. Insights

* Retaining a customer for just one more year yields significant additional revenue.
* Early renewal or loyalty programs effectively increase CLV with minimal marketing cost.

---

## 4. Customer Segmentation Using K-Means

### 4.1. Clustering Model

* Algorithm: **K-Means (K=4)**
* Dimensionality reduction via **PCA** for visualization.
* Input features: tenure, MonthlyCharges, TotalCharges, CLV, InternetService, Contract, etc.
* PCA projection clearly shows four distinct customer segments based on spending and tenure.

### 4.2. Cluster Interpretation

**Cluster 0 – “Loyal VIPs”**

* High tenure (~60 months), high monthly spend (~91).
* Total charges > 5M VND.
* Very low churn → high-value customers.
* Strategy: maintain via exclusive offers, premium care, loyalty rewards.

**Cluster 1 – “Potential, Needs Nurturing”**

* Medium tenure (~3 years), low spending.
* Stable but underutilizing services.
* Strategy: light upselling (bundles, combos), loyalty points program.

**Cluster 2 – “High Spend, Short Tenure”**

* Recently joined (~1.5 years), high spending (~77).
* Possibly trialing premium packages.
* Strategy: proactive engagement, fast support to prevent churn.

**Cluster 3 – “Low-Value / At-Risk”**

* Low tenure (~20 months), low spending (~36).
* Lowest total charges (<1M).
* Strategy: minimal marketing spend, basic maintenance only.

### 4.3. Strategic Summary

| Cluster   | Priority Level | Recommended Actions                      |
| --------- | -------------- | ---------------------------------------- |
| Cluster 0 | High           | Retain – VIP policy & personal care      |
| Cluster 1 | Medium         | Upsell to higher service tiers           |
| Cluster 2 | High           | Early retention – proactive support & CX |
| Cluster 3 | Low            | Cost optimization – minimal engagement   |

---

## 5. Churn Prediction for Proactive Retention

### 5.1. Objective

Early identification of customers likely to churn, enabling **proactive retention campaigns** instead of reactive responses after customer loss.

### 5.2. Workflow

* **Label Encoding**: Convert categorical variables to numeric.
* **Scaling**: Normalize data.
* **Model Training**: Naive Bayes, Decision Tree, Random Forest, XGBoost, LightGBM, CatBoost.
* **Imbalanced Data Handling**: SMOTE, ADASYN, SMOTEENN, Class Weights, Hyperparameter Tuning.

### 5.3. Model Performance

| Model                       | Best Threshold | Precision |    Recall |        F1 |   ROC-AUC |
| --------------------------- | -------------: | --------: | --------: | --------: | --------: |
| Naive Bayes                 |         0.6826 |     0.537 |     0.730 |     0.619 |     0.823 |
| Decision Tree               |          1.000 |     0.493 |     0.479 |     0.486 |     0.650 |
| Random Forest               |          0.296 |     0.531 |     0.746 |     0.621 |     0.826 |
| XGBoost                     |          0.434 |     0.536 |     0.730 |     0.618 |     0.821 |
| LightGBM                    |          0.427 |     0.515 |     0.802 |     0.628 |     0.832 |
| **CatBoost (Class Weight)** |      **0.394** | **0.501** | **0.832** | **0.625** | **0.839** |

### 5.4. CatBoost Confusion Matrix

|                      | Predicted NON-CHURN (0) | Predicted CHURN (1) |
| -------------------- | ----------------------- | ------------------- |
| Actual NON-CHURN (0) | 725 (TN)                | 310 (FP)            |
| Actual CHURN (1)     | 63 (FN)                 | 311 (TP)            |

* True Positive (TP = 311): Correctly identified customers about to churn.
* False Negative (FN = 63): Missed churn cases — lowest among models.
* False Positive (FP = 310): Some false alerts, acceptable for proactive retention.

### 5.5. Model Selection

* **CatBoost (Class Weight)** achieved **Recall = 83%**, **ROC-AUC = 0.839**.
* Although precision is moderate, it aligns well with the **goal of customer retention**.

**Conclusion:** CatBoost with class weighting is the best model for early churn detection.

### 5.6. Business Impact

* Generate a **monthly list of high-risk customers**.
* Automatically trigger retention campaigns (discounts, support, consultations).
* Measure campaign effectiveness based on actual churn probabilities.
