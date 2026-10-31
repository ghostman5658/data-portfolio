# Combined Cycle Power Plant: Data Exploration and Analysis
## Part 2 — Narrative Summary

**Kyle Gotzman**

---

## Problem Statement

How do ambient conditions—temperature, pressure, humidity, and exhaust vacuum—affect electrical power output in a combined cycle power plant? Can we predict power output and classify operating regimes using statistical and machine learning methods? This project uses data from the UCI Machine Learning Repository, collected from a plant over six years (2006–2011) at full load, with nearly 10,000 hourly observations.

---

## Key Findings and Insights

**Data Quality and Preparation**

The dataset had no missing values. Box plots revealed outliers in ambient pressure (AP) and relative humidity (RH), but not in temperature (AT), exhaust vacuum (V), or power output (PE). Rather than trimming or removing these points, we applied standardization. The outliers were few and not extreme, and our models (Random Forest, K-means) are robust to them. Standardization scaled all variables to comparable ranges and prepared the data for clustering and machine learning.

**Variable Relationships**

Covariance and Pearson correlation showed that power output is strongly tied to ambient temperature and exhaust vacuum. As temperature or vacuum rises, power output falls (correlations of −0.95 and −0.87). Ambient pressure and relative humidity have moderate positive links to PE (0.52 and 0.39). Temperature and vacuum move together (0.84), so they carry similar information; our models handled this overlap without issue.

**[INSERT VISUALIZATION 1: Pearson Correlation Heatmap]**
*Recommended: The correlation heatmap from Section 6. It shows the strength and direction of relationships between all variables and highlights AT and V as the main drivers of PE.*

**Distribution and Operating Regimes**

Histograms showed bimodal distributions for temperature and power output—two distinct peaks suggesting different operating regimes (e.g., cooler vs. warmer conditions). A t-test confirmed that mean power output differs significantly between high- and low-temperature conditions. An F-test indicated unequal variance between the two groups, so we ran Welch’s t-test, which does not assume equal variance. The conclusion held: temperature clearly affects power output.

**Regression and Residuals**

Linear regression achieved R² ≈ 0.93 and RMSE ≈ 4.54. Residual plots showed unequal spread (wider at lower predicted values) and heavy tails. We tried a log(PE) model to stabilize variance. The log transform improved residual behavior—more even spread and a better QQ plot—but R² and RMSE stayed similar. The benefit of the log model is better assumptions for inference, not higher predictive accuracy.

**Classification and Clustering**

Logistic regression classified high vs. low power output (above or below the median) with 95% accuracy and AUC 0.99. K-means clustering with k=3 identified three groups that align with low, medium, and high output regimes. A PCA projection showed the clusters mostly separated, with some overlap in the center where conditions blend. This supports the bimodal structure seen in the histograms.

**Machine Learning Comparison**

Random Forest outperformed the linear models. For regression, R² rose to 0.96 and RMSE fell to 3.31. For classification, accuracy reached 97%, with precision and recall around 0.96–0.98 for both classes. Random Forest captures non-linear patterns—such as the banded relationship between vacuum and power output—that linear models miss.

**[INSERT VISUALIZATION 2: Temperature vs. Power Output Scatter Plot OR K-means PCA Cluster Plot]**
*Recommended: Either the scatter plot of AT vs. PE (shows the strong negative relationship) or the K-means PCA plot (shows the three operating regimes). Choose the one that best supports your narrative.*

---

## Recommendations

1. **Operational focus:** Temperature and exhaust vacuum are the strongest predictors of power output. Operators should prioritize monitoring and controlling these variables to maintain or improve efficiency.

2. **Model choice:** For prediction, Random Forest is preferred over linear regression. For interpretability or when confidence intervals matter, the log(PE) linear model is a reasonable alternative.

3. **Regime awareness:** The plant operates in distinct regimes (low, medium, high output). Clustering and classification can support regime-based control and maintenance planning.

4. **Limitations:** The data come from a single plant over a fixed period. Results may not generalize to other plants or different operating conditions. Further work could include validation on additional plants or time periods.

---

*Word count: ~650 words*
