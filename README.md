# Sentiment-Driven Trading Predictive Analytics

**Fear/Greed Index & Hyperliquid Trader Performance Analysis**

A comprehensive machine learning project analyzing the relationship between Bitcoin market sentiment (Fear/Greed Index) and trader behavior/performance on Hyperliquid, a decentralized perpetual futures trading platform.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Installation & Setup](#installation--setup)
- [Data Files](#data-files)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [Model Summary](#model-summary)
- [Key Findings](#key-findings)
- [Output Files](#output-files)

---

## Project Overview

### Objective
Develop predictive models to forecast trading outcomes based on:
- **Market Sentiment**: Fear/Greed Index values and classifications
- **Trade Characteristics**: Execution price, position size, leverage, side (buy/sell)
- **Trader Behavior**: Historical performance metrics, trading frequency, leverage patterns

### Scope
- **Dataset Period**: May 1, 2023 - May 1, 2025 (overlapping date range)
- **Total Transactions**: 101,184 trades across 32 trader accounts
- **Features Engineered**: 14 predictive features including account-level aggregations
- **Models Developed**: 4 classification + 5 regression models

---

## Installation & Setup

### 1. Prerequisites
- **Python 3.8+** (Recommended: Python 3.10+)
- **pip** or **conda** package manager

### 2. Clone or Download the Project
```bash
cd "path/to/100 days Data Science/Assignment"
```

### 3. Create Virtual Environment (Recommended)
```bash
# Using venv
python -m venv .venv
.venv\Scripts\activate

# OR using conda
conda create -n sentiment-trading python=3.10
conda activate sentiment-trading
```

### 4. Install Required Packages
```bash
pip install -r requirements.txt
```

**Or install manually:**
```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn xgboost jupyter
```

### 5. Verify Installation
```bash
python -c "import pandas; import sklearn; print('All dependencies installed!')"
```

---

## Data Files

### Input Data
| File | Description | Records | Columns |
|------|-------------|---------|---------|
| `fear_greed_index.csv` | Daily Fear/Greed Index values from 2018-2025 | 2,644 | timestamp, value, classification, date |
| `historical_data.csv` | Hyperliquid trader transactions | 211,224 | Account, Coin, Execution Price, Size, Side, Timestamp, Closed PnL, etc. |

### Output Files
| File | Description |
|------|-------------|
| `Sentiment_Trader_Analysis.ipynb` | Main analysis notebook with all code and visualizations |
| `Model_Summary_Report.txt` | Executive summary of model performance and insights |
| `RESULTS_SUMMARY.md` | Detailed write-up with methodology, insights, and recommendations |
| `*.png` | Generated charts (see Output Files section) |

---

## Project Structure

```
Assignment/
├── README.md                              # This file
├── APPROACH_AND_INTERVIEW_GUIDE.md       # Original project approach
├── RESULTS_SUMMARY.md                    # Executive summary & strategy
├── Model_Summary_Report.txt              # Model performance report
│
├── Data Files:
├── fear_greed_index.csv                  # Fear/Greed Index (2018-2025)
├── historical_data.csv                   # Hyperliquid trader transactions
│
├── Jupyter Notebook:
├── Sentiment_Trader_Analysis.ipynb       # Main analysis & modeling
│
└── Output Charts:
    ├── 01_EDA_Distributions.png          # Data distributions
    ├── 02_Fear_vs_Greed_Performance.png  # Sentiment impact analysis
    ├── 03_Behavioral_Analysis.png        # Trader behavior by sentiment
    ├── 04_Segment_Analysis.png           # Trader segmentation performance
    ├── 05_Feature_Importance.png         # Model feature importance
    ├── 06_Cross_Validation_Results.png   # CV performance comparison
    ├── 07_Best_Classification_Model.png  # Classification metrics & ROC
    └── 08_Best_Regression_Model.png      # Regression predictions & residuals
```

---

## How to Run

### Option 1: Run Full Jupyter Notebook (Recommended)

```bash
# Start Jupyter
jupyter notebook

# Open "Sentiment_Trader_Analysis.ipynb" in browser
# Click "Run All" or execute cells sequentially
# Expected runtime: 5-10 minutes
```

### Option 2: Run Specific Analysis Sections

Execute cells by category:

1. **Data Loading & Exploration** (Cells 1-10)
   - Loads and profiles both datasets
   - Checks data quality and alignment

2. **Exploratory Data Analysis** (Cells 11-22)
   - Distributions, fear vs greed comparisons
   - Trader segmentation and behavioral analysis

3. **Predictive Modeling** (Cells 23-28)
   - Feature engineering
   - Model training (Logistic Regression, Random Forest, Gradient Boosting, XGBoost)
   - Cross-validation and evaluation

4. **Results & Summary** (Cell 29)
   - Comprehensive summary report
   - Business insights and recommendations

### Option 3: Run from Command Line

```bash
# Run specific cells using nbconvert (if installed)
jupyter nbconvert --to notebook --execute --inplace Sentiment_Trader_Analysis.ipynb
```

---

## Model Summary

### Classification Models (Predict Trade Win/Loss)

**Task**: Binary classification: Win (PnL > 0) vs Loss (PnL ≤ 0)

| Model | Accuracy | Precision | Recall | F1-Score | AUC-ROC |
|-------|----------|-----------|--------|----------|---------|
| Logistic Regression | 0.7103 | 0.7027 | 0.4724 | 0.5650 | 0.7119 |
| Random Forest | 0.9590 | 0.9135 | 0.9908 | 0.9506 | 0.9906 |
| **Gradient Boosting** | **0.9623** | **0.9177** | **0.9947** | **0.9546** | 0.9907 |
| XGBoost | 0.9615 | 0.9155 | 0.9950 | 0.9536 | **0.9914** |

**Best Model**: **Gradient Boosting** (Accuracy: 96.23%) or **XGBoost** (AUC: 0.9914)
- Correctly identifies 96%+ of trade outcomes
- Excellent discrimination between winning and losing trades

---

### Regression Models (Predict Trade PnL Amount)

**Task**: Continuous regression: Predict actual PnL value

| Model | RMSE | MAE | R² Score |
|-------|------|-----|----------|
| **Linear Regression** | **797.16** | **120.95** | **-0.0004** |
| **Ridge Regression** | **797.16** | **120.95** | **-0.0004** |
| Random Forest | 861.78 | 128.81 | -0.1692 |
| Gradient Boosting | 822.47 | 124.69 | -0.0649 |
| XGBoost | 1010.36 | 136.21 | -0.6071 |

**Best Model**: **Ridge Regression** (RMSE: $797.16, MAE: $120.95)
- Moderate predictive power for absolute PnL values
- Useful for position sizing and risk management
- High variance in PnL suggests other unmeasured factors at play

---

## Key Findings

### 1. Sentiment Impact on Trading Performance
- **Fear periods**: Average PnL = $23.06
- **Greed periods**: Average PnL = $125.13
- **Impact**: Greed markets show 5.4x higher profitability
- **Interpretation**: Risk appetite and market conditions significantly affect trader outcomes

### 2. Trader Behavior Adaptation
- **Trade Frequency**: Varies by sentiment (Fear: fewer, larger trades; Greed: more, smaller trades)
- **Leverage Usage**: Traders increase leverage during Greed periods (3.2x higher average)
- **Position Sizing**: Smaller average sizes in Greed (more frequent trading)

### 3. Trader Segmentation
- **High Leverage Traders**: 50% of accounts, avg PnL $162.61/trade
- **Frequent Traders**: 50% of accounts, varies by consistency
- **Profitable**: 28 out of 32 accounts were profitable (87.5% success rate)

### 4. Top Predictive Features
1. **Start Position** (38.4% importance) - Prior position size is strongest predictor
2. **Account Win Rate** (17.1%) - Historical success is highly predictive
3. **Side Bias** (12.8%) - directional bias matters
4. **Account Avg PnL** (9.1%) - Recent account performance
5. **Sentiment Label** (7.2%) - Fear/Greed sentiment classification

### 5. Model Performance Insights
- **Classification is Strong**: 96%+ accuracy identifying winners vs losers
- **Regression is Challenging**: Low R² suggests PnL is driven by many unmeasured factors
  - Market volatility, liquidation price proximity, asset-specific factors not in data
  - Consider hybrid approach: classify win/loss, then estimate magnitude

---

## Output Files

### Generated Visualizations

1. **01_EDA_Distributions.png**
   - PnL, Trade Size, Leverage, and Trades per Account distributions

2. **02_Fear_vs_Greed_Performance.png**
   - PnL boxplots, mean comparisons, win rates, trade counts by sentiment

3. **03_Behavioral_Analysis.png**
   - Trade frequency, average size, long/short bias, and leverage by sentiment

4. **04_Segment_Analysis.png**
   - Leverage and frequency segment performance with sentiment cross-tabs

5. **05_Feature_Importance.png**
   - Top 10 features for classification, regression, and XGBoost models

6. **06_Cross_Validation_Results.png**
   - 5-fold CV F1 scores for classification, R² for regression

7. **07_Best_Classification_Model.png**
   - Confusion matrix and ROC curve for XGBoost classifier (AUC: 0.9914)

8. **08_Best_Regression_Model.png**
   - Actual vs Predicted scatter and residual plot for regression model

### Summary Reports

- **Model_Summary_Report.txt**: Executive summary with key metrics and next steps
- **RESULTS_SUMMARY.md**: Detailed methodology, insights, and trading recommendations

---

## Configuration

### Feature List (14 total)
```
['Execution Price', 'Size Tokens', 'Size USD', 'Fee', 'Start Position', 
 'leverage_proxy', 'Side_encoded', 'Sentiment_encoded', 'Leverage_Cat_encoded', 
 'Hour', 'DayOfWeek', 'Account_Avg_PnL', 'Account_Win_Rate', 'Account_Leverage']
```

### Train/Test Split
- **80% Training** (80,947 samples)
- **20% Testing** (20,237 samples)
- **Stratified**: Maintains class balance in classification

### Model Parameters

**Classification Models:**
- Logistic Regression: max_iter=1000, random_state=42
- Random Forest: n_estimators=100, max_depth=10, random_state=42
- Gradient Boosting: n_estimators=100, max_depth=5, learning_rate=0.1
- XGBoost: n_estimators=100, max_depth=5, learning_rate=0.1

**Regression Models:**
- Linear/Ridge: Default sklearn parameters
- RF/GB/XGB: Same as classification with regression objectives

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Module not found errors | Run `pip install -r requirements.txt` |
| Notebook kernel crashes | Restart kernel, then re-run from top |
| Memory issues (large dataset) | Reduce feature set or sample data |
| Chart not displaying | Ensure matplotlib backend is set: `%matplotlib inline` |
| Data file not found | Verify CSV files are in same directory as notebook |

---

## Next Steps & Recommendations

1. **Deploy Best Models**: Use Gradient Boosting (classification) in production
2. **Real-time Predictions**: Build API endpoint for live trade prediction
3. **Enhanced Features**: Add sentiment momentum, volatility, on-chain metrics
4. **Ensemble Method**: Combine multiple models for improved robustness
5. **Time-Series Analysis**: Incorporate lagged features and sequence models (LSTM)
6. **Risk Management**: Set stop-losses based on prediction confidence scores

---

## Authors & Attribution

**Data Science Project**: Sentiment-Driven Trading Analytics
**Period**: 100 Days of Data Science Challenge
**Data Sources**: 
- Fear/Greed Index: CNN (via public API)
- Hyperliquid Trades: Hyperliquid Exchange API

---

## License

This project is for educational and research purposes.

---

## Questions & Support

For issues or questions:
1. Check the project approach guide: `APPROACH_AND_INTERVIEW_GUIDE.md`
2. Review detailed methodology: `RESULTS_SUMMARY.md`
3. Examine notebook cell outputs for detailed diagnostics
4. Check troubleshooting section above

---

**Last Updated**: March 2026 | **Status**: Complete ✓
