# Statistical Arbitrage on Gold and Silver Futures

A relative-value study asking a simple question: does the long-run link between
Gold and Silver futures let you trade the spread between them profitably?

The notebook walks through the full chain — from checking that the spread actually
mean-reverts, to building a rule-based strategy on it, to testing whether that
strategy is worth the complexity versus just holding the metals.

## What the notebook does

The analysis runs top to bottom in one Jupyter notebook:

- **Data** — ~10 years of daily Gold (`GC=F`) and Silver (`SI=F`) futures closes from Yahoo Finance (2016–2026).
- **Spread & mean reversion** — builds the log-price spread, inspects it (ACF, rolling mean/std), and tests it formally with an Augmented Dickey-Fuller test.
- **Speed of reversion** — estimates the half-life of the spread from an Ornstein-Uhlenbeck-style regression, to see how fast deviations correct.
- **Hedge ratio** — regresses log-Gold on log-Silver to size the two legs of the market-neutral position.
- **Signals** — standardises the spread into a rolling z-score and defines ±1σ entry/exit rules.
- **Backtest** — computes the strategy's daily returns (position applied to the next day to avoid look-ahead in the signal) and its cumulative performance.
- **Evaluation** — total return, annualised return and volatility, Sharpe, and maximum drawdown, benchmarked against buy-and-hold Gold and Silver.

## Key findings

| Metric | Strategy | Gold (B&H) | Silver (B&H) |
|---|---|---|---|
| Total return | 9.4% | 238.9% | 312.3% |
| Annualised return | 0.9% | 11.9% | 13.8% |
| Annualised volatility | 19.0% | 16.7% | 34.8% |
| Sharpe ratio | 0.05 | 0.72 | 0.40 |
| Max drawdown | -44.9% | -25.0% | -49.6% |

The spread is statistically mean-reverting (ADF p ~ 0.04) with a strong hedge-ratio
fit (beta ~ 0.88, R^2 ~ 0.91), but the half-life is slow (~92 trading days). The simple
z-score strategy makes money over the sample, yet its risk-adjusted performance is
weak and it trails a passive long in either metal.

The takeaway is the point of the project: **statistical significance is not economic
significance.** A relationship can be real and still fail to beat a simple long once
you account for risk and the pace of reversion.

## Running it

```bash
pip install -r requirements.txt
```

Then open the notebook and run all cells. The data cells pull from Yahoo Finance,
so an internet connection is needed.

## Structure

```
.
├── Gold_Silver_Pairs_Trading.ipynb
├── requirements.txt
└── README.md
```

## Notes and next steps

The current version estimates the hedge ratio and stationarity on the full sample,
so results are in-sample. Planned improvements: estimate parameters on a training
window and trade out-of-sample, frame the spread with a consistent hedge ratio
across signals and P&L, and add transaction costs to the backtest.
