# **Statistical Arbitrage: Mean-Reversion Pair Trading**

This project implements a **statistical arbitrage mean-reversion strategy** using **S\&P 500 stocks**. It identifies **cointegrated pairs**, computes trading signals based on the **Z-score of the spread**, and backtests the performance with realistic assumptions including transaction costs.

---

## **Project Overview**

**Goal:**

* Build a long-short pair trading strategy using cointegration and mean-reversion.
* Backtest and evaluate performance metrics (Sharpe ratio, drawdowns, total returns).

**Key Steps:**

1. **Data Collection:**

   * S\&P 500 tickers fetched via Wikipedia.
   * Adjusted close price history downloaded using `yfinance`.
   * Missing values filled using forward/backward fill; tickers with >10% missing data removed.

2. **Pair Selection:**

   * Filtered pairs with **high correlation (>0.9)**.
   * **Engle-Granger cointegration test** applied to find stationary spreads.
   * **ADF test** confirmed stationarity of selected pairs.

3. **Signal Generation:**

   * Spread = $P_A - \beta P_B$ computed via linear regression.
   * **Z-score** of spread calculated: $Z_t = \frac{\text{Spread}_t - \mu}{\sigma}$.
   * **Trading rules:**

     * Enter **long spread** if $Z < -2$.
     * Enter **short spread** if $Z > +2$.
     * Exit positions when $|Z| < 0.5$.

4. **Backtesting:**

   * Evaluated each pair over multiple years with **daily data**.
   * NAV tracked using **log-compounding**.
   * **Transaction costs** (0.1%) applied when positions changed.
   * Metrics: Sharpe Ratio, Max Drawdown, Total Return.

---

## **Results**

**Top Pairs Ranked by Sharpe Ratio:**

```
        Pair    Sharpe Ratio    Max Drawdown    Total Return
0  AMAT-NXPI      1.90            -18.6%          +359%
1  PCAR-PTC       1.80            -32.3%          +485%
2  ECL-TDG        1.74             -8.1%          +107%
3  ECL-NVDA       1.72             -3.6%           +54%
...
```

* **Best Pair (AMAT-NXPI):**

  * Sharpe Ratio: **1.90**
  * Max Drawdown: **−18.6%**
  * Total Return: **+359%**

---

## **Key Files**

* **`pair_trading.ipynb`** – Full pipeline: data collection, cointegration tests, signal generation, backtesting.
* **`README.md`** – Project overview (this file).

---

## **How to Run**

1. Install dependencies:

   ```bash
   pip install yfinance pandas numpy statsmodels matplotlib seaborn
   ```
2. Run the Jupyter notebook:

   ```bash
   jupyter notebook pair_trading.ipynb
   ```
3. Modify parameters (e.g., correlation threshold, Z-score entry/exit levels) as needed.

---

## **Next Steps**

* Extend to **multiple pairs** trading with capital allocation.
* Add **rolling hedge ratio** estimation (dynamic β).
* Integrate **risk parity or volatility scaling** to balance positions.
* Test on **different markets** for broader validation.
