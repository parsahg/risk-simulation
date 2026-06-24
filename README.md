# 📊 Quantitative Risk Simulation & Model Validation

![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-production--ready-brightgreen)

A professional‑grade, end‑to‑end market‑risk analysis pipeline built from scratch in Python.  
The project models a 5‑asset equity portfolio, computes advanced risk metrics, backtests Value‑at‑Risk models with formal statistical tests, and stress‑tests the portfolio under historical and hypothetical crisis scenarios.  
All findings are automatically distilled into a reproducible Markdown risk report – exactly the kind of deliverable expected from a **senior data scientist** in a financial institution.

---

## 🧠 Overview

This project demonstrates the ability to:

- Design and implement a **full risk measurement framework** without relying on black‑box libraries
- Move beyond basic VaR into **Cornish‑Fisher adjustments**, **GARCH volatility forecasting**, and **multi‑scenario stress testing**
- **Statistically validate** risk models using the Kupiec test, not just report violation counts
- Quantify **diversification benefits** and **marginal risk contributions** in a portfolio context
- Communicate complex quantitative results through clean visualisations and an **auto‑generated report**

The entire workflow is modular, tested, and fully reproducible – reflecting production‑grade code practices.

---

## 📌 Key Features

| Phase | Description |
|-------|-------------|
| **0 – Data Ingestion** | Downloads 5 years of daily adjusted prices from Yahoo Finance (`AAPL`, `JPM`, `XOM`, `JNJ`, `WMT` + `SPY`). Computes daily returns and cumulative performance. |
| **1 – Univariate Risk** | Annualised volatility, skewness, excess kurtosis, historical VaR/CVaR, normal VaR, Cornish‑Fisher VaR/CVaR, maximum drawdown. Histograms with normal overlays. |
| **2 – GARCH Volatility** | Fits a GARCH(1,1) model to AAPL returns. Extracts conditional volatility, compares with rolling 21‑day vol, forecasts 5‑day ahead volatility. |
| **3 – VaR Backtesting** | Rolling 500‑day out‑of‑sample backtest for both historical and normal VaR. Counts violations and performs the **Kupiec (1995) unconditional coverage test**. |
| **4 – Portfolio Risk** | Equal‑weighted portfolio construction. Correlation matrix, portfolio volatility, diversification ratio, portfolio VaR/CVaR, marginal and component risk contributions. |
| **5 – Stress Testing** | Three scenarios: (1) 2008‑style 40% crash, (2) COVID‑style vol doubling + correlation spike to 0.9, (3) custom stagflation scenario. Compares stressed VaR/CVaR. |
| **6 – Automated Report** | Generates a complete Markdown risk report (`phase6_risk_report.md`) pulling together all metrics, tables, and insights. |

---

## 📁 Project Structure

├── data/ # (Optional) CSVs if offline
├── phase0_data.py # Download prices and compute returns
├── phase1_univariate.py # Univariate risk metrics & histograms
├── phase2_garch.py # GARCH(1,1) volatility modeling
├── phase3_backtest.py # VaR backtesting & Kupiec test
├── phase4_portfolio.py # Portfolio construction & risk decomposition
├── phase5_stress.py # Multi‑scenario stress testing
├── phase6_report.py # Auto‑generated Markdown report
├── phase6_risk_report.md # Final risk report (output)
├── requirements.txt # Python dependencies
└── README.md # This file

All scripts are self‑contained or can be run sequentially. Each phase produces plots (PNG) and prints key metrics to the console.

---

## 📊 Data

- **Source**: Yahoo Finance via `yfinance`
- **Assets**: `AAPL`, `JPM`, `XOM`, `JNJ`, `WMT` (benchmark: `SPY`)
- **Period**: 2019‑01‑01 → 2024‑12‑31 (covers COVID, recovery, rate‑hike cycle)
- **Frequency**: Daily adjusted close prices

> **Offline fallback**: If `yfinance` is unavailable, a pre‑downloaded CSV with the exact same data can be used (see `/data` folder).

---

## 🧪 Methodology & Models

### Risk Measures
- **Value‑at‑Risk (VaR)**: 95% confidence, historical (non‑parametric) and parametric (normal)
- **Expected Shortfall (CVaR)**: Average loss beyond VaR
- **Cornish‑Fisher VaR/CVaR**: Adjusts the normal quantile for skewness and kurtosis, providing a more realistic tail estimate

### Volatility Modelling
- **GARCH(1,1)**: `σ²_t = ω + α·ε²_{t-1} + β·σ²_{t-1}`
- Captures volatility clustering and provides forward‑looking forecasts
- Persistence measured by α+β, long‑run volatility derived analytically

### Backtesting
- **Rolling window** (500 days) to avoid look‑ahead bias
- **Kupiec test**: Likelihood‑ratio test for correct unconditional coverage
- Null hypothesis: violation rate = 5% (for 95% VaR)

### Stress Testing
- **Historical scenario**: Uniform 40% price shock
- **Synthetic scenario**: Correlations forced to 0.9, volatilities doubled
- **Custom scenario**: Stagflation (‑15% shock + 50% vol increase)

### Portfolio Analytics
- Equal‑weighted allocation
- Diversification ratio = (weighted avg individual vol) / portfolio vol
- Marginal & component risk contributions from covariance decomposition

---

## 🔍 Key Findings

1. **Real returns are non‑normal** – all assets exhibit negative skew and high excess kurtosis.
2. **Normal VaR is rejected** – Kupiec test p‑value = 0.029 (<0.05), confirming the normal assumption fails for AAPL.
3. **Historical VaR is reliable** – violation rate 4.07%, Kupiec p‑value = 0.163 (cannot reject).
4. **GARCH captures volatility dynamics** – α+β ≈ 0.96, indicating high persistence; forecasts converge to long‑run mean of ~29% annualised.
5. **Diversification works, but fails in crises** – portfolio vol is significantly lower than average individual vol, but under stress correlations spike and the benefit vanishes.
6. **Stress tests reveal tail vulnerability** – a 40% crash scenario pushes daily VaR to –41.7%, demonstrating that a long‑only equity portfolio has no downside protection in a systemic meltdown.

These conclusions are fully documented in the [auto‑generated report](phase6_risk_report.md).

---

## 🚀 Getting Started

### Prerequisites
- Python 3.10 or higher
- `pip` for package installation

### Installation
```bash
git clone https://github.com/yourusername/portfolio-risk-simulation.git
cd portfolio-risk-simulation
pip install -r requirements.txt

All scripts are self‑contained or can be run sequentially. Each phase produces plots (PNG) and prints key metrics to the console.

---
```
## 📊 Data

- **Source**: Yahoo Finance via `yfinance`
- **Assets**: `AAPL`, `JPM`, `XOM`, `JNJ`, `WMT` (benchmark: `SPY`)
- **Period**: 2019‑01‑01 → 2024‑12‑31 (covers COVID, recovery, rate‑hike cycle)
- **Frequency**: Daily adjusted close prices

> **Offline fallback**: If `yfinance` is unavailable, a pre‑downloaded CSV with the exact same data can be used (see `/data` folder).

---

## 🧪 Methodology & Models

### Risk Measures
- **Value‑at‑Risk (VaR)**: 95% confidence, historical (non‑parametric) and parametric (normal)
- **Expected Shortfall (CVaR)**: Average loss beyond VaR
- **Cornish‑Fisher VaR/CVaR**: Adjusts the normal quantile for skewness and kurtosis, providing a more realistic tail estimate

### Volatility Modelling
- **GARCH(1,1)**: `σ²_t = ω + α·ε²_{t-1} + β·σ²_{t-1}`
- Captures volatility clustering and provides forward‑looking forecasts
- Persistence measured by α+β, long‑run volatility derived analytically

### Backtesting
- **Rolling window** (500 days) to avoid look‑ahead bias
- **Kupiec test**: Likelihood‑ratio test for correct unconditional coverage
- Null hypothesis: violation rate = 5% (for 95% VaR)

### Stress Testing
- **Historical scenario**: Uniform 40% price shock
- **Synthetic scenario**: Correlations forced to 0.9, volatilities doubled
- **Custom scenario**: Stagflation (‑15% shock + 50% vol increase)

### Portfolio Analytics
- Equal‑weighted allocation
- Diversification ratio = (weighted avg individual vol) / portfolio vol
- Marginal & component risk contributions from covariance decomposition

---

## 🔍 Key Findings

1. **Real returns are non‑normal** – all assets exhibit negative skew and high excess kurtosis.
2. **Normal VaR is rejected** – Kupiec test p‑value = 0.029 (<0.05), confirming the normal assumption fails for AAPL.
3. **Historical VaR is reliable** – violation rate 4.07%, Kupiec p‑value = 0.163 (cannot reject).
4. **GARCH captures volatility dynamics** – α+β ≈ 0.96, indicating high persistence; forecasts converge to long‑run mean of ~29% annualised.
5. **Diversification works, but fails in crises** – portfolio vol is significantly lower than average individual vol, but under stress correlations spike and the benefit vanishes.
6. **Stress tests reveal tail vulnerability** – a 40% crash scenario pushes daily VaR to –41.7%, demonstrating that a long‑only equity portfolio has no downside protection in a systemic meltdown.

These conclusions are fully documented in the [auto‑generated report](phase6_risk_report.md).

---

## 🚀 Getting Started

### Prerequisites
- Python 3.10 or higher
- `pip` for package installation

### Installation
```bash
git clone https://github.com/yourusername/portfolio-risk-simulation.git
cd portfolio-risk-simulation
pip install -r requirements.txt
```
Run the Analysis

Execute the phases in order (or run any script independently):
The final report will be saved as phase6_risk_report.md. All diagnostic plots are saved as PNG files in the project root.
The dependencies are in requirements.txt
📈 Results & Visuals

The project generates the following plots:

    Cumulative returns of all stocks vs SPY

    Histograms of daily returns with normal fit and VaR lines

    Rolling volatility and GARCH conditional volatility comparison

    VaR backtest violation plots

    Correlation matrix heatmap

    Portfolio vs individual cumulative returns

All are automatically saved and can be included in presentations or dashboards.
