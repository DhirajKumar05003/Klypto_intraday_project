# Klypto_intraday_project

# Regime-Aware Intraday Trading Strategy with Machine Learning Enhancement

# 1. Project Overview

This project implements a regime-aware intraday trading framework for the NIFTY 50 index using a layered approach:

Data cleaning and alignment of intraday market data

Feature engineering on price, trend, volatility, and time dimensions

Market regime detection using a Hidden Markov Model (HMM)

A baseline EMA crossover strategy filtered by regimes

Machine learning–based trade filtering (XGBoost, LSTM)

High-performance (outlier) trade analysis to extract structural insights

The objective is not to maximize raw returns, but to build a robust, interpretable, and reproducible trading research pipeline.

# 2. Project Structure
├── 01_data_loading.ipynb
├── 02_data_cleaning.ipynb
├── 03_feature_engineering.ipynb
├── 04_regime_detection.ipynb
├── 05_strategy_logic.ipynb
├── 06_ml_enhancement.ipynb
├── 07_Outlier_analysis.ipynb
├── requirements.txt
└── README.txt

# 3. Data Description

Instrument: NIFTY 50 Index

Frequency: Intraday (resampled to 5-minute candles)

Time Period: ~1 year

Primary Data:

Spot price (Open, High, Low, Close, Volume)

Derived Data:

Synthetic futures (constructed from spot prices)

ATM strike references and option-related proxies where applicable

Important Note on Data Integrity

Where derivative-specific data (e.g., intraday options Greeks, implied volatility) was unavailable, no synthetic substitutes were introduced to avoid analytical bias.

# 4. Data Pipeline Summary

Key preprocessing steps:

Timestamp standardization

Market hours filtering (09:15–15:30)

Uniform 5-minute resampling

NaN and alignment validation across datasets

The pipeline ensures no look-ahead bias and consistent indexing across all stages.

# 5. Feature Engineering

Features engineered include:

Log returns

EMA (5, 15)

EMA spread and distance

Rolling volatility measures

Lagged price and EMA features

Time-based features (minute of day, day of week)

All features are:

Observable

Interpretable

Deterministically reproducible

# 6. Regime Detection

Model: Hidden Markov Model (HMM)

Number of regimes: 3

+1 → Uptrend

0 → Sideways

−1 → Downtrend

Inputs: Returns, volatility, EMA dynamics, synthetic futures basis

Training: First 70% of data (chronological split)

Regimes are validated using:

Price overlays

Transition matrices

Regime duration analysis

# 7. Trading Strategy
Baseline Strategy

EMA (5/15) crossover

Regime filter applied:

Long only in uptrend

Short only in downtrend

No trades in sideways regime

Entry and exit at next candle open

Backtesting

Chronological train/test split (70% / 30%)

Metrics evaluated:

Total return

Sharpe ratio

Max drawdown

Win rate

# 8. Machine Learning Enhancement
Problem Definition

Binary classification:

Target = 1 if trade is profitable

Target = 0 otherwise

Models Used

XGBoost

Time-series cross-validation

Feature importance analysis

LSTM

Sequence input of last 10 candles

Architecture: LSTM → Dropout → Dense

ML-Enhanced Strategy

Trades are executed only if:

ML model predicts profitability with confidence > 0.5

Performance is compared against the baseline strategy.

# 9. High-Performance Trade Analysis

Profitable trades analyzed using Z-score

Outliers defined as trades with Z-score > 3

Approximately ~2% of trades identified as outliers

Key Findings

Outlier trades are:

Short duration

High volatility

Strongly regime-aligned

Long-duration trades rarely produce extreme returns

This analysis highlights that a small subset of trades drives a disproportionate share of profits.

# 10. Assumptions and Limitations

No transaction costs or slippage modeled

No leverage applied

Options Greeks and IV not included due to data unavailability

Synthetic futures derived only from spot prices (no additional information introduced)

All assumptions are made explicitly to preserve transparency and reproducibility.

# 11. Reproducibility

All results are derived from observable market data

Modeling steps are fully deterministic and notebook-driven

No manual labeling or future data leakage

To run the project:

pip install -r requirements.txt

12. Conclusion

This project demonstrates that:

Regime awareness significantly improves intraday strategy robustness

Machine learning is most effective as a trade filter, not a signal generator

Understanding high-performance trades provides actionable insights beyond aggregate metrics

The framework is extensible and suitable for further research and production-grade enhancement.
