## Statistical Analysis of Bitcoin Prices and Halving Effects  
*Category:* Data science · Time series · Event study

## Problem statement
Every ~4 years Bitcoin cuts its block reward in half (“halving”). People say price and volatility change a lot around those dates, but the evidence I usually see is anecdotal or hard to reproduce. I want a small, clean study that anybody can rerun: describe the data, check basic distributional assumptions, and test whether the average price level and the variance look different before vs. after each halving. 

## Planned approach and technologies
**Data.** Daily BTC close in USD.
**What I’ll do.**
- Descriptive stats (count, quantiles, mean, variance/std).
- Normality checks: Q–Q and P–P plots on price and log-price.
- Sampling demo: t-based confidence intervals with n=4 and n=100.
- Halving study
  - Means: Welch’s t-test (unequal variances).  
  - Variances: F-test on the ratio (post/pre).
**Tech.** Python 3.11 with pandas, numpy, scipy, matplotlib (maybe statsmodels). Light tests with pytest. A simple CLI or Makefile so one command rebuilds figures (PNG), tables (CSV), and a short report (Markdown).

## Expected challenges
- **Heavy tails / outliers.** Work with log-prices , report median/IQR alongside mean/variance, show sensitivity with and without extremes.
- **Autocorrelation / heteroskedasticity.** Treat the basic tests as descriptive, not causal. Add bootstrap CIs or use returns as a robustness check.
- **Window choice.** Report results for and ±365 days and put them side by side.
- **Data quirks.** Careful date parsing, numeric coercion, and an audit log of rows dropped or fixed so nothing is out of the hat.

## Success criteria
- **Reproducible:** one command rebuilds everything from the raw Excel.
- **Correct:** a few unit tests pass (e.g., CI math, window slicing); spot checks match manual calculations.
- **Clear:** figures are labeled and assumptions and limits are written down.
- **Findings:** at least one halving window shows a statistically significant difference (α=0.05) in mean and/or variance, and the direction is consistent across the two window sizes.

## Stretch goals
- Fit a simple GARCH model to log-returns to show time-varying volatility.
- Try a basic change-point detector instead of fixed windows.
- Generalize to another asset/event (e.g., Litecoin halving) or switch the main analysis to returns.