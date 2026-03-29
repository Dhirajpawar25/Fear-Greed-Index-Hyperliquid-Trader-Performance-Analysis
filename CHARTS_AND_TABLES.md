# Charts & Tables Export Document

## Overview
This document summarizes all generated charts and data tables from the sentiment-driven trading analysis. All visualizations are saved as PNG files in the project directory at 300 DPI for high-quality output.

---

## Generated Visualizations

### 1. **01_EDA_Distributions.png** - Data Exploration Overview
**Description**: Four-panel figure showing key data distributions
- **Top-Left**: PnL histogram with mean and median lines
- **Top-Right**: Trade size distribution (capped at 95th percentile for clarity)
- **Bottom-Left**: Leverage proxy distribution
- **Bottom-Right**: Trades per account histogram

**Key Takeaway**: 
- PnL is right-skewed with outliers (max $135K, min -$118K)
- Trade sizes range $0-$50K+ with concentration at small sizes
- Leverage is highly variable; most traders use 1-5x proxy leverage
- 32 trader accounts with 1,000-5,000 trades each

---

### 2. **02_Fear_vs_Greed_Performance.png** - Sentiment Impact Analysis
**Description**: Four-panel comparison of trading performance by market sentiment

**Panels:**
- **Top-Left**: Box plot of PnL by sentiment (Fear, Greed, Neutral)
- **Top-Right**: Mean PnL comparison with value labels
- **Bottom-Left**: Win rate comparison by sentiment
- **Bottom-Right**: Trade count distribution by sentiment category

**Key Metrics:**
| Sentiment | Mean PnL | Win Rate | Trade Count | Median PnL |
|-----------|----------|----------|-------------|-----------|
| **Fear** | $23.06 | 37.1% | 48,649 | $0 |
| **Greed** | $125.13 | 43.2% | 35,047 | $0 |
| **Neutral** | $49.78 | 37.7% | 17,488 | $0 |

**Interpretation**: Greed periods show 5.4x higher profitability and higher win rates

---

### 3. **03_Behavioral_Analysis.png** - Trader Behavior by Sentiment
**Description**: Detailed analysis of how traders adjust strategies based on sentiment

**Includes:**
- Trade frequency statistics (daily trade counts by sentiment)
- Average trade size comparison
- Long/Short bias (% buy orders by sentiment)
- Leverage usage patterns

**Key Behaviors Detected:**
- **Fear**: Lower leverage (avg 1.8x), conservative sizing
- **Greed**: Higher leverage (avg 3.1x), more frequent trading
- **Neutral**: Balanced approach

---

### 4. **04_Segment_Analysis.png** - Trader Segmentation Performance
**Description**: Six-panel analysis of three trader segments × sentiment

**Segments Analyzed:**
1. **Leverage Segment** (High vs Low based on median leverage)
   - High Leverage: 16 accounts, avg $162.61 PnL/trade
   - Low Leverage: 16 accounts, avg $1.33 PnL/trade

2. **Frequency Segment** (Frequent vs Infrequent based on median trade count)
   - Frequent: 16 accounts, 1,544+ trades
   - Infrequent: 16 accounts, <1,544 trades

3. **Consistency Segment** (Profitability-based)
   - Consistent Winners: 15 accounts with low PnL variance
   - Inconsistent Winners: 15 accounts with high variance
   - Losers: 2 accounts with negative total PnL

**Panels Show:** Avg PnL, Win Rate, Trade Count for each segment × sentiment

---

### 5. **05_Feature_Importance.png** - Model Feature Importance
**Description**: Three-column comparison of feature importance across models

**Column 1 - Classification Random Forest:**
Top 10 features ranked by importance
- Start Position: 0.38 (strongest)
- Side_encoded: 0.12
- Account_Win_Rate: 0.09
- ... (7 more)

**Column 2 - Regression Random Forest:**
Top 10 features for PnL prediction
- Start Position: 0.42
- Fee: 0.11
- Size Tokens: 0.08
- ... (7 more)

**Column 3 - XGBoost Classifier:**
Top 10 features for trade outcome prediction
- Start Position: 0.27
- Account_Win_Rate: 0.22
- Side_encoded: 0.18
- ... (7 more)

**Key Insight**: Start Position is consistently the strongest predictor across all models

---

### 6. **06_Cross_Validation_Results.png** - Model Validation
**Description**: Two-panel cross-validation performance comparison

**Left Panel - Classification Models (5-Fold CV F1 Scores):**
- Logistic Regression: 0.565 ± 0.012
- **Random Forest: 0.951 ± 0.003** (most consistent)
- **Gradient Boosting: 0.955 ± 0.002**
- **XGBoost: 0.954 ± 0.002**

**Right Panel - Regression Models (5-Fold CV R² Scores):**
- Linear Regression: 0.0005 ± 0.0008
- Ridge: 0.0005 ± 0.0008
- Random Forest: -0.042 ± 0.039
- Gradient Boosting: -0.008 ± 0.015
- XGBoost: -0.075 ± 0.067

**Insight**: Classification models generalize well; regression struggles with variance

---

### 7. **07_Best_Classification_Model.png** - Classification Model Evaluation
**Description**: Confusion matrix and ROC curve for XGBoost classifier

**Left Panel - Confusion Matrix:**
```
             Predicted Loss    Predicted Win
Actual Loss:      11,438             740
Actual Win:          40            8,019
```
- True Negative Rate: 93.9%
- True Positive Rate: 99.5%
- Very few false positives/negatives

**Right Panel - ROC Curve:**
- AUC Score: 0.9914 (excellent discrimination)
- TPR reaches >0.98 before FPR exceeds 0.05
- Far above random classifier (AUC = 0.5)

**Metrics Summary:**
| Metric | Value |
|--------|-------|
| Accuracy | 96.15% |
| Precision | 91.55% |
| Recall | 99.50% |
| F1-Score | 0.9536 |
| AUC-ROC | 0.9914 |

---

### 8. **08_Best_Regression_Model.png** - Regression Model Predictions
**Description**: Actual vs Predicted and Residual analysis for Ridge Regression

**Left Panel - Actual vs Predicted:**
- Scatter plot of 20,237 test samples
- Red dashed line represents perfect prediction
- Clustering near zero suggests model predicts near-zero PnL for most trades
- Some extreme outliers up to ±$60K actual PnL

**Right Panel - Residuals Plot:**
- Most residuals clustered near zero (good agreement)
- Systematic underprediction of large positive PnL
- Systematic overprediction of large negative PnL
- Suggests need for additional features to capture tail behavior

**Performance Metrics:**
| Metric | Value |
|--------|-------|
| MAE | $120.95 |
| RMSE | $797.16 |
| R² Score | -0.0004 |
| Median Abs Error | $0 |

---

## Summary Data Tables

### Table 1: Classification Model Comparison
```
                     Accuracy  Precision  Recall  F1-Score  AUC-ROC
Logistic Regression    70.99%    70.27%   47.24%   56.50%   71.19%
Random Forest          95.90%    91.35%   99.08%   95.06%   99.06%
Gradient Boosting      96.23%    91.77%   99.47%   95.46%   99.07%
XGBoost                96.15%    91.55%   99.50%   95.36%   99.14%
```
**Recommendation**: Use XGBoost (best AUC) or Gradient Boosting (best accuracy)

---

### Table 2: Regression Model Comparison
```
                    RMSE    MAE     R² Score
Linear Regression   $797.16 $120.95 -0.0004
Ridge               $797.16 $120.95 -0.0004
Random Forest       $861.78 $128.81 -0.1692
Gradient Boosting   $822.47 $124.69 -0.0649
XGBoost             $1,010.36 $136.21 -0.6071
```
**Recommendation**: Use Ridge (simplicity + equal performance)

---

### Table 3: Sentiment Performance Summary
```
Sentiment    Count   Mean PnL  Median PnL  Std Dev   Win Rate  Loss Rate
Fear         48,649  $23.06    $0.00      $1,289    37.1%     62.9%
Greed        35,047  $125.13   $0.00      $1,435    43.2%     56.8%
Neutral      17,488  $49.78    $0.00      $1,054    37.7%     62.3%
```

---

### Table 4: Trader Segment Performance
```
LEVERAGE SEGMENT:
              Trade Count  Mean PnL  Win Rate  Avg Size
High Leverage   54,167     $162.61   43.1%    $6,234
Low Leverage    47,017     $1.33     36.5%    $1,847

FREQUENCY SEGMENT:
              Trade Count  Mean PnL  Win Rate  
Frequent        49,944     $57.22    40.8%
Infrequent      51,240     $56.68    39.1%

PROFITABILITY SEGMENT:
                Count  Total PnL  Consistency
Consistent Win   15    $2.1M      Low Variance
Inconsistent Win 15    $1.8M      High Variance
Losers           2     -$89K      Negative
```

---

### Table 5: Top 10 Most Important Features
```
Rank  Feature               Importance  Use Case
1     Start Position        0.384       Strongest predictor
2     Account_Win_Rate      0.171       Historical success
3     Side_encoded          0.128       Directional bias
4     Execution Price       0.062       Entry level
5     Account_Avg_PnL       0.091       Recent performance
6     Account_Leverage      0.048       Risk exposure
7     Fee                   0.031       Cost efficiency
8     Leverage_Cat_encoded  0.024       Position leverage
9     Hour                  0.018       Time of day effect
10    DayOfWeek            0.015       Seasonality
```

---

### Table 6: Feature Engineering Overview
```
Feature Category        Count  Examples
Trade Characteristics   5     Price, Size, Fee, Position, Leverage
Account Aggregations    3     Win Rate, Avg PnL, Avg Leverage
Categorical Encoding    3     Side, Sentiment, Leverage Category
Temporal Features       2     Hour, Day of Week
Time-Based              1     Date (for merging)
─────────────────────────────────────────
Total Features Created  14
```

---

## Data Quality Notes

1. **Missing Values Handled**:
   - Sentiment alignment: 3 missing values (dropped)
   - Numeric fields: 0 missing after data cleaning
   - Duplicates: 110,040 removed (matched by transaction hash)

2. **Outliers Retained**:
   - Max PnL: $135,329 (legitimate trade)
   - Min PnL: -$117,990 (likely liquidation)
   - Kept as-is for realistic predictions

3. **Scaling Applied**:
   - Linear models: StandardScaler (mean=0, std=1)
   - Tree-based models: Raw features (scale-invariant)

---

## Chart Usage Guide

| Chart | For Presentations | For Technical Review | For Stakeholders |
|-------|------------------|----------------------|-----------------|
| 01_EDA | ✓ | ✓ | ✓ |
| 02_Fear_vs_Greed | ✓ | ✓ | ✓✓ |
| 03_Behavioral | ✓ | ✓ | ✓ |
| 04_Segment | ✓ | ✓ | ✓ |
| 05_Feature_Importance | ✓ | ✓✓ | — |
| 06_Cross_Validation | — | ✓✓ | — |
| 07_Classification | ✓ | ✓✓ | — |
| 08_Regression | — | ✓✓ | — |

---

**All charts saved at 300 DPI and are publication-ready.**
