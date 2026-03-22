---
name: quant-strategy-backtest
description: Creates robust backtesting frameworks for trading strategies with proper data handling, performance metrics, and bias prevention
---

# Quant Strategy Backtesting Framework

Generate production-quality backtesting code for trading strategies with proper handling of data, transaction costs, performance metrics, and common pitfalls.

## Purpose

This skill creates well-structured backtesting frameworks that:
- Prevent look-ahead bias and survivorship bias
- Handle transaction costs and slippage realistically
- Calculate comprehensive performance metrics
- Support multiple asset classes
- Enable easy strategy iteration and optimization
- Follow industry best practices

## Instructions

When this skill is invoked:

1. **Understand Strategy Requirements**
   - Ask about the trading strategy type:
     - Mean reversion, momentum, trend following, statistical arbitrage, pairs trading
   - Identify the asset class:
     - Equities, futures, FX, crypto, fixed income, options
   - Determine the trading frequency:
     - High-frequency, intraday, daily, weekly, monthly
   - Clarify data requirements:
     - OHLCV, fundamentals, alternative data, market microstructure
   - Understand constraints:
     - Capital, position limits, sector exposure, leverage

2. **Design the Backtesting Architecture**

   Create a modular structure with these components:

   **a) Data Handler**
   - Load and validate historical data
   - Handle missing data and corporate actions
   - Align multiple data sources
   - Implement proper time-based indexing
   - Support point-in-time data access (no look-ahead bias)

   **b) Strategy Class**
   - Generate signals based on historical data only
   - Maintain strategy state (positions, indicators, etc.)
   - Support parameter optimization
   - Separate signal generation from execution

   **c) Portfolio/Position Manager**
   - Track positions and cash
   - Implement position sizing logic
   - Handle portfolio constraints (max position, sector limits)
   - Calculate portfolio value over time
   - Manage margin and leverage

   **d) Execution Simulator**
   - Model order types (market, limit, stop)
   - Simulate slippage (spread, market impact)
   - Apply transaction costs (commissions, fees)
   - Handle partial fills
   - Respect market hours and liquidity

   **e) Performance Analyzer**
   - Calculate returns (absolute, excess, risk-adjusted)
   - Compute standard metrics (Sharpe, Sortino, Calmar, max drawdown)
   - Analyze trade statistics (win rate, profit factor, avg holding period)
   - Risk metrics (VaR, CVaR, volatility)
   - Benchmark comparison

   **f) Reporting/Visualization**
   - Equity curve with drawdowns
   - Returns distribution
   - Rolling metrics (Sharpe, volatility)
   - Monthly/yearly returns heatmap
   - Trade analysis plots

3. **Implement Core Backtesting Logic**

   Follow this event-driven or vectorized approach:

   **Event-Driven (More realistic, slower):**
   ```python
   for timestamp in data.index:
       # Update market data (no future data!)
       current_data = data.loc[:timestamp]

       # Generate signals
       signals = strategy.generate_signals(current_data)

       # Execute trades
       trades = portfolio.rebalance(signals, current_data.iloc[-1])

       # Apply costs
       trades_with_costs = execution.apply_costs(trades)

       # Update portfolio
       portfolio.update(trades_with_costs, timestamp)

       # Record metrics
       metrics.record(portfolio.value, timestamp)
   ```

   **Vectorized (Faster, need to avoid look-ahead bias):**
   ```python
   # Calculate indicators on full dataset
   data['signal'] = strategy.compute_signals(data)

   # Shift signals to next period (prevent look-ahead)
   data['position'] = data['signal'].shift(1)

   # Calculate returns
   data['returns'] = data['close'].pct_change()
   data['strategy_returns'] = data['position'] * data['returns']

   # Apply costs (simplified)
   data['costs'] = data['position'].diff().abs() * transaction_cost
   data['net_returns'] = data['strategy_returns'] - data['costs']
   ```

4. **Implement Bias Prevention**

   **Look-Ahead Bias:**
   - Use `.shift(1)` when converting signals to positions
   - Ensure indicators only use past data
   - Never use future data for signal generation
   - Validate with out-of-sample testing

   **Survivorship Bias:**
   - Include delisted securities in universe
   - Account for bankruptcies and delistings
   - Use point-in-time constituent data for indices

   **Optimization Bias (Overfitting):**
   - Use train/validation/test split (e.g., 60/20/20)
   - Implement walk-forward optimization
   - Use cross-validation for parameter selection
   - Report both in-sample and out-of-sample results
   - Apply penalties for complex models (fewer parameters better)

5. **Calculate Performance Metrics**

   **Returns:**
   - Total return, CAGR
   - Excess return over benchmark
   - Alpha and beta

   **Risk-Adjusted:**
   - Sharpe ratio (annualized)
   - Sortino ratio (downside risk)
   - Calmar ratio (return / max drawdown)
   - Information ratio (vs benchmark)
   - Omega ratio

   **Drawdown:**
   - Maximum drawdown
   - Average drawdown
   - Drawdown duration
   - Underwater period

   **Trade Statistics:**
   - Total trades, win rate
   - Average win/loss
   - Profit factor (gross profit / gross loss)
   - Expected value per trade
   - Average holding period

   **Risk:**
   - Volatility (annualized)
   - Value at Risk (VaR) - parametric and historical
   - Conditional VaR (CVaR/Expected Shortfall)
   - Beta to market
   - Tracking error

6. **Handle Transaction Costs Realistically**

   **Commission Models:**
   - Fixed per trade
   - Percentage of trade value
   - Tiered (volume-based)
   - Per share/contract

   **Slippage Models:**
   - Fixed basis points
   - Percentage of volatility
   - Volume-based (market impact)
   - Spread-based (bid-ask)

   **Other Costs:**
   - Borrowing costs for short positions
   - Financing costs for leverage
   - Exchange fees
   - Market data fees (if applicable)

7. **Provide Complete Implementation**

   Deliver:
   - Full Python code with classes and functions
   - Clear docstrings and comments
   - Example usage with sample data
   - Unit tests for critical functions
   - Requirements.txt with dependencies
   - README with setup and usage instructions

## Examples

**Example 1: Simple Moving Average Crossover**
```
User: /quant-strategy-backtest Create a SMA crossover strategy for daily stock data
Claude: I'll create a backtesting framework for a Simple Moving Average crossover strategy.

[Generates complete code with:]

1. Data Handler:
   - Loads OHLCV data
   - Validates data integrity
   - Handles missing values

2. SMA Crossover Strategy:
   - Fast SMA (e.g., 50-day) and Slow SMA (e.g., 200-day)
   - Long signal: fast > slow
   - Exit: fast < slow
   - Parameterized for optimization

3. Portfolio Manager:
   - Fixed position sizing
   - Cash management
   - Position tracking

4. Execution:
   - Market orders at close
   - 0.1% slippage
   - $10 + 0.05% commission

5. Performance Metrics:
   - Sharpe ratio, max drawdown, CAGR
   - Trade statistics
   - Benchmark comparison (buy & hold)

6. Visualization:
   - Equity curve
   - Drawdown chart
   - Signal overlay on price chart

[Full implementation code follows...]
```

**Example 2: Mean Reversion Strategy**
```
User: /quant-strategy-backtest Pairs trading strategy for two stocks
Claude: I'll build a pairs trading backtesting framework.

Strategy Logic:
- Calculate spread between two cointegrated stocks
- Z-score of spread
- Entry: |z-score| > 2
- Exit: z-score crosses 0
- Position sizing: Equal dollar amounts

Key Features:
1. Cointegration testing (Engle-Granger)
2. Rolling z-score calculation
3. Simultaneous long/short execution
4. Spread tracking
5. Market-neutral P&L attribution

Risk Management:
- Stop loss at z-score > 4
- Maximum holding period: 20 days
- Position limits per stock

Transaction Costs:
- Round-trip commission: 0.1%
- Slippage: 5 bps per leg
- Short borrow cost: 1% annually

[Complete implementation with Statsmodels for cointegration...]
```

**Example 3: Multi-Asset Momentum**
```
User: /quant-strategy-backtest Cross-sectional momentum across 100 stocks
Claude: I'll create a quantile-based momentum backtesting system.

Strategy:
- Universe: S&P 500 stocks (or custom universe)
- Ranking: 12-month returns (skip most recent month)
- Long top decile, short bottom decile
- Monthly rebalancing
- Equal-weighted within portfolios

Implementation:
1. Universe Manager:
   - Point-in-time constituents
   - Liquidity filters (min volume, price)
   - Exclude recent IPOs

2. Ranking System:
   - Momentum score calculation
   - Handle missing data
   - Rank and select top/bottom

3. Portfolio Construction:
   - Equal weight or volatility-weighted
   - Portfolio turnover tracking
   - Rebalancing logic

4. Risk Controls:
   - Sector neutrality (optional)
   - Max position size
   - Leverage limits

5. Advanced Metrics:
   - Factor exposure analysis
   - Turnover and capacity estimates
   - Regime analysis (bull/bear markets)

[Full code with Pandas operations for rankings and rebalancing...]
```

## Guidelines

### Backtesting Best Practices

**Data Quality:**
- Always inspect data for errors (negative prices, unrealistic jumps)
- Handle stock splits and dividends correctly
- Use adjusted prices for returns, actual prices for execution
- Account for survivorship bias

**Realistic Assumptions:**
- Don't underestimate transaction costs
- Model slippage especially for illiquid securities
- Consider market impact for large orders
- Account for execution delay (signal → order → fill)

**Risk Management:**
- Implement position limits
- Use stop losses where appropriate
- Monitor leverage and margin
- Test sensitivity to parameters

**Performance Evaluation:**
- Always compare to relevant benchmark
- Report both in-sample and out-of-sample
- Use walk-forward or cross-validation
- Check stability across market regimes

**Code Quality:**
- Modular design for easy strategy iteration
- Comprehensive logging
- Unit tests for calculations
- Version control for strategy parameters

### Common Pitfalls to Avoid

1. **Look-Ahead Bias**: Most common and deadly error
2. **Survivorship Bias**: Using only currently existing stocks
3. **Data Snooping**: Testing too many strategies on same data
4. **Overfitting**: Too many parameters, curve-fitting
5. **Ignoring Costs**: Unrealistic assumptions on commissions/slippage
6. **Sample Size**: Too short backtest period
7. **Regime Dependency**: Strategy works only in specific market conditions
8. **Insufficient Capital**: Not accounting for margin requirements

### Recommended Libraries

**Core:**
- pandas: Data manipulation
- numpy: Numerical computations
- scipy/statsmodels: Statistical functions

**Backtesting:**
- backtrader: Event-driven backtesting
- zipline: Pythonic algorithmic trading
- vectorbt: Fast vectorized backtesting
- bt: Flexible backtesting framework

**Data:**
- yfinance: Yahoo Finance data
- pandas-datareader: Multiple data sources
- Alpha Vantage, Quandl, IEX Cloud

**Performance:**
- pyfolio: Performance and risk analysis
- quantstats: Portfolio analytics
- empyrical: Financial risk metrics

**Visualization:**
- matplotlib, seaborn: Static plots
- plotly: Interactive charts
- mplfinance: Candlestick charts

## Output Structure

Provide code organized as:

```
backtest_framework/
├── data/
│   └── data_handler.py        # Data loading and validation
├── strategy/
│   └── my_strategy.py         # Strategy logic
├── execution/
│   └── simulator.py           # Order execution and costs
├── portfolio/
│   └── portfolio.py           # Position and cash management
├── performance/
│   └── metrics.py             # Performance calculations
│   └── visualization.py       # Plotting functions
├── utils/
│   └── helpers.py             # Utility functions
├── tests/
│   └── test_strategy.py       # Unit tests
├── run_backtest.py            # Main execution script
├── config.yaml                # Strategy parameters
├── requirements.txt           # Dependencies
└── README.md                  # Documentation
```

Or as a single Jupyter notebook for simpler strategies.

## Notes

- Start simple, add complexity gradually
- Backtest is a tool for research, not a guarantee of future performance
- Out-of-sample testing is essential
- Transaction costs can kill a strategy - be realistic
- Consider market regime changes (2008 crisis, COVID, etc.)
- Document all assumptions clearly
- Keep parameter count low to avoid overfitting
- For production: Add error handling, logging, monitoring
- Consider scaling - will strategy work with more capital?
- Think about implementation: Can you actually trade this?

## References to Suggest

- "Advances in Financial Machine Learning" by Marcos López de Prado
- "Evidence-Based Technical Analysis" by David Aronson
- "Quantitative Trading" by Ernest Chan
- "Algorithmic Trading" by Ernie Chan
- QuantStart.com tutorials
- Quantopian lectures (archived)
