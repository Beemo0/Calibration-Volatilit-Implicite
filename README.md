# TP1 — Implied Volatility Calibration and Volatility Smile (Black-Scholes)

## Overview

This project implements several calibration procedures for the **Black-Scholes option pricing model** using real market data.

The main objective is to compute **implied volatility** from observed market option prices and analyze the resulting **volatility smile** across different strikes and maturities.

The project also studies the effect of **dividends** and applies the methodology to multiple datasets including market index options and equity options.

The implementation is written in **Python** and relies on numerical methods such as the **Newton algorithm** for solving nonlinear equations.

---

## Project Structure


TP1/
├── TP1.py # Python implementation of all computations
├── sp-index.txt 
├── NEW_spx_quotedata.csv
├── spx_quotedata.csv
└── README.md # Project documentation


---

# Theoretical Background

## Black-Scholes Option Pricing

The Black-Scholes model provides a theoretical price for European options.

For a **Call option**, the price is given by:

\[
C = S_0 N(d_1) - K e^{-rT} N(d_2)
\]

with

\[
d_1 = \frac{\ln(S_0/K) + (r + \sigma^2/2)T}{\sigma \sqrt{T}}
\]

\[
d_2 = d_1 - \sigma \sqrt{T}
\]

where:

| Variable | Meaning |
|--------|--------|
| \(S_0\) | underlying asset price |
| \(K\) | strike price |
| \(T\) | time to maturity |
| \(r\) | risk-free interest rate |
| \(σ\) | volatility |
| \(N(x)\) | standard normal cumulative distribution |

---

# Implied Volatility

The **implied volatility** is the value of \(σ\) such that the Black-Scholes price equals the market price:

\[
V_{BS}(σ) = V_{market}
\]

This leads to solving the nonlinear equation:

\[
F(σ) = V_{BS}(σ) - V_{market} = 0
\]

To compute the solution we apply the **Newton iterative algorithm**:

\[
σ_{n+1} = σ_n - \frac{F(σ_n)}{F'(σ_n)}
\]

The derivative \(F'(σ)\) corresponds to the **Vega** of the option.

---

# Implemented Functions

The code contains the following main functions:

### Pricing Functions

- `BlackScholes_Call()`
- `BlackScholes_Put()`

Compute the theoretical price of European Call and Put options.

---

### Greeks

- `Vega()`

Measures the sensitivity of the option price to volatility.

This quantity is essential for the Newton algorithm.

---

### Root Finding

- `Newton()`

Iterative method used to compute implied volatility.

---

### Arbitrage Checks

Functions:

- `verification()`
- `verification2()`

These ensure that the market option prices satisfy **no-arbitrage bounds** before attempting to compute implied volatility.

---

# Part I — LIFFE Options Dataset

The first part of the project calibrates implied volatility using option prices from the **London International Financial Futures and Options Exchange (LIFFE)**.

Given parameters:

- \(S_0 = 5430.3\)
- \(r = 0.05\)
- \(T = 4/12\)

For each strike \(K_i\), the algorithm computes the implied volatility \(σ_i\).

The resulting values are plotted as a function of the strike price.

This produces the **volatility smile**.

---

# Part II — Dividends and SP-INDEX Dataset

When the underlying asset pays dividends, the Black-Scholes formula becomes:

\[
C = S_0 e^{-qT} N(d_1) - K e^{-rT} N(d_2)
\]

where \(q\) is the **continuous dividend rate**.

The modified **Call-Put parity** becomes:

\[
C - P = S_0 e^{-qT} - K e^{-rT}
\]

This relation allows estimating:

- the **initial asset price \(S_0\)**
- the **dividend rate \(q\)**

using market option prices.

---

## Estimating Dividend Rate

For each maturity \(T_j\), an estimate of the discounted underlying price is computed:

\[
\hat S_j =
\frac{1}{N_j}
\sum_i
(Call - Put + K e^{-rT})
\]

Taking the logarithm:

\[
\ln(\hat S_j) = \ln(S_0) - qT_j
\]

A **linear regression** is performed:

\[
\ln(\hat S_j) = β_1 T_j + β_2
\]

with

- \(q = -β_1\)
- \(S_0 = e^{β_2}\)

The code verifies the theoretical values:

```
S0 ≈ 1260.36
q ≈ 0.0217
```

---

# Volatility Smile Visualization

The project generates several plots:

### 2D Smile

Implied volatility vs strike

```
σ_imp(K)
```

for:

- Call options
- Put options

---

### 3D Surface

Implied volatility as a function of:

- strike \(K\)
- maturity \(T\)

```
σ_imp(K, T)
```

These visualizations illustrate the **volatility smile** and the **volatility surface** observed in financial markets.

---

# Additional Market Datasets

The implementation also processes other datasets:

### SPX Options

Dataset containing:

- bid/ask prices
- strike prices
- expiration dates

From these data the script:

1. computes mid-quotes
2. calculates implied volatility
3. visualizes smiles and surfaces

---

### Google Options Dataset

Used to study the shape of volatility smiles under different dividend assumptions.

The script allows adjusting the **dividend rate \(q\)** to obtain smoother volatility curves.

---

# Libraries Used

The project relies on the following Python libraries:

```
numpy
matplotlib
pandas
scipy
scikit-learn
datetime
```

Install dependencies with:

```bash
pip install numpy matplotlib pandas scipy scikit-learn
```

---

# Usage

Run the main script:

```bash
python code.py
```

The script will:

1. load market datasets
2. compute implied volatilities
3. generate volatility smile plots
4. visualize volatility surfaces

---

# Key Concepts Covered

This project covers several important topics in **quantitative finance**:

- Black-Scholes option pricing
- implied volatility calibration
- Newton numerical methods
- option Greeks (Vega)
- volatility smile
- volatility surface
- call-put parity
- dividend modeling
- linear regression calibration

---

# Disclaimer

This repository contains the **code and methodology** used in an academic laboratory session.

The original written report has been removed due to **academic and data usage restrictions**.

```

---
