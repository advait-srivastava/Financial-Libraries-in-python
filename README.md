# Financial-Libraries-in-python

Small, self-contained examples of Python libraries for options pricing and
stock price prediction.

## Contents

### `py_vollib library`
Black-Scholes option pricing and the full set of Greeks (delta, gamma, vega,
theta, rho) using [`py_vollib`](https://github.com/vollib/py_vollib).

```python
from py_vollib.black_scholes import black_scholes as bs
from py_vollib.black_scholes.greeks.analytical import delta, gamma, vega, theta, rho

call_price = bs('c', S=60, K=55, t=30/365, r=0.05, sigma=0.20)
```

### `Mibian library`
The same Black-Scholes pricing via [`mibian`](https://github.com/yassinemaaroufi/MibianLib) —
a simpler API, but note it doesn't provide theta or rho (only price, delta,
gamma, vega).

```python
import mibian as mb
c = mb.BS([spot, strike, interest_rate, days_to_expiry], volatility=20)
print(c.callPrice, c.callDelta, c.gamma, c.vega)
```

### `Stock_Price_Predictor_ML.ipynb`
A Google Colab notebook that pulls historical price data for a stock
(ZOMATO.NS, via `yfinance`) and compares three regression models —
Linear Regression, Decision Tree, and Random Forest — for next-day price
prediction, evaluated with MSE and R².

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/advait-srivastava/Financial-Libraries-in-python/blob/main/Stock_Price_Predictor_ML.ipynb)

## Setup

```bash
pip install py_vollib mibian yfinance pandas numpy scikit-learn matplotlib seaborn
```
