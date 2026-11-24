# DCF Valuation of Apple Inc. (AAPL)

A discounted cash flow (DCF) model of Apple built in Python, using financial data from Yahoo Finance,
with an own beta regression and a sensitivity analysis. Data as of November 2026.

## Key results

| | |
|---|---|
| Base-case value per share | **USD 131** |
| Market price | USD 343 |
| WACC | ≈ 10.2% (own beta 1.085) |
| Terminal growth | 3.0% |
| Terminal value share of EV | 72% |
| Value range (WACC 6–12%, g 1–4%) | USD 92 – 453 |
| Implied terminal growth (reverse DCF) | ≈ 8.0% |
| Implied WACC at g = 3% (reverse DCF) | ≈ 5.7% |

![Sensitivity heatmap](sensitivity.png)

The market price is only reached with a WACC near 6% and terminal growth of 3–3.5%. Given the current
~5% risk-free rate, this implies a very low equity risk premium; more plausibly, the market expects
substantially higher growth than this forecast assumes.

## Method

1. **Data:** income statement, cash flow statement and balance sheet via `yfinance`
2. **Free cash flow to the firm:** FCFF = EBIT × (1 − t) + D&A − CapEx − ΔNWC
3. **5-year forecast:** revenue-driven, with all items as % of revenue
4. **Beta and WACC:** own regression of 5 years of monthly returns against the S&P 500, CAPM, WACC
5. **Terminal value:** Gordon growth model
6. **Value per share:** enterprise value → equity value (− debt + cash + investments) → per share
7. **Sensitivity analysis:** value per share across WACC and terminal growth, shown as a heatmap
8. **Reverse DCF:** solves for the terminal growth rate and WACC implied by the market price (`brentq` root-finding)

## Key assumptions

| Assumption | Value | Reasoning |
|---|---|---|
| Revenue growth | 8% → 7% → 5% → 4% → 3% | FY2026 growth of 15% treated as a one-off; fades to long-run rate |
| EBIT margin | 32.5% | Between 4-year average (31.6%) and latest year (33%) |
| NWC change | median of history | Reduces the impact of extreme years (conservative) |
| D&A, CapEx | historical averages | Both stable over time |
| Equity risk premium | 5% | Standard assumption |
| Terminal growth | 3% | Consistent with year-5 growth; below long-run nominal GDP growth |

## Findings

- **FCF is more volatile than net income,** mainly because of working capital timing.
- **Own beta of 1.085** (SE 0.153, 95% CI 0.78–1.39) is not significantly different from 1;
  this uncertainty alone moves the cost of equity between ~8.9% and ~11.9%.
- **The valuation is most sensitive to the WACC,** and the effect of terminal growth increases sharply
  as the WACC falls, because the terminal value depends on the gap between them.
- **The market price implies** ~8% growth forever or a ~5.7% WACC; neither is plausible alone, suggesting the market
  expects strong growth for longer than a 5-year forecast captures.

## Limitations and next steps

- Yahoo Finance data is unofficial and may contain gaps; results change as new data is downloaded.
- Single-scenario forecast; planned extensions: **reverse DCF** (growth implied by the market price)
  and **Monte Carlo simulation** of the key assumptions.
- Single-scenario forecast; planned extensions: a multi-stage growth model and a **Monte Carlo simulation** of the key assumptions.

## How to run

```
pip install -r requirements.txt
jupyter lab apple_dcf.ipynb
```

## Author

Matej Balogh — BBE student at WU Vienna, interested in quantitative finance.