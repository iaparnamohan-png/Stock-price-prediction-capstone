# Predicting Stock Price Movement Across US Market Sectors

**Author:** Aparna Mohan  
**Program:** UC Berkeley Professional Certificate in Machine Learning and AI

**[View Notebook on nbviewer](https://nbviewer.org/github/iaparnamohan-png/Stock-price-prediction-capstone/blob/main/capstone_final_aparnamohan.ipynb)**

---

## Executive Summary

**Project overview and goals:** This project investigates whether historical daily stock price data can predict next-day price returns across US publicly traded companies, and whether prediction accuracy varies meaningfully by sector. Three regression models: Linear Regression, Ridge, and Lasso  are trained on lagged price and volume features using a time-based train/test split to simulate real-world forecasting conditions.

**Key design decision:** Rather than predicting the next-day closing price (which creates data leakage when using same-day features like moving averages), this project predicts the **next-day return percentage** how much a stock's price will move up or down. A chronological 80/20 train/test split ensures the model is always trained on past data and tested on future data.

**Findings:** All three models performed comparably on the test set, with Ridge Regression achieving the best balance of RMSE and R². Lagged returns were the strongest predictors of next-day price movement. Sector predictability varies meaningfully, with Financial Services and Consumer Defensive sectors being the most predictable and Technology the least.

**Results summary:**

| Model | RMSE (%) | MAE (%) | R² |
|---|---|---|---|
| Linear Regression | 2.0438 | 1.3516 | 0.0004 |
| Ridge Regression (tuned) | 2.0437 | 1.3516 | 0.0005 |
| Lasso Regression (tuned) | 2.0442 | 1.3502 | -0.0000 |

**Cross-Validation Results (TimeSeriesSplit, 5 folds):**

| Model | Mean CV RMSE (%) | Std CV RMSE |
|---|---|---|
| Linear Regression | 1.8813 | 0.2094 |
| Ridge (tuned) | 1.8809 | 0.2094 |
| Lasso (tuned) | 1.8630 | 0.2057 |

**Sector Performance (Ridge Model):**

| Sector | RMSE (%) | R² |
|---|---|---|
| Financial Services | 1.6685 | -0.0038 |
| Consumer Defensive | 1.7416 | 0.0005 |
| Industrials | 1.7770 | 0.0001 |
| Communication Services | 1.8678 | 0.0054 |
| Healthcare | 2.0392 | -0.0037 |
| Energy | 2.1147 | -0.0035 |
| Basic Materials | 2.1341 | 0.0022 |
| Consumer Cyclical | 2.1493 | 0.0014 |
| Technology | 2.7417 | 0.0047 |

**Conclusion:** Ridge Regression is the recommended model. All three models achieve similar RMSE (~2.04%), confirming that short-term return prediction from price features alone is a difficult task  consistent with the efficient market hypothesis. However, the sector-level variation in RMSE confirms that some sectors are more predictable than others, directly answering the core research question.

---

## Rationale

Stock price prediction is one of the most studied problems in quantitative finance. The ability to forecast even modest directional movements has real financial value for portfolio managers, quantitative analysts, and retail investors. This project uses regression techniques covered in the program to predict next-day returns from historical price behavior. The sector-level comparison adds business insight, knowing which sectors are more forecastable helps allocate analytical resources more effectively.

Without this kind of analysis, investors are forced to treat all sectors as equally predictable, which leads to poor risk calibration and suboptimal portfolio allocation decisions.

---

## Research Question

Can historical daily OHLCV stock price data predict next-day price returns across US publicly traded companies, and does prediction accuracy vary meaningfully by sector?

---

## Data Sources

**Dataset:** US Stock Market Historical OHLCV Dataset  
**Source:** [Kaggle](https://www.kaggle.com/datasets/asadullahcreative/us-stock-market-historical-ohlcv-dataset)  
**Size:** 184,138 daily records  
**Coverage:** 120 tickers across 9 sectors, January 2020 to February 2026  
**Features:** Date, Ticker, Company Name, Sector, Industry, Open, High, Low, Close, Adj_Close, Volume  
**Sectors:** Technology, Healthcare, Financial Services, Consumer Cyclical, Basic Materials, Industrials, Consumer Defensive, Communication Services, Energy

No missing values were found. Duplicate rows were removed and data was sorted chronologically before feature engineering.

---

## Methodology

### Feature Engineering
All features use only information available before the trading day being predicted, to avoid data leakage:

| Feature | Description |
|---|---|
| Daily_Return | Today's % price change |
| Return_Lag1 | Yesterday's % price change |
| Return_Lag2 | Two days ago % price change |
| Price_Range | (High - Low) / Close — intraday volatility proxy |
| Log_Volume | Log-transformed volume |
| Volume_Change | Day-over-day volume % change |
| MA_Cross | (MA5 - MA20) / Close — momentum signal |
| DayOfWeek | Day of week (0=Monday, 4=Friday) |

**Target variable:** Next_Day_Return — the next trading day's % price change

### Train/Test Split
A time-based 80/20 chronological split is used training on older data and testing on more recent data. This simulates real-world forecasting and prevents future data from leaking into training.

### Models
1. Linear Regression: baseline, no regularization
2. Ridge Regression: L2 regularization, tuned via Grid Search with TimeSeriesSplit cross-validation
3. Lasso Regression: L1 regularization with built-in feature selection, tuned via Grid Search

### Evaluation Metrics
- **RMSE (Root Mean Squared Error):** Primary metric, expressed in % return units — directly interpretable
- **MAE (Mean Absolute Error):** Average absolute prediction error in % return
- **R²:** Proportion of variance in next-day returns explained by the model

---

## Key Findings

**1. Return prediction is genuinely hard**  
All three models achieve RMSE of ~2.04% on the test set and R² close to zero consistent with the efficient market hypothesis that short-term price movements are largely unpredictable from price history alone.

**2. Regularization provides marginal improvement**  
Ridge and Lasso slightly outperform plain Linear Regression, confirming that regularization helps with correlated stock features even when the overall signal is weak.

**3. Sector predictability varies meaningfully**  
Financial Services (RMSE 1.67%) and Consumer Defensive (RMSE 1.74%) are the most predictable sectors. Technology (RMSE 2.74%) is the least predictable — directly supporting the core research question.

**4. Short-term momentum is the strongest signal**  
Lagged returns dominate feature importance, consistent with well-documented momentum effects in financial markets.

### Business Implications
- Sectors with lower RMSE offer more reliable return signals for model-driven strategies
- Technology stocks require wider risk buffers due to higher return volatility
- Momentum-based features (lagged returns) are more useful than volume-based features for next-day prediction

---

## Next Steps and Recommendations

1. Add sector as a categorical feature using one-hot encoding
2. Build sector-specific models to test whether specialized training improves predictions
3. Incorporate macroeconomic indicators such as VIX and interest rates
4. Explore tree-based models (Random Forest, Gradient Boosting) for nonlinear patterns
5. Frame as a classification problem — predict up/down direction rather than exact return magnitude

---

## Project Files

- [`capstone_final_aparnamohan.ipynb`](capstone_final.ipynb) — Main Jupyter Notebook with full analysis
- [`README.md`](README.md) — This file
- `stock_prices_daily.csv` — Dataset (download from Kaggle link above)

## How to Run

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook capstone_final.ipynb
```

Place `stock_prices_daily.csv` in the same folder as the notebook before running.

---

## Contact

**Email:** iaparnamohan@gmail.com
