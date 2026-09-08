# Financial-Libraries-in-python

Small, self-contained examples of Python libraries for options pricing and
stock price prediction.

## Contents

### `py_vollib_example.py`
Black-Scholes option pricing and the full set of Greeks (delta, gamma, vega,
theta, rho) using [`py_vollib`](https://github.com/vollib/py_vollib).

```python
from py_vollib.black_scholes import black_scholes as bs
from py_vollib.black_scholes.greeks.analytical import delta, gamma, vega, theta, rho

call_price = bs('c', S=60, K=55, t=30/365, r=0.05, sigma=0.20)
```

### `mibian_example.py`
The same Black-Scholes pricing via [`mibian`](https://github.com/yassinemaaroufi/MibianLib) —
a simpler API, but note it doesn't provide theta or rho (only price, delta,
gamma, vega).

```python
import mibian as mb
c = mb.BS([spot, strike, interest_rate, days_to_expiry], volatility=20)
print(c.callPrice, c.callDelta, c.gamma, c.vega)
```

### `Stock_Price_Predictor_ML.ipynb`
Compares Linear Regression, a Decision Tree and a Random Forest at forecasting
the **forward 25-day return** of ETERNAL.NS (formerly ZOMATO.NS), pulled with
`yfinance`. The headline result is negative — none of the three beats a random
walk — and the notebook is built around showing why that is the trustworthy
answer.

An earlier version reported R² ≈ 0.98. That number came from three mistakes,
which the notebook now reproduces in Section 1 before fixing:

| | Original | Now |
|---|---|---|
| Split | `train_test_split(..., random_state=42)` — shuffled, so test rows sit between train rows on the calendar | Chronological, with a 25-day purge gap at the boundary |
| Target | `Close.shift(-25)` — a price **level**, with `Close` as an input feature | `log(Close_{t+25} / Close_t)` — a **return** |
| Features | OHLCV levels | Multi-horizon returns, realised vol, ATR, volume z-score |
| Baseline | none | Random walk (forward return = 0), plus walk-forward validation |
| Reported R² | 0.977 | negative — the models lose to the baseline |

The three numbers that make the point, same model and data throughout:

```
(a) shuffled split, price levels     R2 =  0.9448   <- the old headline
(b) naive 'in 25 days = today'       R2 =  0.9439   <- no model at all
(c) same model, chronological split  R2 =  0.1670   <- out of sample
```

Line (b) is the tell: against a price-level target with `Close` as a feature,
R² measures autocorrelation, not skill, so a forecast that ignores the data
entirely scores 0.94. The model's actual contribution was +0.0009 R².

Runs in Colab or locally — no `google.colab` imports, and no CSV round-trip.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/advait-srivastava/Financial-Libraries-in-python/blob/main/Stock_Price_Predictor_ML.ipynb)

## Setup

```bash
pip install -r requirements.txt        # py_vollib, mibian
pip install yfinance pandas numpy scikit-learn matplotlib   # notebook only
```
