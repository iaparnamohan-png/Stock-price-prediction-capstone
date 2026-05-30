# Capstone Project: Stock Price Movement Prediction Across Sectors

## Overview
This project uses historical daily stock price data to predict next-day closing prices across US publicly traded companies, and examines whether prediction accuracy varies meaningfully by sector.

## Research Question
Can historical daily OHLCV stock price data predict price movement across US publicly traded companies, and do these predictions vary by sector?

## Dataset
- **Source:** US Stock Market Historical OHLCV Dataset (Kaggle)
- **Link:** https://www.kaggle.com/datasets/asadullahcreative/us-stock-market-historical-ohlcv-dataset
- **Size:** 184,138 daily records
- **Coverage:** 120 tickers across 9 sectors, January 2020 — February 2026
- **Features:** Date, Ticker, Company Name, Sector, Industry, Open, High, Low, Close, Adj_Close, Volume

## Sectors Covered
Technology, Healthcare, Financial Services, Consumer Cyclical, Basic Materials, Industrials, Consumer Defensive, Communication Services, Energy

## Techniques Used
- Exploratory Data Analysis (EDA)
- Feature Engineering (daily returns, moving averages, price range, log volume)
- Outlier Analysis (IQR method)
- Multiple Linear Regression (baseline model)

## Engineered Features
| Feature | Description |
|---|---|
| Daily_Return | Day-over-day % change in close price |
| Price_Range | High minus Low — proxy for intraday volatility |
| MA5 | 5-day moving average of close price |
| MA20 | 20-day moving average of close price |
| MA_Signal | 1 if MA5 > MA20 (bullish signal), else 0 |
| Log_Volume | Log-transformed volume to reduce skew |

## Results

### EDA Key Findings
- No missing values — clean dataset ready for modeling
- Close price is right-skewed, driven by high-price outliers in Technology and Healthcare sectors
- Technology and Consumer Cyclical show the highest daily return volatility
- Energy and Consumer Defensive are the most stable sectors
- MA5 and MA20 are the strongest predictors of next-day close price
- Volume has relatively low correlation with next-day price direction

### Baseline Model Performance
**Model:** Multiple Linear Regression
**Target:** Next-day closing price
**Overall RMSE:** 4.4531
**Overall R²:** 0.9992

**Evaluation metric rationale:** RMSE measures average prediction error in dollar terms — easy to interpret for non-technical stakeholders. R² captures how much of the variance in next-day close price is explained by our features. An R² of 0.9992 means the model explains 99.92% of the variance in next-day closing prices.

### Model Performance by Sector

| Sector | RMSE ($) | R² |
|---|---|---|
| Energy | 1.7887 | 0.9984 |
| Basic Materials | 3.1026 | 0.9993 |
| Communication Services | 3.6608 | 0.9993 |
| Consumer Defensive | 3.8015 | 0.9995 |
| Industrials | 3.9560 | 0.9989 |
| Consumer Cyclical | 4.2472 | 0.9978 |
| Financial Services | 4.3812 | 0.9995 |
| Technology | 5.2030 | 0.9986 |
| Healthcare | 6.1490 | 0.9990 |

### Key Takeaway
Model performance varies meaningfully by sector. Energy is the most predictable sector (RMSE $1.79) while Healthcare is the hardest to predict (RMSE $6.15) — directly supporting the core research question. All sectors achieve R² above 0.997, confirming that historical OHLCV features and moving averages are strong predictors of next-day closing prices across the board.


``
