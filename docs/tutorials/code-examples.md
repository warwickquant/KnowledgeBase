# Code Examples & Templates

Practical implementations to kickstart your quantitative finance projects.

## Quick Start Examples

### 1. Moving Average Crossover

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

### 2. Pairs Trading Cointegration Test

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
    print("Stocks are cointegrated")
else:
    print("Stocks are NOT cointegrated")

# Calculate spread
hedge_ratio = np.polyfit(stock_b, stock_a, 1)[0]
spread = stock_a - hedge_ratio * stock_b

# Z-score
z_score = (spread - spread.mean()) / spread.std()
print(f"Current Z-score: {z_score.iloc[-1]:.2f}")
```

### 3. Performance Metrics Calculator

```python
import pandas as pd
import numpy as np

def calculate_metrics(returns):
    """Calculate key performance metrics from returns series"""
    
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
```

### 4. Black-Scholes Greeks

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
greeks = black_scholes_greeks(S=100, K=100, T=0.25, r=0.05, sigma=0.2)
print(greeks)
```

## Essential Libraries

### Data Collection
- **yfinance** - Yahoo Finance API
- **pandas-datareader** - Multiple data sources
- **ccxt** - Cryptocurrency exchange APIs

### Analysis & Backtesting
- **pandas** - Data manipulation
- **numpy** - Numerical computing
- **scipy** - Scientific computing
- **statsmodels** - Statistical models
- **backtrader** - Backtesting framework
- **vectorbt** - Vectorized backtesting

### Visualization
- **matplotlib** - Basic plotting
- **seaborn** - Statistical visualization
- **plotly** - Interactive charts

### Machine Learning
- **scikit-learn** - Classical ML algorithms
- **xgboost** - Gradient boosting
- **tensorflow/pytorch** - Deep learning

## Best Practices

- **Test on sample data first** before running on full datasets
- **Use version control** (Git) to track all changes
- **Document parameters** with comments and docstrings
- **Handle errors** with try-except blocks for API calls
- **Vectorize operations** using pandas/numpy instead of loops

## Related Tutorials
- [Research Pipeline](./research-pipeline.md) - Full research workflow
- [Backtesting Framework](./backtesting-framework.md) - Build robust backtests

**Suggest an addition?** See [Community Contributions](../community/README.md)

---

**⚠️ Disclaimer**: These are educational examples. Always backtest thoroughly, understand risks, account for transaction costs, and never trade with money you can't afford to lose.
