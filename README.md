# DAX 40 – Quantitative Portfolio Analysis

A quantitative research project built during my exchange semester at the University of Cologne.
The project covers the full analytical workflow from equity screening to portfolio construction
and Monte Carlo risk simulation – applied to the DAX 40 index.

## Project Structure
```
DAX-Alpha-Engine/
│
├── notebooks/
│   ├── 01_fundamental_screening.ipynb
│   ├── 02_portfolio_construction.ipynb
│   └── 03_monte_carlo_simulation.ipynb
│
├── data/
│   ├── 01_fundamental_heatmap.png
│   ├── 02_correlation_heatmap.png
│   ├── 02_efficient_frontier.png
│   ├── 02_portfolio_weights.png
│   ├── 03_monte_carlo_paths.png
│   ├── 03_return_distribution.png
│   └── 03_benchmark_comparison.png
│
└── README.md

## Methodology

### Module 1 – Fundamental Screening
Screens all 40 DAX constituents across five key metrics: P/E Ratio, P/B Ratio, ROE,
Debt/Equity and EV/EBITDA. Metrics are Z-score normalized to allow comparison across
different scales. Output is a heatmap showing each company's relative standing vs DAX peers.

Data sourced in real-time via yfinance – reflects latest available financials as of March 2026.

### Module 2 – Portfolio Construction
Selects 8 candidates from the screening output, one per sector, to ensure diversification.
Computes a correlation matrix of daily log returns (Jan 2022 – Mar 2026) and visualizes
the Efficient Frontier across 10,000 randomly simulated portfolios.

Final portfolio weights determined by **Risk Parity optimization** – each stock contributes
equally to total portfolio risk. This approach is more robust than Max Sharpe Ratio
optimization which tends to over-concentrate in recent outperformers.

Bounds: 5% minimum, 30% maximum per position.

### Module 3 – Monte Carlo Simulation
Simulates 10,000 possible portfolio return paths over one trading year (252 days) using
historical mean and standard deviation of portfolio daily returns.

Key risk metrics reported:
- **VaR 95%** – maximum loss not exceeded in 95% of scenarios
- **CVaR 95%** – average loss in the worst 5% of scenarios
- **Benchmark comparison** – portfolio return distribution vs DAX index simulation

## Key Results

| Metric | Value |
|--------|-------|
| Portfolio Sharpe Ratio | 1.82 |
| Expected Annual Return (median) | 22.6% |
| Annualized Volatility | ~18% |
| VaR 95% (1 year) | -8.0% |
| CVaR 95% (1 year) | -14.2% |
| Probability of loss | 11.7% |
| Portfolio median vs DAX median | 22.6% vs 9.9% |

---

## Selected Portfolio

| Company | Ticker | Sector | Weight |
|---------|--------|--------|--------|
| SAP | SAP.DE | Technology | 22.1% |
| Siemens | SIE.DE | Industrials | 15.3% |
| Munich Re | MUV2.DE | Insurance | 12.9% |
| Deutsche Telekom | DTE.DE | Telecom | 11.1% |
| Siemens Energy | ENR.DE | Energy | 10.9% |
| Rheinmetall | RHM.DE | Defense | 10.3% |
| Adidas | ADS.DE | Consumer | 10.2% |
| Airbus | AIR.DE | Aerospace | 7.1% |

---

## Tools & Libraries
```
Python 3.x        – core language
yfinance          – market data
pandas / numpy    – data manipulation
matplotlib        – visualization
seaborn           – heatmaps
scipy.optimize    – portfolio optimization
```

---

## Limitations & Methodology Notes

This project is intended as a demonstration of quantitative methodology,
not a live trading strategy. The following limitations apply:

**Look-ahead bias**
Screening and portfolio optimization use the same historical period (2022–2026).
In a production setting these would be separated into in-sample and out-of-sample periods
to provide a proper backtest.

**Historical period**
The 2022–2026 period captures an exceptionally strong performance window,
particularly for defense stocks (Rheinmetall +900%) and technology (SAP +200%).
The elevated Sharpe Ratio and median return figures reflect this and should not
be interpreted as forward-looking return expectations.

**Normal distribution assumption**
Monte Carlo simulation assumes normally distributed returns (Geometric Brownian Motion).
Real equity returns exhibit negative skewness and excess kurtosis – actual tail risk
may be higher than VaR estimates suggest. A more robust approach would use
multivariate simulation with Cholesky decomposition to preserve cross-asset correlations.

**Correlation stability**
Portfolio is simulated as a single asset. Cross-asset correlations are not modelled
dynamically – in market stress scenarios correlations typically spike toward 1.0,
which this model does not capture.

---

## About

Built by Aleš Ecler – Finance student at Prague University of Economics and Business (VŠE),
currently on exchange at University of Cologne (WiSo Faculty).

Motivated by wanting to apply Python skills learned between semesters to something
concrete – analysing the index I was literally studying in.
(https://www.linkedin.com/in/ales-ecler) · (https://github.com/ales-ecler)