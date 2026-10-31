# Predicting Positive and Negative Market Segment Reactions to Tariff Announcements

**Kyle Gotzman | Spring 2026**

---

## 1. Introduction

### 1.1 The Problem

Tariff announcements have been front and center in the news, with wide-ranging implications across global financial markets. Some argue that they are good for markets whereas others are convinced they are detrimental. However, these policy actions do not affect all industries equally. Some market segments may decline due to higher input costs, supply-chain disruption, or export exposure, while other segments may benefit from protectionist policies or domestic substitution effects. Understanding this heterogeneity is critical for firms navigating an increasingly uncertain trade environment.

This project examines whether tariff announcements can predict positive or negative short-term returns across different market segments, helping firms identify which industries are most vulnerable and which may benefit from trade-policy shocks.

### 1.2 Why This Problem Matters

The business value of this analysis is significant across multiple stakeholder groups:

- **Investment firms** can use these insights to inform sector allocation decisions following tariff announcements.
- **Manufacturers** can assess which upstream or downstream industries face the greatest risk.
- **Procurement teams** can anticipate cost pressures in trade-exposed industries.
- **Corporate strategy teams** can identify sectors that may gain competitive advantage from protectionist policy.

### 1.3 Pitching to Stakeholders

If pitching this analysis to a group of stakeholders, the argument would center on the following: tariff announcements are not random noise, they systematically create winners and losers among market sectors. A predictive model that identifies which sectors will react positively or negatively provides an actionable edge for portfolio positioning, risk management, and strategic planning. The 2018-2025 tariff cycle offers a rich natural experiment that we can learn from to anticipate future trade shocks.

### 1.4 Data Sources

The data for this project comes from two primary sources:

1. **Sector Impact Dataset** --- Over 33,000 daily observations of sector ETF performance across 15 U.S. market sectors (including Technology, Agriculture, Steel & Metals, Semiconductors, Automobiles, and others). Features include daily and weekly returns, tariff-sensitivity ratings, and volatility metrics.

2. **Tariff Timeline Dataset** --- A catalog of 45 major tariff actions from 2018 through 2025, including the imposing country, targeted country, sector, tariff rate changes, legal basis, and estimated trade value affected.

Additional supplementary data included S&P 500 daily returns and 20-day volatility from a stock market reaction dataset, used to capture prevailing market conditions.

---

## 2. Milestone 1: Exploratory Data Analysis

### 2.1 Approach

The EDA phase focused on characterizing the distribution of post-tariff returns across sectors and establishing whether tariff announcements produce heterogeneous market reactions. Each tariff announcement date was mapped to the nearest actual trading day (within a 3-day tolerance), yielding 33 unique tariff-event trading days across 15 U.S. sector ETFs and 528 sector-event observations.

### 2.2 Key Visuals and Findings

**Average Post-Tariff Returns by Sector:** Bar charts of mean same-day returns revealed that reactions vary meaningfully across sectors. Financials and Energy posted the most negative average returns, while Semiconductors and Technology led positively. This confirmed the core hypothesis that tariff shocks create winners and losers rather than one uniform response.

**Return Distributions:** Boxplots showed that even sectors with near-zero average returns exhibit wide interquartile ranges, meaning the direction of the reaction depends on event-specific features (tariff size, target country, etc.), not just sector identity alone.

**Heatmap of Sector Co-Reactions:** A correlation heatmap of post-tariff daily returns showed that broad-based tariffs tend to move most sectors in the same direction, while more targeted actions produce differentiated sector patterns.

**Positive vs. Negative Reaction Counts:** A stacked bar chart of positive vs. negative reaction days by sector confirmed that most sectors have a fairly balanced mix, making this a non-trivial binary classification task.

### 2.3 Milestone 1 Conclusion

Tariff announcements affect sectors unevenly. The balanced class split (positive vs. negative) makes this well-suited for binary classification.

---

## 3. Milestone 2: Data Preparation

### 3.1 Cleaning and Scoping

- Dropped irrelevant columns from both datasets (e.g., `source`, `notes`, `open`, `high`, `low`, `close`, `volume`).
- Restricted the sector impact data to U.S. sector ETFs only (11 sectors after filtering).
- Rebuilt the event-returns dataset from U.S. only data, resulting in 363 sector-event observations across 33 tariff-event trading days.

### 3.2 Feature Engineering

Features were organized into four groups:

| Feature Group | Features | Description |
|---|---|---|
| **Tariff-event** | `max_tariff_rate_delta`, `max_tariff_rate_pct`, `max_trade_value_bn`, `n_events`, `any_retaliation`, `is_usa_imposing` | Aggregated tariff characteristics for each event date |
| **Market-context** | `sp500_return_prev1d`, `sp500_volatility_20d` | Prior-day S&P 500 return and 20-day realized volatility |
| **Sector-level** | `volatility_10d`, `sector_momentum_5d`, `tariff_sensitivity_ord` | Sector-specific recent performance and sensitivity |
| **Sector dummies** | 10 one-hot columns | Sector fixed effects (drop-first encoding) |

**Leakage prevention:** The same-day S&P 500 return was replaced with the prior-day value (`sp500_return_prev1d`) to avoid information leakage, since the S&P 500 return on the same day would contain information about the target variable.

**Merge duplication fix (added since Milestone 3):** A bug was identified where multiple tariff announcement dates mapping to the same trading day caused row duplication during the merge. A re-aggregation step was added after the trading-date mapping to ensure each trading day appears only once in the tariff-event feature table.

### 3.3 Target Definition

- Neutral-return observations (exactly zero) were removed.
- A binary target variable was defined: `1` for positive daily return, `0` for negative.
- Final dataset: **362 rows, 21 predictors**.
- Class balance: 57% positive, 43% negative (reasonably balanced).

---

## 4. Milestone 3: Model Building and Evaluation

### 4.1 Model Selection

Three classifiers were selected for this small (362-row), tabular, binary classification task:

| Model | Rationale |
|---|---|
| **Logistic Regression** | Interpretable baseline; L2 regularization handles collinearity |
| **Random Forest** | Captures non-linear interactions; built-in feature importance |
| **Gradient Boosting** | Sequential learner; often best on structured data |

**Primary metric: F1 (macro)** --- balances precision and recall for both classes.
**Secondary metric: ROC-AUC** --- threshold-independent discriminative power.

### 4.2 Cross-Validation Results

All models were evaluated using 5-fold stratified cross-validation with `StandardScaler` inside the pipeline to prevent test-fold leakage.

| Model | F1 (macro) | ROC-AUC | Accuracy |
|---|---|---|---|
| **Random Forest** | 0.781 +/- 0.041 | 0.881 +/- 0.026 | 0.789 +/- 0.035 |
| Gradient Boosting | 0.764 +/- 0.043 | 0.838 +/- 0.030 | 0.772 +/- 0.040 |
| Logistic Regression | 0.620 +/- 0.064 | 0.662 +/- 0.075 | 0.626 +/- 0.066 |

**Random Forest was selected as the best model** based on the highest cross-validated F1 (macro) score.

### 4.3 Test Set Evaluation

An 80/20 stratified split was used (289 training / 73 test). The Random Forest achieved:

- **Test F1 (macro): 0.774**
- **Test ROC-AUC: 0.835**
- **Test Accuracy: 0.795**

The model demonstrates strong discriminative ability, correctly distinguishing positive from negative sector reactions approximately 80% of the time.

### 4.4 Feature Importance

The top predictors from the Random Forest's impurity-based feature importance were:

1. **sp500_volatility_20d** (0.137) --- Prevailing market volatility
2. **sector_momentum_5d** (0.135) --- Pre-event 5-day sector momentum
3. **sp500_return_prev1d** (0.129) --- Prior-day broad market direction
4. **volatility_10d** (0.121) --- Sector-specific recent volatility
5. **max_trade_value_bn** (0.098) --- Estimated trade value affected by the tariff

These results directly connect to the business hypothesis: both market conditions and tariff severity influence sector reactions. The dominance of market-context features suggests that general market sentiment on the day of a tariff announcement heavily moderates the sector-level response.

---

## 5. Content Added Since Milestone 3

### 5.1 Hyperparameter Tuning

**Feedback addressed:** "Add hyperparameter tuning for Random Forest."

A randomized search over 80 parameter combinations was applied to the Random Forest, tuning `n_estimators`, `max_depth`, `min_samples_split`, `min_samples_leaf`, and `max_features`.

**Best parameters found:**
- `max_depth`: 10
- `max_features`: sqrt
- `min_samples_leaf`: 2
- `min_samples_split`: 3
- `n_estimators`: 319

**Tuned model results:**
- Test F1 (macro): 0.787
- Test ROC-AUC: 0.835

The tuned model shows a marginal improvement in F1 over the untuned baseline (0.787 vs. 0.774), while ROC-AUC remained the same. This marginal improvement is expected given the small dataset size, the primary constraint is data volume, not model configuration.

### 5.2 Do Same-Day Returns Fully Capture Market Reactions?

**Feedback addressed:** "Discuss whether same-day returns fully capture delayed market reaction."

A multi-day event window analysis was conducted, comparing average cumulative sector returns over 1-day, 3-day, and 5-day post-tariff windows.

**Key findings:**

- **Directional consistency:** Most sectors maintain the same return sign across all three windows, suggesting that same-day returns do capture the general direction of the tariff reaction.
- **Magnitude amplification:** For some sectors, the magnitude of the cumulative return grows from 1-day to 5-day, indicating that the full market response may take several days to materialize.
- **Sign reversals observed:** Five sectors (Industrials, Healthcare, Financials, Agriculture, and Retail) showed opposite signs between the 1-day and 5-day average returns, suggesting that initial same-day reactions in these sectors may represent noise or overreaction that reverses in subsequent trading days.

**Implication:** While a same-day event window is a reasonable baseline, a 3-day or 5-day cumulative return target could better capture the fundamental tariff impact and reduce noise from intraday volatility. Future model iterations should explore multi-day targets.

### 5.3 Data Pipeline Fixes

**Feedback addressed:** "Fix row-count inconsistencies" and "Investigate why feature engineering increased event rows."

- A merge duplication bug was identified and fixed in the feature-engineering step: multiple tariff announcement dates could map to the same nearest trading day, causing row inflation during the left merge. A re-aggregation step was added after the trading-date mapping to eliminate duplicates.
- All row-count references in the narrative were updated to match actual code output (362 rows after removing neutral-return observations).
- Conclusion metrics were corrected to align with printed results (test ROC-AUC = 0.835).

---

## 6. Conclusion

### 6.1 What Does the Analysis Tell Us?

Tariff shocks create heterogeneous winners and losers across U.S. market sectors. A Random Forest classifier can predict same-day sector return direction with approximately 80% accuracy and an ROC-AUC of 0.835. The most influential predictors are market-context features (S&P 500 volatility and prior-day return), sector-specific momentum, and tariff severity (estimated trade value affected).

The correlation vs. causation limitation is important: the model identifies which features *predict* return direction on tariff days but cannot prove that tariffs *caused* those returns. The strong predictive power of prior-day S&P 500 return suggests that general market conditions heavily influence same-day sector performance, meaning a tariff announcement's impact is partly a function of the market environment in which it occurs.

### 6.2 Is This Model Ready for Deployment?

Not yet. The small sample size (362 rows from 33 unique tariff-event days) limits model complexity and generalizability. The model demonstrates strong discriminative power and confirms the business hypothesis, but several enhancements are needed before production use:

1. **Larger dataset** with more tariff events and a longer time horizon.
2. **Matched control group** of non-event days to disentangle tariff-specific effects from background market movements.
3. **Multi-day return targets** to capture delayed reactions, especially for sectors (like Financials and Healthcare) where same-day returns show sign reversals over 5 days.
4. **Additional data sources** such as news sentiment, options-implied volatility, or trade flow data.

### 6.3 Recommendations

- **Investment firms:** Use the identified sector sensitivity patterns as a screening tool for post-tariff allocation decisions. Sectors with high tariff sensitivity ratings and negative momentum should be underweighted following large tariff announcements.
- **Risk teams:** Monitor not just same-day sector reactions but also 3-5 day cumulative returns to capture delayed adjustment, particularly in Financials, Healthcare, Industrials, Agriculture, and Retail.
- **Strategy teams:** The strong influence of S&P 500 prior-day return suggests that tariff announcements made during bearish market conditions are more likely to produce widespread negative reactions across sectors.

### 6.4 Challenges and Opportunities

- **Correlation vs. causation** remains a limitation without a proper control group design.
- **Expanding the event set** beyond U.S. tariffs to include global trade actions would increase sample size and improve robustness.
- **Ensemble methods** (stacking Random Forest, Gradient Boosting, and Logistic Regression) could improve predictions for borderline cases.
- **Real-time deployment** would require an automated tariff event detection pipeline and live market data integration.
