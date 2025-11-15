# Code Examples & Templates

Practical code examples to kickstart your quantitative finance projects.

---

## Getting Started

This directory contains implementation examples for common quantitative finance tasks:
- **Data collection**: APIs, web scraping, data cleaning
- **Strategy development**: Mean-reversion, momentum, volatility trading
- **Backtesting**: Simple frameworks and performance evaluation
- **Visualization**: Equity curves, correlation matrices, risk metrics
- **Statistics**: Time series analysis, cointegration tests, Monte Carlo

---

# Code Examples

This directory is for storing full code implementations and scripts.

**For code snippets and quick examples**, see [Tutorials - Code Examples](../docs/tutorials/code-examples.md).

## Directory Structure

```
examples/
├── data_collection/       # API scripts, web scraping
├── strategies/           # Full strategy implementations
├── backtesting/          # Complete backtesting frameworks
├── analysis/             # Performance and risk analysis tools
└── visualization/        # Plotting and charting scripts
```

## Usage

Place full Python scripts, Jupyter notebooks, and complete implementations here. For quick reference examples and code snippets, see the tutorials section.

**Contribute code?** See [Community Contributions](../docs/community/README.md)

---

## Quick Examples

### 1. Simple Moving Average Crossover (Python)

```python
import pandas as pd
import numpy as np
import yfinance as yf

# Download data
ticker = yf.Ticker("SPY")
data = ticker.history(period="5y")

# Calculate moving averages
data['SMA_50'] = data['Close'].rolling(50).mean()
data['SMA_200'] = data['Close'].rolling(200).mean()

# Generate signals
data['Signal'] = 0
data.loc[data['SMA_50'] > data['SMA_200'], 'Signal'] = 1
data.loc[data['SMA_50'] < data['SMA_200'], 'Signal'] = -1

# Calculate returns
data['Returns'] = data['Close'].pct_change()
data['Strategy_Returns'] = data['Signal'].shift(1) * data['Returns']

# Performance
sharpe = data['Strategy_Returns'].mean() / data['Strategy_Returns'].std() * np.sqrt(252)
print(f"Sharpe Ratio: {sharpe:.2f}")
```

### 2. Pairs Trading Cointegration Test (Python)

```python
import pandas as pd
import numpy as np
from statsmodels.tsa.stattools import coint
import yfinance as yf

# Download stock data
stock_a = yf.download('NVDA', start='2020-01-01')['Adj Close']
stock_b = yf.download('AMD', start='2020-01-01')['Adj Close']

# Test for cointegration
score, pvalue, _ = coint(stock_a, stock_b)

print(f"Cointegration p-value: {pvalue:.4f}")
if pvalue < 0.05:
    print("Stocks are cointegrated (reject null of no cointegration)")
else:
    print("Stocks are NOT cointegrated")

# Calculate spread
hedge_ratio = np.polyfit(stock_b, stock_a, 1)[0]
spread = stock_a - hedge_ratio * stock_b

# Z-score
z_score = (spread - spread.mean()) / spread.std()
print(f"Current Z-score: {z_score.iloc[-1]:.2f}")
```

### 3. Performance Metrics Calculator (Python)

```python
import pandas as pd
import numpy as np

def calculate_metrics(returns):
    """
    Calculate key performance metrics from returns series
    
    Parameters:
    returns: pd.Series of strategy returns
    
    Returns:
    dict of performance metrics
    """
    # Annualized return (assuming daily returns)
    annual_return = returns.mean() * 252
    
    # Annualized volatility
    annual_vol = returns.std() * np.sqrt(252)
    
    # Sharpe ratio
    sharpe = annual_return / annual_vol
    
    # Maximum drawdown
    cumulative = (1 + returns).cumprod()
    running_max = cumulative.expanding().max()
    drawdown = (cumulative - running_max) / running_max
    max_drawdown = drawdown.min()
    
    # Calmar ratio
    calmar = annual_return / abs(max_drawdown) if max_drawdown != 0 else np.nan
    
    # Sortino ratio (downside deviation)
    downside_returns = returns[returns < 0]
    downside_dev = downside_returns.std() * np.sqrt(252)
    sortino = annual_return / downside_dev if downside_dev != 0 else np.nan
    
    # Win rate
    win_rate = (returns > 0).sum() / len(returns)
    
    return {
        'Annual Return': f"{annual_return:.2%}",
        'Annual Volatility': f"{annual_vol:.2%}",
        'Sharpe Ratio': f"{sharpe:.2f}",
        'Sortino Ratio': f"{sortino:.2f}",
        'Max Drawdown': f"{max_drawdown:.2%}",
        'Calmar Ratio': f"{calmar:.2f}",
        'Win Rate': f"{win_rate:.2%}"
    }

# Usage
# metrics = calculate_metrics(strategy_returns)
# print(metrics)
```

### 4. Options Greeks Calculator (Python)

```python
import numpy as np
from scipy.stats import norm

def black_scholes_greeks(S, K, T, r, sigma, option_type='call'):
    """
    Calculate Black-Scholes Greeks
    
    S: Spot price
    K: Strike price
    T: Time to expiration (years)
    r: Risk-free rate
    sigma: Volatility
    option_type: 'call' or 'put'
    """
    d1 = (np.log(S/K) + (r + 0.5*sigma**2)*T) / (sigma*np.sqrt(T))
    d2 = d1 - sigma*np.sqrt(T)
    
    if option_type == 'call':
        delta = norm.cdf(d1)
        price = S*norm.cdf(d1) - K*np.exp(-r*T)*norm.cdf(d2)
    else:
        delta = -norm.cdf(-d1)
        price = K*np.exp(-r*T)*norm.cdf(-d2) - S*norm.cdf(-d1)
    
    gamma = norm.pdf(d1) / (S * sigma * np.sqrt(T))
    vega = S * norm.pdf(d1) * np.sqrt(T) / 100  # per 1% change
    theta = (-S*norm.pdf(d1)*sigma / (2*np.sqrt(T)) - 
             r*K*np.exp(-r*T)*norm.cdf(d2 if option_type=='call' else -d2)) / 365
    
    return {
        'Price': price,
        'Delta': delta,
        'Gamma': gamma,
        'Vega': vega,
        'Theta': theta
    }

# Usage
# greeks = black_scholes_greeks(S=100, K=100, T=0.25, r=0.05, sigma=0.2)
# print(greeks)
```

---

## How to Use

1. **Clone examples**: Copy relevant code to your project
2. **Modify for your data**: Adapt to your specific assets, timeframes
3. **Extend functionality**: Add features like transaction costs, slippage
4. **Combine approaches**: Mix strategies, data sources, analysis methods
5. **Share improvements**: Contribute back via [Community](../community/README.md)

---

## Best Practices

- **Test on sample data first**: Don't run untested code on full datasets
- **Use version control**: Git track all changes
- **Document parameters**: Comment what parameters do and reasonable ranges
- **Handle errors**: Add try-except blocks for API calls and data issues
- **Vectorize when possible**: Use pandas/numpy operations instead of loops

---

## Additional Resources

### Libraries to Know
- **Data**: `yfinance`, `pandas-datareader`, `ccxt` (crypto)
- **Analysis**: `pandas`, `numpy`, `scipy`, `statsmodels`
- **Backtesting**: `backtrader`, `zipline`, `vectorbt`
- **Visualization**: `matplotlib`, `seaborn`, `plotly`
- **Machine Learning**: `scikit-learn`, `xgboost`, `tensorflow`

### Related Sections
- [Research Pipeline](../tutorials/research-pipeline.md)
- [Backtesting Framework](../tutorials/backtesting-framework.md)
- [Active Projects](../projects/README.md)

---

**Have code to contribute?** See [Community Contributions](../community/README.md)

---

## Note

These are educational examples for learning purposes. Always:
- Backtest thoroughly before live trading
- Understand risks and position sizing
- Account for transaction costs and slippage
- Never trade with money you can't afford to lose
- Consider regulatory and tax implications
