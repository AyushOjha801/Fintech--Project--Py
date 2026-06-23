#  NSE Value Investing Stock Screener

A Python-based quantitative stock screener that analyzes **all NSE-listed stocks** and ranks them by a composite **Value Score** using fundamental financial metrics — helping you identify the most undervalued stocks on the Indian market.

---

##  What It Does

- Fetches real-time data for **2000+ NSE-listed stocks** using `yfinance`
- Calculates key **value investing metrics** for each stock
- Converts raw metrics into **percentile scores** for fair cross-stock comparison
- Computes a composite **Value Score** to rank stocks
- Tells you exactly **how many shares to buy** based on your investment amount

---

##  Metrics Used

| Metric | Description |
|---|---|
| PE Ratio | Price to Earnings |
| PB Ratio | Price to Book Value |
| PS Ratio | Price to Sales |
| PEG Ratio | Price/Earnings to Growth |
| EV/EBITDA | Enterprise Value to EBITDA |
| EPS | Earnings Per Share |
| Debt to Equity | Leverage ratio |
| ROE | Return on Equity |

Each metric is converted to a **percentile score (0–1)** and averaged into a final **Value Score**.

---

##  Tech Stack

- **Python 3.x**
- **yfinance** — stock data fetching
- **pandas** — data manipulation
- **numpy** — numerical operations
- **scipy** — percentile calculations
- **concurrent.futures** — parallel fetching for speed

---

## ⚙️ How It Works

```
1. Load all NSE tickers
2. Fetch OHLCV + fundamental data in parallel (10 threads)
3. Clean data — remove negatives, inf, NaN values
4. Calculate percentile rank for each metric
5. Average percentiles → Value Score
6. Sort by Value Score → top undervalued stocks
7. Input your budget → get exact shares to buy per stock
```

---

##  Installation

```bash
git clone https://github.com/yourusername/nse-value-screener.git
cd nse-value-screener
pip install -r requirements.txt
```

**requirements.txt**
```
yfinance
pandas
numpy
scipy
```

---

##  Usage

```python
# 1. Run the screener
df = fetch_values_of_stocks(tickers_list)

# 2. Clean and score
df = calculate_value_scores(df)

# 3. Get top N stocks
df = df.head(10).reset_index(drop=True)

# 4. Input your budget
portfolio_size = int(input("Enter the Amount you want to Invest: "))
position_size = portfolio_size / len(df)
df["Number Of Shares to buy"] = df["Price"].apply(lambda p: math.floor(position_size / p))
```

---

##  Project Structure

```
nse-value-screener/
│
├── tickers/
│   └── all_nse_stocks.csv       # All NSE tickers with company names
│
├── screener.py                  # Main screener logic
├── fetch.py                     # Parallel data fetching
├── score.py                     # Percentile scoring logic
├── requirements.txt
└── README.md
```

---

##  Key Design Decisions

- **Parallel fetching** with `ThreadPoolExecutor(max_workers=10)` — reduces fetch time from ~80 mins to ~8 mins
- **Negative values filtered out** — loss-making companies excluded from value scoring
- **`thresh=6` NaN filter** — stocks with fewer than 6 valid metrics are dropped to avoid inflated scores
- **Percentile-based scoring** — ensures fair comparison across sectors regardless of absolute metric values

---

##  Disclaimer

This project is for **educational purposes only**. It is not financial advice. Always do your own research before investing. Past performance does not guarantee future results.

---

##  Contributing

Pull requests are welcome! If you'd like to add momentum metrics, dividend yield scoring, or a Streamlit dashboard — feel free to open an issue.

---

## 📄 License

MIT License — free to use and modify.
