# SGX Stock Portfolio Tracker 📈

## What this does
Tracks and analyses the 1-year price performance of 
3 SGX-listed stocks: DBS (D05.SI), OCBC (O39.SI), and SGX (S68.SI).

Computes annual returns and 20/50-day moving averages 
to identify price trends and momentum signals.

## Why I built this
I'm an incoming NUS Business Analytics student targeting 
a career in financial analytics and risk management at 
institutions like GIC, MAS, and DBS.

I built this project to understand how analysts monitor 
portfolio performance using Python — as well as to 
apply the Pandas and data visualisation skills 
I developed through Harvard CS50P and Kaggle.

## Tools used
- Python 3
- yfinance (live SGX price data)
- Pandas (computes returns and moving averages)
- Matplotlib (visualises price trends)
- SQLite3 (stores stock data in SQL database,
           queried with pandas read_sql)

## Key outputs
- Annual return (%) for each stock
- 20-day and 50-day moving averages
- Price trend chart with MA crossover signals
- SQL database of historical prices with query output

## Sample output
<img width="1488" height="738" alt="Screenshot 2026-07-28 204541" src="https://github.com/user-attachments/assets/49bb8710-4f14-47d6-ab28-dd1bde4c3adc" />


## Core Code

```python
import yfinance as yf
import pandas as pd
import matplotlib.pyplot as plt
import sqlite3

# Pull 1 year of live SGX data
tickers = ['D05.SI', 'O39.SI', 'S68.SI']
data = yf.download(tickers, period='1y', auto_adjust=True)['Close']

# Calculate annual returns
start_price = data.iloc[0]
end_price = data.iloc[-1]
returns = (end_price - start_price) / start_price * 100

# 20 and 50 day moving averages
dbs = data['D05.SI']
dbs_20ma = dbs.rolling(window=20).mean()
dbs_50ma = dbs.rolling(window=50).mean()

# Bullish/bearish signal
if dbs_20ma.iloc[-1] > dbs_50ma.iloc[-1]:
    signal = "BULLISH — 20MA above 50MA"
else:
    signal = "BEARISH — 20MA below 50MA"

# Store in SQL database and query
conn = sqlite3.connect('sgx_stocks.db')
data.to_sql('stock_prices', conn, if_exists='replace')
query = '''
    SELECT Date, "D05.SI" as DBS, 
           "O39.SI" as OCBC, "S68.SI" as SGX
    FROM stock_prices
    ORDER BY Date DESC
    LIMIT 10
'''
result = pd.read_sql_query(query, conn)
conn.close()
```

## What I Learnt

**Python & Data Engineering**
- How to pull live financial data via API using yfinance
- How to structure a financial data pipeline end-to-end
- How to store and query financial data using SQLite3
- How to combine Python, Pandas and SQL in one workflow

**Financial Analysis**
- How moving averages signal price momentum and trend direction
- How to interpret bullish and bearish MA crossover signals
- How annual returns are calculated and compared across stocks
- How Singapore blue chip stocks (DBS, OCBC, SGX) performed 
  over a full market cycle

**Data Visualisation**
- How to build clean, professional financial charts using Matplotlib
- How to format output as a structured analyst-style summary report

**Key insight from this project:**
All three SGX stocks delivered exceptional returns over the 
analysis period (DBS +63%, OCBC +82%, SGX +56%), significantly 
outperforming the global market average of 8–10% annually. 
DBS maintained a consistent BULLISH signal throughout, with 
the 20-day MA remaining above the 50-day MA.

## Author
Lee Yu Hng 

Incoming NUS Business Analytics (Aug 2027)  
NUS Undergraduate Scholar  
www.linkedin.com/in/yuhng-lee-404709404
