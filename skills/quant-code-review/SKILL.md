---
name: quant-code-review
description: Reviews Python code for quant finance with focus on numerical stability, performance, vectorization, and financial modeling best practices
---

# Quant Finance Code Review

Perform comprehensive code reviews of Python code written for quantitative finance, focusing on best practices, performance, numerical accuracy, and common pitfalls.

## Purpose

This skill provides expert code review for quant finance Python code, covering:
- Numerical stability and precision issues
- Vectorization and performance optimization
- Financial calculation accuracy
- Pandas/NumPy best practices
- Risk management and edge cases
- Code structure and maintainability

## Instructions

When this skill is invoked:

1. **Analyze the Code Context**
   - Identify what the code does (pricing, backtesting, risk calculation, data processing)
   - Understand the financial domain (equities, fixed income, derivatives, etc.)
   - Note the libraries used (NumPy, Pandas, SciPy, QuantLib, etc.)
   - Check if this is production code or research/exploratory code

2. **Review Numerical Computation**
   - **Numerical Stability**:
     - Check for potential overflow/underflow issues
     - Look for divisions by zero or very small numbers
     - Verify log-space calculations for probabilities
     - Check for catastrophic cancellation (subtracting similar numbers)
     - Verify proper handling of NaN and infinity

   - **Precision Issues**:
     - Flag use of float32 where float64 is needed
     - Check date/time precision for financial timestamps
     - Verify rounding for currency calculations (use Decimal for money)
     - Check if relative vs absolute tolerances are appropriate

   - **Financial Calculations**:
     - Verify day count conventions (ACT/365, 30/360, etc.)
     - Check compounding frequency handling (annual, semi-annual, continuous)
     - Validate present value and discount factor calculations
     - Review option pricing model implementations
     - Check Greeks calculations for numerical accuracy

3. **Review Performance and Vectorization**
   - **Vectorization**:
     - Flag Python loops that should be vectorized with NumPy/Pandas
     - Suggest `apply()` alternatives with vectorized operations
     - Identify opportunities for broadcasting
     - Check for inefficient DataFrame operations (iterrows, apply on large data)

   - **Memory Efficiency**:
     - Flag unnecessary data copies
     - Check for memory-inefficient data types
     - Suggest chunking for large datasets
     - Review in-place operations vs copies

   - **Computational Efficiency**:
     - Check for redundant calculations inside loops
     - Suggest caching expensive computations
     - Review algorithm complexity (O(n²) vs O(n log n))
     - Recommend JIT compilation (Numba) for hot loops

4. **Review Pandas Usage**
   - **Best Practices**:
     - Use `.loc[]` and `.iloc[]` instead of chained indexing
     - Avoid `SettingWithCopyWarning`
     - Use categorical dtype for repeated strings
     - Leverage `groupby()` and `resample()` efficiently
     - Check for proper time zone handling

   - **Common Mistakes**:
     - Flag iterating over DataFrame rows
     - Identify repeated `append()` calls (use list + `pd.concat()`)
     - Check for unnecessary `copy()` calls
     - Verify proper handling of missing data (NaN)

5. **Review Financial Logic**
   - **Edge Cases**:
     - Zero or negative prices
     - Extreme volatility values
     - Very short or long time periods
     - Market holidays and weekends
     - Corporate actions (splits, dividends)

   - **Risk Checks**:
     - Verify position sizing constraints
     - Check for portfolio leverage limits
     - Validate stop-loss and take-profit logic
     - Review slippage and transaction cost modeling

   - **Data Quality**:
     - Check for look-ahead bias in backtests
     - Verify proper handling of delisted securities
     - Check survivorship bias prevention
     - Validate data alignment and forward-filling

6. **Review Code Structure**
   - **Organization**:
     - Suggest breaking large functions into smaller ones
     - Recommend classes for related functionality
     - Check separation of concerns (data, logic, presentation)
     - Review module structure and imports

   - **Documentation**:
     - Check docstrings for financial formulas
     - Verify parameter units (%, decimal, bps)
     - Request examples for complex functions
     - Suggest type hints for function signatures

   - **Testing Considerations**:
     - Suggest unit tests for financial calculations
     - Recommend property-based testing (Hypothesis)
     - Suggest regression tests for pricing models
     - Flag functions that need validation against market data

7. **Provide Actionable Feedback**
   - Prioritize issues: Critical (bugs, numerical errors) → High (performance) → Medium (style)
   - Provide specific code examples for suggested improvements
   - Explain the "why" behind each recommendation
   - Reference academic papers or industry standards when relevant
   - Suggest specific libraries or tools (QuantLib, Zipline, PyPortfolioOpt, etc.)

## Examples

**Example 1: Reviewing a Returns Calculation**
```python
# Original Code
returns = []
for i in range(1, len(prices)):
    ret = (prices[i] - prices[i-1]) / prices[i-1]
    returns.append(ret)
```

**Review:**
```
ISSUE: Performance - Non-vectorized calculation
SEVERITY: High
EXPLANATION: Loop-based calculation is slow for large datasets.
NumPy/Pandas vectorized operations are 10-100x faster.

SUGGESTED FIX:
returns = prices.pct_change().dropna()
# or with NumPy:
returns = np.diff(prices) / prices[:-1]

ADDITIONAL: Consider log returns for better statistical properties:
log_returns = np.log(prices / prices.shift(1)).dropna()
```

**Example 2: Reviewing Sharpe Ratio Calculation**
```python
# Original Code
def sharpe_ratio(returns, risk_free_rate=0.02):
    mean_return = np.mean(returns)
    std_return = np.std(returns)
    return (mean_return - risk_free_rate) / std_return
```

**Review:**
```
ISSUES:
1. CRITICAL: Annualization missing
2. HIGH: Risk-free rate frequency assumption unclear
3. MEDIUM: No handling of zero volatility edge case

EXPLANATION:
- Returns and risk-free rate must be on same frequency
- Sharpe ratio convention is to annualize
- Zero volatility (constant returns) causes division by zero

SUGGESTED FIX:
def sharpe_ratio(returns, risk_free_rate=0.0, periods_per_year=252):
    """
    Calculate annualized Sharpe ratio.

    Parameters
    ----------
    returns : array-like
        Periodic returns (not annualized)
    risk_free_rate : float
        Annual risk-free rate (default: 0.0)
    periods_per_year : int
        Trading periods per year (252 for daily, 12 for monthly)

    Returns
    -------
    float
        Annualized Sharpe ratio
    """
    excess_returns = returns - (risk_free_rate / periods_per_year)
    mean_excess = np.mean(excess_returns)
    std_excess = np.std(excess_returns, ddof=1)  # Use sample std

    if std_excess == 0:
        return np.nan if mean_excess == 0 else np.sign(mean_excess) * np.inf

    return (mean_excess / std_excess) * np.sqrt(periods_per_year)

REFERENCE: "The Sharpe Ratio" by William F. Sharpe (1994)
```

**Example 3: Reviewing Black-Scholes Implementation**
```python
# Original Code
def black_scholes_call(S, K, T, r, sigma):
    d1 = (np.log(S/K) + (r + sigma**2/2)*T) / (sigma*np.sqrt(T))
    d2 = d1 - sigma*np.sqrt(T)
    return S*norm.cdf(d1) - K*np.exp(-r*T)*norm.cdf(d2)
```

**Review:**
```
ISSUES:
1. CRITICAL: No handling of T=0 (expiration)
2. HIGH: Division by zero when sigma=0
3. HIGH: Numerical instability for extreme parameters
4. MEDIUM: No input validation
5. LOW: Missing docstring with parameter units

SUGGESTED IMPROVEMENTS:
from scipy.stats import norm
import numpy as np

def black_scholes_call(S, K, T, r, sigma):
    """
    Black-Scholes European call option price.

    Parameters
    ----------
    S : float
        Current stock price
    K : float
        Strike price
    T : float
        Time to expiration (in years)
    r : float
        Risk-free rate (annual, continuously compounded)
    sigma : float
        Volatility (annual, as decimal not %)

    Returns
    -------
    float
        Call option price
    """
    # Input validation
    if S <= 0 or K <= 0:
        raise ValueError("Stock and strike prices must be positive")
    if T < 0:
        raise ValueError("Time to expiration cannot be negative")
    if sigma < 0:
        raise ValueError("Volatility cannot be negative")

    # Handle edge cases
    if T == 0:
        return max(S - K, 0)  # Intrinsic value at expiration

    if sigma == 0:
        # Zero volatility: option is worthless or worth intrinsic value
        return max(S - K * np.exp(-r * T), 0)

    # Standard Black-Scholes formula
    sqrt_T = np.sqrt(T)
    d1 = (np.log(S / K) + (r + 0.5 * sigma**2) * T) / (sigma * sqrt_T)
    d2 = d1 - sigma * sqrt_T

    call_price = S * norm.cdf(d1) - K * np.exp(-r * T) * norm.cdf(d2)

    return call_price

ADDITIONAL NOTES:
- Consider adding vectorization support (S, K as arrays)
- For production, add Greeks calculations
- Consider numerical stability for deep ITM/OTM options (use put-call parity)
- For American options, this formula is incorrect (use binomial/finite difference)
```

## Guidelines

### Numerical Computing Priorities
1. **Correctness** > **Performance** > **Elegance**
2. Always validate against known test cases or market data
3. Document assumptions (day count, compounding, calendar)
4. Use appropriate precision for financial calculations

### Performance Review Framework
- Don't optimize prematurely; profile first
- Vectorization > Caching > JIT compilation > C extensions
- For research code, clarity > speed (unless it's prohibitively slow)
- For production code, both clarity and speed matter

### Financial Domain Specifics
- Distinguish between simple vs log returns
- Know when to use arithmetic vs geometric mean
- Understand discrete vs continuous time models
- Be aware of bid-ask spread and transaction costs
- Consider market microstructure (trading hours, holidays)

### Common Red Flags
- `for i in range(len(df))` - Almost always wrong
- `.append()` in a loop for DataFrames
- Comparing floats with `==`
- Not handling NaN/missing data
- Look-ahead bias in backtests
- Ignoring transaction costs and slippage
- Using daily returns without accounting for weekends

### Recommended Libraries
- **Core**: NumPy, Pandas, SciPy, Statsmodels
- **Visualization**: Matplotlib, Seaborn, Plotly
- **Performance**: Numba, Cython, Dask
- **Finance**: QuantLib, Zipline, PyPortfolioOpt, TA-Lib
- **Data**: yfinance, pandas-datareader, Alpha Vantage
- **Testing**: pytest, Hypothesis

## Output Format

Structure feedback as:

```
## Summary
[Brief overview of code quality and main findings]

## Critical Issues
[Bugs, numerical errors, financial logic errors]

## Performance Issues
[Vectorization opportunities, memory problems]

## Best Practice Improvements
[Code structure, documentation, testing]

## Positive Aspects
[What the code does well]

## Recommended Next Steps
1. [Priority 1]
2. [Priority 2]
...
```

## Notes

- Assume the user is learning, so explain concepts clearly
- Balance between academic rigor and practical implementation
- Provide references to papers, books, or documentation when relevant
- Encourage testing against known benchmarks (e.g., Bloomberg, QuantLib)
- Consider both research/exploratory context and production requirements
- Remember that quant finance code often involves statistical assumptions - review those too
