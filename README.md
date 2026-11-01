# Footwear Sector Stock Analysis: Price, Return and Risk (Oct 2025 to Sep 2026)

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![pandas](https://img.shields.io/badge/pandas-data%20analysis-150458)
![yfinance](https://img.shields.io/badge/data-yfinance-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

A one-year analysis of five Indian footwear stocks. The project downloads daily prices, calculates daily and cumulative returns, and compares the stocks on volatility, correlation, risk-adjusted return, maximum drawdown and performance against the Nifty 50.

This project was completed as **Assignment 3** (Stock Price, Return and Cumulative Return Analysis).

---

## Table of Contents

1. [Stocks and Period](#stocks-and-period)
2. [Results at a Glance](#results-at-a-glance)
3. [Key Findings](#key-findings)
4. [Methodology](#methodology)
5. [Project Structure](#project-structure)
6. [Getting Started (Step by Step)](#getting-started-step-by-step)
7. [How to Read the Notebook](#how-to-read-the-notebook)
8. [Limitations and Next Steps](#limitations-and-next-steps)
9. [Author](#author)

---

## Stocks and Period

| Company | Yahoo Finance ticker |
| --- | --- |
| Bata India | `BATAINDIA.NS` |
| Relaxo Footwears | `RELAXO.NS` |
| Campus Activewear | `CAMPUS.NS` |
| Metro Brands | `METROBRAND.NS` |
| Liberty Shoes | `LIBERTSHOE.NS` |

- **Period:** 1 October 2025 to 30 September 2026 (one year)
- **Data source:** Yahoo Finance, downloaded with the `yfinance` library
- **Trading days:** 247 for Bata, Relaxo and Campus; 246 for Metro Brands and Liberty Shoes
- **Benchmark:** Nifty 50 (`^NSEI`)

## Results at a Glance

| Stock | Final cumulative return | Avg daily return | Volatility (std of daily return) | Best day | Worst day | Sharpe ratio* | Max drawdown |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Bata India | -45.43% | -0.23% | 1.94% | 16.42% | -5.66% | -2.09 | -50.22% |
| Relaxo Footwears | -33.91% | -0.13% | 2.78% | 19.99% | -6.97% | -0.91 | -46.61% |
| Campus Activewear | **-22.17%** | **-0.09%** | **1.66%** | 7.56% | -4.56% | -1.11 | **-26.33%** |
| Metro Brands | -30.03% | -0.13% | 1.86% | 9.34% | -4.11% | -1.34 | -30.98% |
| Liberty Shoes | -34.31% | -0.14% | 2.52% | 11.37% | -6.28% | -1.07 | -38.71% |

\*Annualised, assuming a 7% annual risk-free rate (change `rf_daily` in the notebook to use a different rate).

**Benchmark comparison:** the average cumulative return of the five stocks ended at about -33%, while the Nifty 50 ended at roughly -9% (read from the chart).

## Key Findings

- **All five stocks fell over the year.** The sector underperformed the Nifty 50 by a wide margin.
- **Campus Activewear held up best:** the smallest loss (-22.17%), the lowest volatility (1.66%) and the shallowest maximum drawdown (-26.33%).
- **Bata India was the weakest:** the largest loss (-45.43%), the lowest average daily return (-0.23%) and the worst Sharpe ratio (-2.09). Its price fell steadily from about Rs 1,134 to about Rs 620.
- **Relaxo and Liberty Shoes were the most volatile** (2.78% and 2.52% daily standard deviation), with the biggest single-day swings. Relaxo's best day was +19.99% and it recovered to near break-even in July 2026 before falling again.
- **Correlations between stocks were low to moderate**, from 0.19 (Campus and Metro Brands) to 0.43 (Bata and Campus). Stock-specific factors mattered, but the whole sector still declined.
- **Be careful with the Sharpe ratio when returns are negative.** Every ratio here is negative, and a highly volatile stock can look "less bad" because the larger risk term shrinks the ratio. Relaxo (-0.91) ranks better than Campus (-1.11) on Sharpe even though Campus lost much less. Use the cumulative return and drawdown alongside it.

## Methodology

1. **Download** daily OHLCV data for each stock with `yf.download` and store it in a dictionary of DataFrames. Combine them into one DataFrame with a `Stock` column.
2. **Plot closing prices** of all five stocks on one chart.
3. **Daily returns:** `closing.pct_change()`, which is (today / yesterday) - 1. Stored in `returns_df` and plotted separately for each stock.
4. **Cumulative returns:** `(1 + returns_df).cumprod() - 1`. Plotted individually, combined and as a 3 x 2 grid of subplots.
5. **Summary table:** final cumulative return, average daily return, volatility, best day and worst day.
6. **Extra analysis:**
   - Correlation heatmap of daily returns
   - Volatility bar chart and risk versus return scatter plot
   - Annualised Sharpe ratio and maximum drawdown
   - 20-day and 50-day moving averages (shown for Bata India)
   - Average of the five stocks versus the Nifty 50

## Project Structure

```text
.
├── Assignment3_Footwear_Stocks.ipynb   # Full analysis notebook
├── requirements.txt                    # Python dependencies
└── README.md
```

No data file is needed. The notebook downloads prices directly from Yahoo Finance when it runs.

## Getting Started (Step by Step)

### Option A: Run in Google Colab (easiest)

1. Open [Google Colab](https://colab.research.google.com/) and upload `Assignment3_Footwear_Stocks.ipynb` (File > Upload notebook).
2. Add a cell at the top and run it to install the data library:

   ```python
   !pip install yfinance
   ```

3. Run all cells (Runtime > Run all). You need an internet connection, because the data is downloaded live.

### Option B: Run locally

1. **Clone the repository**

   ```bash
   git clone https://github.com/<your-username>/<your-repo-name>.git
   cd <your-repo-name>
   ```

2. **Create and activate a virtual environment**

   ```bash
   python -m venv venv
   source venv/bin/activate        # macOS / Linux
   venv\Scripts\activate           # Windows
   ```

3. **Install dependencies.** Create a `requirements.txt` containing:

   ```text
   pandas
   numpy
   matplotlib
   seaborn
   yfinance
   jupyter
   ```

   then run:

   ```bash
   pip install -r requirements.txt
   ```

4. **Launch the notebook**

   ```bash
   jupyter notebook Assignment3_Footwear_Stocks.ipynb
   ```

5. **Run all cells** from top to bottom.

### Change the analysis

- **Different stocks:** edit the `stocks` list in the first code cell. Indian NSE tickers end in `.NS`.
- **Different period:** edit `start_date` and `end_date`. Yahoo Finance excludes the end date, so set it to the day after the last day you want.
- **Risk-free rate:** edit `rf_daily` (currently `0.07 / 252`).
- **Moving averages:** change `symbol` in the moving-average cell to plot another stock.

> Yahoo Finance data can be revised over time, so a fresh run may differ slightly from the figures above.

## How to Read the Notebook

| Section | What it covers |
| --- | --- |
| 1. Download the data | Fetching prices with yfinance, building one combined DataFrame |
| 2. Closing prices | All five stocks on one chart |
| 3. Daily returns | Percentage change stored in a DataFrame |
| 4. Daily returns plotted | One chart per stock, each in a different colour |
| 5. Cumulative returns | Compounded return stored in a DataFrame |
| 6. Cumulative return charts | Individual, combined and grid of subplots |
| 7. Key numbers | Summary table for the written conclusions |
| Extra analysis | Correlation, volatility, risk vs return, Sharpe, drawdown, moving averages, benchmark |

## Limitations and Next Steps

- Two stocks (Metro Brands and Liberty Shoes) have one fewer trading day than the others, which leaves small gaps and missing values in the return series. Aligning the dates (for example with `dropna()` or forward-filling) would make the comparison cleaner.
- One year of data is a short window, and the period covers a broad market decline, so the results may not describe a normal year.
- The Sharpe ratio uses a flat 7% risk-free rate and is hard to interpret when returns are negative.
- Next steps could include beta against the Nifty 50, rolling volatility, an equal-weighted portfolio of the five stocks, and a longer history.

This project is for learning and is not investment advice.

## Author

**Danis Shrestha**
MBA (Finance and Business Analytics), CMS Business School, Bengaluru
LinkedIn: [add your profile link]

If you found this useful, consider giving the repository a star.
