# 📊 Trader Performance vs Market Sentiment
## Primetrade.ai — Data Science Intern Assignment

---

## Overview

This project analyzes how the **Bitcoin Fear/Greed Index** relates to trader behavior and
performance on **Hyperliquid** — a decentralized perpetuals exchange.

**Key question:** Do traders perform differently, and behave differently, during Fear vs Greed markets?

---

## Datasets

| Dataset | Rows | Date Range | Key Fields |
|---|---|---|---|
| `fear_greed_index.csv` | 2,644 | Feb 2018 – May 2025 | date, value, classification |
| `historical_data.csv` | 211,224 | May 2023 – May 2025 | Account, Coin, Execution Price, Size USD, Direction, Closed PnL, Fee |

**Overlap for analysis:** 479 days · 32 unique traders · 246 unique coins

---

## Setup

```bash
# Clone the repo
git clone Rizwan-Beg/primetrade-analysis
cd primetrade-analysis

# Install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn scipy nbformat jupyter

# Place the two CSV files in the project root:
#   fear_greed_index.csv
#   historical_data.csv

# Launch the notebook
jupyter notebook primetrade_analysis.ipynb
```

---

## Project Structure

```
primetrade-analysis/
├── primetrade_analysis.ipynb   ← Main analysis notebook
├── fear_greed_index.csv        ← Bitcoin Fear/Greed dataset
├── historical_data.csv         ← Hyperliquid trader data
├── README.md                   ← This file
└── figures/                   
    ├── fig1_fg_overview.png
    ├── fig2_trader_overview.png
    ├── fig3_performance_by_sentiment.png
    ├── fig4_behavior_by_sentiment.png
    ├── fig5_freq_segments.png
    ├── fig6_style_segments.png
    ├── fig7_account_sentiment_heatmap.png
    ├── fig8_winrate_vs_pnl.png
    ├── fig9_cumulative_pnl.png
    ├── fig10_feature_importance.png
    └── fig11_clustering.png
```

---

## Methodology

### 1. Data Cleaning
- Parsed IST timestamps (`DD-MM-YYYY HH:MM`) from the trader dataset
- Normalized Fear/Greed classification to 3 categories: **Fear, Neutral, Greed**
- Excluded non-trade events (Spot Dust Conversion, Auto-Deleveraging, Settlement)
- Inner-joined both datasets on date → 479 overlapping days

### 2. Feature Engineering
- **Daily market aggregates:** total PnL, win rate, trade count, long ratio, volume
- **Per-account metrics:** total PnL, Sharpe proxy, drawdown proxy, consistency score
- **Lagged features** for predictive model (yesterday's behavior predicts today's outcome)

### 3. Analysis
- Statistical tests (t-test) to compare Fear vs Greed PnL distributions
- Behavioral comparison across sentiment regimes
- Three trader segmentation schemes: PnL-based, frequency-based, style-based
- 11 charts providing visual evidence for all insights

### 4. Bonus
- **GBM Predictive Model:** Predicts next-day market profitability using sentiment + behavioral features
- **K-Means Clustering (k=4):** Groups traders into behavioral archetypes

---

## Key Insights

### 📈 Insight 1 — Greed Days Produce Significantly Higher PnL
Average total daily PnL is markedly higher on Greed vs Fear days (statistically significant, p < 0.05).
The market collectively performs better in optimistic regimes.

### 🔄 Insight 2 — Traders Shift to Shorts on Fear Days (But Win Less)
Long ratio drops below 50% on Fear days. Despite increased short activity, win rates don't improve —
indicating reactive rather than strategic short-selling.

### 🎯 Insight 3 — Frequent Traders Are Highly Sentiment-Sensitive
High-frequency traders earn the most on Greed days but lose disproportionately on Fear days.
Infrequent/selective traders show more stable, sentiment-agnostic returns.

### 🏆 Insight 4 — Long-Biased Traders Dominate in Greed, Suffer in Fear
Style-to-regime alignment is critical. Balanced traders (50/50 L/S) are the most consistent
across all sentiment environments.

---

## Strategy Recommendations

### Strategy 1 — Sentiment-Adaptive Position Sizing
- **Greed days:** Increase position size to 1.5× baseline for long trades
- **Fear days:** Reduce to 0.5–0.7× baseline to protect capital
- *Rationale: PnL variance is highest on Fear days; preservation > aggression*

### Strategy 2 — Frequency Throttle During Fear
- Frequent traders should cap daily trades at ~50% of normal during Extreme Fear
- *Rationale: Overtrading on noise is the primary source of loss on Fear days*

### Strategy 3 — Style-Regime Matching
- Long-biased traders → full capacity only on Greed/Extreme Greed days
- Short-biased traders → activate selectively on Extreme Fear only (index < 25)
- Balanced traders → consistent across all regimes, no adjustment needed

---

## Bonus Model Results

| Metric | Value |
|---|---|
| Model | Gradient Boosting Classifier |
| Target | Next-day market profitability (binary) |
| CV ROC-AUC | ~0.65–0.72 |
| Top features | `pnl_lag1`, `fg_value`, `winrate_lag1` |

**Trader Archetypes (k=4 clustering):**
- 🟢 Aggressive Winner — High win rate, large sizes, long-biased
- 🟣 Conservative Trader — Low frequency, moderate size, balanced
- 🔴 High Volume Loser — High trade count, negative overall PnL
- 🟡 Selective Scalper — Low frequency, small sizes, positive PnL

---

*Submitted by: Rizwan Beg | Primetrade.ai Data Science Intern Application*
Note : For making this project , I have used google AI antigravity + my own knowledge + chatgpt.
