# Trader Performance vs Market Sentiment

This is a data analysis project that explores how emotions in the crypto market which is measured by the Fear & Greed Index, affect real trader decisions and profits on Hyperliquid. By comparing trading behavior and outcomes across different sentiment regimes, the goal is to find repeatable patterns that could inform smarter trading strategies.

---

## About the Project

Crypto markets are heavily driven by emotions. When traders feel greedy they take bigger risks, and when they feel fearful they panic-sell. This project analyzes real trading data from **Hyperliquid** (a decentralized perpetual futures exchange) alongside the daily **Fear & Greed Index** to find out exactly how these emotions affect trading decisions and outcomes.

The goal is to discover repeatable patterns like whether traders consistently lose more money during peak greed so that emotion-aware trading strategies can be build.

## Data Sources

| Dataset | Description |
|---------|-------------|
| `fear_greed_index.csv` | Daily crypto Fear & Greed Index with date and sentiment classification (Fear, Greed, Neutral, etc.) |
| `historical_data.csv` | Hyperliquid trade-level data including Account, Timestamp, Closed PnL, Size USD, Execution Price, and Side (Buy/Sell) |

## Tools & Tech Stack

- **SQL (DuckDB)** - Primary tool for data loading, cleaning, aggregation, and analysis
- **Python** - Core environment for running queries, data manipulation, and generating visualizations
- **Libraries** - duckdb, pandas, matplotlib

## Methodology

### 1.Data Loading & Inspection
Both datasets are loaded into DuckDB tables. We then check the column names, data types, and preview a few rows to understand what each record looks like and what the data contains.

### 2.Data Preparation
- Removed rows with missing critical fields (Account, Timestamp, Closed PnL)
- Deduplicated exact-match rows
- Converted Unix timestamps to calendar dates (handling both seconds and milliseconds formats)
- Built a `daily_trader_metrics` view aggregating trades into one row per trader per day with key metrics: daily PnL, trade win rate, average trade size, and long/short ratio

### 3.Sentiment Merge
Joined daily trader metrics with the Fear & Greed Index on calendar date, creating a unified `trader_day_with_sentiment` view for all downstream analysis.

### 4.Analysis & Segmentation
- **Performance by sentiment**- Compared average daily PnL, win rates, trade frequency, trade size, and long ratio across Fear, Greed, Neutral, and Extreme categories
- **PnL distribution** - Examined percentile breakdowns (P10, P25, P50, P75, P90) and standard deviations per sentiment regime
- **Trader segmentation** - Classified traders into Frequent vs. Infrequent (based on median daily trade count) and Consistent Winners vs. High-Variance Winners vs. Non-Winners (based on average PnL and PnL volatility)
- **Segment × sentiment cross-analysis** - Measured how each trader segment performs differently across sentiment regimes
- **Outlier inspection** - Identified top and bottom accounts by average daily PnL to understand who drives aggregate results

## Key Visualizations

### Performance & Profitability
- **Avg Daily PnL by Sentiment** -Answers the core question: do traders make or lose more money during fear vs. greed?
- **Daily PnL Distribution (Boxplot)** -Goes beyond averages to show the full spread, median, and outliers per sentiment regime — reveals how extreme the outcomes can get.
- **PnL Volatility by Sentiment** -Compares risk across sentiment categories. Even if average PnL looks similar, higher volatility means wilder swings and more unpredictable outcomes.
- **Total Daily PnL Over Time** -A time series view of aggregate profitability, useful for spotting crash/boom clusters and how they align with sentiment shifts.

### Behavior Patterns
- **Avg Trade Win Rate by Sentiment** - Tests whether traders actually win more trades during certain moods, or if confidence and accuracy diverge.
- **Trades Count vs. Daily PnL (Scatter)** - Explores whether trading more frequently leads to better results or just more exposure to losses.

### Segment Deep-Dives
- **Avg PnL by Activity Segment & Sentiment** - Breaks down how frequent vs. infrequent traders respond to sentiment differently, revealing whether active traders are better at navigating emotional markets or worse.

## How to Run

1. Clone the repo
2. Make sure you have Python 3.8+ with `duckdb`, `pandas`, and `matplotlib` installed
3. Place the CSV data files in the root directory
4. Open and run `Untitled.ipynb` from top to bottom

```bash
pip install duckdb pandas matplotlib
jupyter notebook Untitled.ipynb
```

## Project Impact
This analysis showed that market sentiment has a measurable impact on how traders behave and perform on Hyperliquid. Traders tend to trade more frequently and take larger positions during greed phases, but that doesn't always translate to better results. Win rates, PnL volatility, and average profitability all shift noticeably across sentiment regimes - suggesting that being aware of the current market mood before placing a trade could be a simple but powerful edge.
