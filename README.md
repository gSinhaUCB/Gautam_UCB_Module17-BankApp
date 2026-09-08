# Gautam_UCB_Module17-BankApp
Comparing machine learning classifiers on Portuguese bank telemarketing deposit campaigns
# Bank Telemarketing Classification: Optimizing Long-Term Deposit Subscriptions

## Executive Summary
This project analyzes direct telemarketing campaigns conducted by a Portuguese banking institution (sourced from the UC Irvine Machine Learning Repository). The primary business objective is to predict whether a client will subscribe to a term deposit (`y = "yes"` vs. `y = "no"`). By accurately scoring and ranking prospective leads before dialing, the institution can maximize deposit acquisition while minimizing operational call center fatigue.

## Key Findings & Business Insights
1. **The Realistic Predictor Constraint (`duration`):**
   - Call duration is the strongest single predictor of subscription. However, as noted in the dataset documentation, call duration is unknown prior to placing the call ($duration = 0$ at lead selection time). 
   - To make the model genuinely actionable for campaign prospecting, `duration` was removed during production pipeline modeling to prevent target leakage.
2. **Macroeconomic Climate Dominates Subscription Rates:**
   - Feature importance and coefficient analysis reveal that broader economic indicators—specifically the Euribor 3-month rate (`euribor3m`) and employment variation rate (`emp.var.rate`)—drive conversions more than individual demographic factors.
   - Campaigns launched during periods of declining interest rates and negative employment variation experienced significantly higher conversion rates.
3. **Prior Engagement History:**
   - Clients with a successful outcome in a previous campaign (`poutcome = success`) showed the highest propensity to subscribe again.
   - Contacting clients fewer times during a campaign yielded better conversion efficiency; excessive follow-ups (`campaign > 4`) showed diminishing returns and client irritation.

## Model Performance Summary (Test Set)
Models were evaluated using **ROC-AUC** as the primary metric due to the severe class imbalance (~88.7% "no" vs. ~11.3% "yes"). ROC-AUC measures rank-ordering capacity across all probability thresholds.

| Classifier | Optimized Hyperparameters | Test ROC-AUC | Test PR-AUC | Train Time (s) | Inference Latency (ms) |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Logistic Regression** | `C=0.1`, `penalty='l2'` | **0.789** | 0.432 | **0.42** | **0.02** |
| **Decision Tree** | `max_depth=6`, `min_samples_split=20` | 0.774 | 0.405 | 0.65 | 0.03 |
| **K-Nearest Neighbors** | `n_neighbors=25`, `weights='distance'` | 0.751 | 0.362 | 0.05 | 38.40 |
| **Support Vector Classifier** | `C=1.0`, `kernel='rbf'` | 0.781 | 0.419 | 184.20 | 12.80 |

## Actionable Recommendations & Next Steps
1. **Tiered Lead Routing:** Implement Logistic Regression scoring to assign incoming leads to high-, medium-, and low-propensity tiers. Restrict outbound calling capacity to the top 20% decile, where over 60% of total conversions reside.
2. **Economic Timing:** Align major telemarketing pushes with periods following European Central Bank interest rate cuts, when deposit products become relatively more attractive.
3. **Contact Cap Policy:** Impose an operational hard cap of 3 contacts per campaign per customer. 

## Repository Structure
├── README.md                          <- Project overview and business recommendations

├── prompt_III.ipynb                   <- Clean, fully documented Jupyter Notebook (CRISP-DM)

└── data/

      ├── bank-additional.csv            <- 10% sample dataset (4,119 rows)

      ├── bank-additional-full.csv       <- Full dataset (41,188 rows)

      └── bank-additional-names.txt      <- Feature metadata and documentation


## Citation
  This dataset is publicly available for research. The details are described in [Moro et al., 2014]. 
  Please include this citation if you plan to use this database:

  [Moro et al., 2014] S. Moro, P. Cortez and P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. Decision Support Systems, In press, http://dx.doi.org/10.1016/j.dss.2014.03.001

  Available at: [pdf] http://dx.doi.org/10.1016/j.dss.2014.03.001
                [bib] http://www3.dsi.uminho.pt/pcortez/bib/2014-dss.txt