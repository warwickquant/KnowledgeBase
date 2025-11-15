# Backtesting Framework Guide

A practical guide to building robust backtesting frameworks for quantitative trading strategies.

---

## Why Backtesting Matters

Backtesting allows you to:
- Validate strategy ideas before risking capital
- Estimate expected returns and risks
- Identify parameter sensitivity and overfitting
- Understand strategy behavior in different market conditions

**Warning**: Backtesting is not predictive. Past performance ≠ future results. Be skeptical of results that seem too good.

---

## 1. Framework Requirements

### Essential Components
- **Data handling**: Load, clean, and align multiple data sources
- **Signal generation**: Compute indicators and trading signals
- **Position sizing**: Determine how much to trade
- **Order execution**: Simulate realistic fills (market/limit orders)
- **Portfolio tracking**: Track positions, cash, equity over time
- **Performance metrics**: Calculate returns, Sharpe, drawdown, etc.
- **Transaction costs**: Model commissions, slippage, market impact

### Nice-to-Have Features
- Walk-forward optimization
- Monte Carlo simulation
- Parameter sensitivity analysis
- Regime detection
- Multi-asset support

---

## 2. Common Pitfalls to Avoid

### Lookahead Bias
**Problem**: Using future information not available at trade time  
**Example**: Using close price at time `t` to generate signal at time `t`  
**Solution**: Only use data from `t-1` or earlier for decisions at time `t`

### Survivorship Bias
**Problem**: Only backtesting on stocks that survived (ignoring delisted/bankrupt)  
**Example**: Testing on current S&P 500 constituents from 2000-2020  
**Solution**: Use point-in-time constituent data or survivorship-bias-free datasets

### Overfitting
**Problem**: Optimizing parameters until backtest looks great (but doesn't generalize)  
**Example**: Testing 100 parameter combinations, picking the best  
**Solution**: Use train/test split, limit optimization, prefer simple strategies

### Unrealistic Assumptions
**Problem**: Assuming perfect fills, no slippage, infinite liquidity  
**Example**: Backtesting high-frequency strategy with zero transaction costs  
**Solution**: Model realistic transaction costs, limit orders, market impact

---

## 3. Backtesting Workflow

### Step 1: Data Preparation
```python
import pandas as pd
import numpy as np

# Load data
prices = pd.read_csv('stock_prices.csv', index_col='date', parse_dates=True)

# Clean data
prices = prices.dropna()  # or forward-fill
prices = prices[prices > 0]  # remove invalid prices

# Compute returns
returns = prices.pct_change()

# Align multiple dataframes
data = pd.concat([prices, volume, fundamentals], axis=1).dropna()
```

### Step 2: Signal Generation
```python
# Example: Moving average crossover
data['SMA_50'] = prices.rolling(50).mean()
data['SMA_200'] = prices.rolling(200).mean()

# Generate signals (1 = long, 0 = neutral, -1 = short)
data['signal'] = 0
data.loc[data['SMA_50'] > data['SMA_200'], 'signal'] = 1
data.loc[data['SMA_50'] < data['SMA_200'], 'signal'] = -1

# Lag signals to avoid lookahead bias
data['signal'] = data['signal'].shift(1)
```

### Step 3: Position Sizing
```python
# Simple: Fixed position size
data['position'] = data['signal'] * 1.0  # 100% of capital

# Better: Volatility-adjusted sizing (inverse volatility)
data['volatility'] = returns.rolling(20).std()
data['position'] = data['signal'] / data['volatility']
data['position'] = data['position'] / data['position'].abs().max()  # normalize

# Advanced: Kelly criterion, risk parity, etc.
```

### Step 4: Calculate Returns
```python
# Strategy returns
data['strategy_returns'] = data['position'].shift(1) * returns

# Cumulative returns
data['cumulative_returns'] = (1 + data['strategy_returns']).cumprod()

# Account for transaction costs
data['trades'] = data['position'].diff().abs()
transaction_costs = 0.001  # 10 bps per trade
data['strategy_returns'] -= data['trades'] * transaction_costs
```

### Step 5: Performance Metrics
```python
# Sharpe ratio (annualized, assuming daily data)
sharpe = data['strategy_returns'].mean() / data['strategy_returns'].std() * np.sqrt(252)

# Maximum drawdown
cumulative = (1 + data['strategy_returns']).cumprod()
running_max = cumulative.expanding().max()
drawdown = (cumulative - running_max) / running_max
max_drawdown = drawdown.min()

# Calmar ratio
annual_return = data['strategy_returns'].mean() * 252
calmar = annual_return / abs(max_drawdown)

print(f"Sharpe Ratio: {sharpe:.2f}")
print(f"Max Drawdown: {max_drawdown:.2%}")
print(f"Calmar Ratio: {calmar:.2f}")
```

<!-- ---

## 4. Recommended Libraries

### Python

#### **Backtrader**
- Comprehensive event-driven framework
- Supports multiple data feeds, strategies, indicators
- Built-in optimization and walk-forward analysis
```python
import backtrader as bt

class MyStrategy(bt.Strategy):
    def __init__(self):
        self.sma = bt.indicators.SMA(period=50)
    
    def next(self):
        if self.data.close > self.sma:
            self.buy()
        elif self.data.close < self.sma:
            self.sell()
```

#### **Zipline**
- Used by Quantopian (now archived but still functional)
- Handles data alignment, calendar, orders automatically
- Realistic slippage and commission models
```python
from zipline import run_algorithm
from zipline.api import order_target_percent, symbol

def initialize(context):
    context.asset = symbol('AAPL')

def handle_data(context, data):
    if data.can_trade(context.asset):
        order_target_percent(context.asset, 1.0)
```

#### **VectorBT**
- Vectorized backtesting (fast for simple strategies)
- Great for parameter optimization
- Easy visualization
```python
import vectorbt as vbt

# Simulate SMA crossover
fast_ma = vbt.MA.run(prices, 50)
slow_ma = vbt.MA.run(prices, 200)

entries = fast_ma.ma_crossed_above(slow_ma)
exits = fast_ma.ma_crossed_below(slow_ma)

portfolio = vbt.Portfolio.from_signals(prices, entries, exits)
portfolio.stats()
```

### R

#### **quantmod / blotter / quantstrat**
- `quantmod`: Data retrieval and charting
- `blotter`: Portfolio accounting
- `quantstrat`: Strategy specification and backtesting
```r
library(quantstrat)

# Define strategy
strategy("my_strat")
add.indicator(strategy = "my_strat", name = "SMA", arguments = list(x = quote(Cl(mktdata)), n = 50))
add.signal(strategy = "my_strat", name = "sigCrossover", ...)
```

--- -->

<!-- ## 5. Advanced Topics

### Walk-Forward Optimization
Avoid overfitting by optimizing on rolling windows:
1. **In-sample period** (e.g., 2015-2017): Optimize parameters
2. **Out-of-sample period** (e.g., 2018): Test with optimized parameters
3. Roll forward and repeat

### Monte Carlo Simulation
Test robustness by randomizing:
- Trade order (shuffle trades)
- Entry/exit timing (add noise)
- Returns distribution (resample historical returns)

Ensure strategy still performs across many simulations.

### Realistic Order Execution
Model different order types:
- **Market orders**: Immediate fill, but slippage
- **Limit orders**: Better price, but may not fill
- **Market impact**: Large orders move prices

### Multi-Asset Backtesting
Challenges:
- Different trading calendars
- Cross-asset correlation
- Portfolio-level risk management
- Rebalancing logic

--- -->

## 6. Backtesting Checklist

Before trusting your backtest:
- [ ] No lookahead bias (signals use only past data)
- [ ] Out-of-sample testing performed
- [ ] Transaction costs included (realistic estimates)
- [ ] Slippage modeled
- [ ] Market impact considered for large trades
- [ ] Survivorship bias addressed
- [ ] Parameter sensitivity tested (does small change break strategy?)
- [ ] Regime analysis performed (does strategy work in all market conditions?)
- [ ] Drawdown tolerance acceptable
- [ ] Turnover/capacity appropriate for implementation

---

## 7. Example: Complete Backtest

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load data
prices = pd.read_csv('prices.csv', index_col='date', parse_dates=True)['close']
returns = prices.pct_change()

# Parameters
fast_period = 50
slow_period = 200
transaction_cost = 0.001

# Indicators
fast_ma = prices.rolling(fast_period).mean()
slow_ma = prices.rolling(slow_period).mean()

# Signals
signal = pd.Series(0, index=prices.index)
signal[fast_ma > slow_ma] = 1
signal[fast_ma < slow_ma] = -1
signal = signal.shift(1)  # Avoid lookahead

# Position changes (for transaction costs)
trades = signal.diff().abs()

# Strategy returns
strategy_returns = signal * returns - trades * transaction_cost

# Performance
cumulative = (1 + strategy_returns).cumprod()
sharpe = strategy_returns.mean() / strategy_returns.std() * np.sqrt(252)
max_dd = (cumulative / cumulative.expanding().max() - 1).min()

print(f"Sharpe: {sharpe:.2f}, Max DD: {max_dd:.2%}")

# Plot
cumulative.plot(title='Strategy Performance')
plt.show()
```

---

## Resources

### Related Sections
- [Research Pipeline](./research-pipeline.md)
- [Active Projects](../projects/README.md)

---

**Have improvements or examples to share?** See [Community Contributions](../community/README.md)
