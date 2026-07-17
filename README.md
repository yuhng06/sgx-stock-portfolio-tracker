# SGX Stock Portfolio Tracker 📈

## What this does
Tracks and analyses the 1-year price performance of 3 
SGX-listed stocks: DBS (D05.SI), OCBC (O39.SI), and 
SGX (S68.SI).

Computes annual returns and 20/50-day moving averages 
to identify price trends and momentum signals.

## Why I built this
I'm an incoming NUS Business Analytics student targeting 
a career in financial analytics and risk management at 
institutions like GIC, MAS, and DBS.

I built this project to understand how analysts monitor 
portfolio performance using Python — and to apply the 
Pandas and data visualisation skills I developed through 
Harvard CS50P and Kaggle.

## Tools used
- **Python 3**
- **yfinance** — pulls live SGX price data
- **Pandas** — computes returns and moving averages
- **Matplotlib** — visualises price trends

## Key outputs
- Annual return (%) for each stock
- 20-day and 50-day moving averages
- Price trend chart with MA crossover signals

## Sample output


## Core code
```python
import yfinance as yf
import pandas as pd
import matplotlib.pyplot as plt

# DBS, OCBC, SGX
tickers = ['D05.SI', 'O39.SI', 'S68.SI']

# Pull 1 year of data
data = yf.download(tickers, period='1y')['Close']

# Calculate annual returns
returns = (data.iloc[-1] - data.iloc[0]) / data.iloc[0] * 100

# 20 and 50 day moving averages
dbs = data['D05.SI']
dbs_20ma = dbs.rolling(window=20).mean()
dbs_50ma = dbs.rolling(window=50).mean()
```

## What I learned
- How to pull live financial data via API
- How moving averages signal price momentum
- How to structure a financial data pipeline in Python
- Basics of technical analysis used by professional analysts

## Author
Lee Yu Hng 
Incoming NUS Business Analytics (Aug 2027)  
NUS Undergraduate Scholar  
www.linkedin.com/in/yuhng-lee-404709404
