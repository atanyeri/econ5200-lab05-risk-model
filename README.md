# Diagnosing a Flawed Risk Model: VaR, Expected Shortfall & Monte Carlo

## Objective
I tested a junior analyst's normal-distribution VaR model against the data it was built on, and showed that it understates tail risk.

## Methodology
- Checked the analyst's return series with descriptive statistics and found fat tails (excess kurtosis well above 0), which the normal assumption ignores.
- Computed 99% VaR and Expected Shortfall three ways: normal, Student-t (degrees of freedom fitted by maximum likelihood), and historical simulation.
- Priced a European call by Monte Carlo, then repeated it with antithetic variates (pairing each draw Z with -Z) and compared the standard errors.
- Used a `risk_metrics.py` module with `calculate_var`, `calculate_es` and `mc_var`, and ran its self-tests.
- Had an AI write a VaR backtest function, found that my first prompt left the sign convention unstated, revised the prompt once, and checked the function's output against my own count.

## Key Findings
- At 99%, the analyst's normal VaR was 12.7% too small compared with historical VaR, a gap of $40,393 on the portfolio.
- The fitted Student-t had 4.58 degrees of freedom, which confirms heavy tails.
- Expected Shortfall exceeded VaR under every method.
- Antithetic variates cut the Monte Carlo standard error by 1.26x with no extra draws.
- In the backtest, the normal 99% VaR was breached on 1.71% of days, against the 1% it promises. The analyst's claim that risk was "well within acceptable limits" does not hold up.
- The AI's backtest function matched my hand count once the sign convention was stated explicitly in the prompt.
