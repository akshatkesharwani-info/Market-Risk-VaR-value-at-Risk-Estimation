# Market Risk VaR (Value at Risk) Estimation

**ML-2 | ML Engineer Track |

100% free and open-source tools only — no paid APIs, no credit card, runs on Google Colab's free tier.

---

## 📌 Overview

Risk desks need a daily estimate of maximum expected portfolio loss at a given confidence level. Build a VaR engine that computes Historical, Parametric, and Monte Carlo VaR for a multi-asset portfolio -- using only free market data and open-source numerical libraries.

**Domain:** Market Risk Management

---

## 📊 Dataset

Free daily price history for any portfolio of tickers via yfinance

**Source:** [https://finance.yahoo.com](https://finance.yahoo.com)

> This script also includes a **safe fallback**: if the real dataset file isn't found next to the
> notebook/script, it automatically generates a small realistic sample dataset with the same column
> names, so the whole pipeline still runs end-to-end even before you've downloaded the real data.

---

## 🛠️ Tech Stack

Python 3 | yfinance | NumPy | SciPy | pandas

**Skills demonstrated:** Python, NumPy, Monte Carlo simulation, yfinance, pandas

---

## 🎯 What This Project Builds

- A free price history loader for a multi-ticker portfolio via yfinance
- Daily return calculation and a portfolio-weighted return series
- Historical VaR (empirical percentile method)
- Parametric VaR (variance-covariance / normal distribution method)
- Monte Carlo VaR (simulated return paths using the portfolio's covariance matrix)
- A comparison chart of all three VaR estimates at 95% and 99% confidence

---

## 🧭 Step-by-Step Approach

### Step 1: Load Free Price Data

**What:** Pull daily adjusted close prices for the portfolio tickers via yfinance

**Why:** yfinance gives free daily data adequate for VaR estimation without a paid market data feed

**How:** yfinance.download(tickers, period='2y')['Close']


### Step 2: Compute Portfolio Returns

**What:** Calculate daily log returns and weight them by portfolio allocation

**Why:** VaR is calculated on the portfolio's combined return series, not each asset separately

**How:** returns = np.log(prices/prices.shift(1)); portfolio_returns = (returns * weights).sum(axis=1)


### Step 3: Historical & Parametric VaR

**What:** Compute the empirical percentile VaR and the normal-distribution VaR

**Why:** Comparing both methods highlights how much fat-tail risk the normal assumption misses

**How:** np.percentile(returns, 5) for historical; mean - z*std for parametric


### Step 4: Monte Carlo VaR

**What:** Simulate thousands of correlated return paths using the covariance matrix

**Why:** Monte Carlo captures correlation structure and can model more complex distributions than parametric VaR

**How:** np.random.multivariate_normal(mean_returns, cov_matrix, n_simulations)


---

## 📈 Dashboard / Reporting Ideas

- KPI cards: 95% and 99% VaR side by side for each method (Historical/Parametric/Monte Carlo)
- Histogram: Monte Carlo simulated P&L; distribution with VaR lines marked
- Line chart: rolling 30-day VaR over the trailing year to track risk trend
- Table: per-asset contribution to total portfolio variance
- Alert card: flag if today's actual loss exceeds the 95% VaR estimate (backtesting exception)

---

## 💡 Key Insights

- Parametric VaR consistently understates tail risk versus Historical/Monte Carlo when returns aren't normally distributed
- Monte Carlo VaR properly captures cross-asset correlation, which simple historical VaR on a blended series can miss
- VaR should always be backtested: count how often actual losses exceed the VaR threshold and compare to the confidence level
- yfinance is sufficient for portfolio-level VaR estimation at the retail/educational scale without any paid data feed
- This is the same three-method VaR comparison risk desks use to sanity-check any single model's assumptions

---

## 🚀 How to Run

1. Open the `.py` file in **Google Colab** (free tier — no GPU or paid compute needed) or run it locally with Python 3.
2. Install dependencies with the `pip install ...` line at the top of the script (all free, open-source packages).
3. (Optional) Download the real dataset from the Kaggle link above and place it in the same folder — the filename the script expects is noted in the code's data-loading step. If you skip this, the script auto-generates sample data so you can still see it run.
4. Run the script top to bottom. Outputs (charts, CSVs, model files) are saved in the working directory.

```bash
pip install -r requirements.txt   # or the pip install line at the top of the script
python MLEng_02_Market_Risk_VaR_Estimation.py
```

---

## 📂 Repo Structure

```
market-risk-var-value-at-risk/
├── MLEng_02_Market_Risk_VaR_Estimation.py       # complete, runnable, free-only solution code
├── README.md              # this file
└── outputs/                # charts, CSVs, and model files generated on run
```

---

## ⚠️ Disclaimer

This project is built for educational and portfolio purposes to demonstrate applied ML/quant-risk
skills. It is not financial, credit, or investment advice, and should not be used for real lending,
trading, or compliance decisions without proper review by a licensed professional.

---

*Part of a 20-project AI Engineer + ML Engineer portfolio focused on finance and consulting use cases —
built entirely with free, open-source tools.*
