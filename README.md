# Bermudan Options Pricer (Longstaff-Schwartz Monte Carlo)

This repository provides a Python implementation of Bermudan option pricing using the **Longstaff-Schwartz least-squares Monte Carlo (LSMC)** algorithm, **geometric Brownian motion (GBM)** simulation, and **Black-Scholes-based regression**.

The goal is to estimate continuation values, determine optimal early exercise decisions, and compute Bermudan/American option prices in a clear, notebook-driven framework.

## Features

- Longstaff-Schwartz algorithm with backward induction  
- Monte Carlo simulation using geometric Brownian motion  
- Black-Scholes basis regression for continuation values  
- Modular, easy-to-extend notebook code  
- Workflow suitable for learning, experimentation, and research  

## Project Structure

```text
bermudan-pricer.ipynb     # Main implementation notebook
README.md                 # Project documentation
```

If your repository includes additional notebooks (for example, on regression or conditional expectation), they can live alongside `bermudan-pricer.ipynb` in the same directory.

## How It Works

### 1. Simulate underlying price paths (GBM)

Monte Carlo simulation generates stock price paths \(S_t\) under the risk-neutral measure:

```python
def blackscholes_mc(ts, n_paths, S0, vol, r, q):
    ...
```

### 2. Compute exercise values

For a Bermudan put option:

```python
ex = np.maximum(K - S, 0.0)
itm = ex > 0  # in the money
```

### 3. Regress continuation value

On in-the-money paths, use Black-Scholes prices as basis functions to approximate the continuation value:

```python
X = np.column_stack([np.ones_like(bs_itm), bs_itm])
beta = np.linalg.lstsq(X, pay[itm], rcond=None)[0]
```

This regression provides an estimate of the expected continuation payoff conditional on the current state.

### 4. Optimal exercise decision

At each Bermudan exercise date:

- If `exercise_value > continuation_value`, exercise early.  
- Otherwise, continue and keep the discounted future payoff.  

### 5. Discount and average

The Bermudan option price is obtained as the discounted mean of the optimal payoffs across all simulated paths.

## Example Usage

Within `bermudan-pricer.ipynb`:

```python
S0 = 223.78
K = 180
vol = 0.24
r = 0.05
q = 0.02
T = 4 / 12
```

Run the Longstaff-Schwartz policy and apply it to the simulated paths:

```python
pol = ls_policy_bs(ts, paths, K, r, q)
value = apply_policy_bs(pol, paths)
value.mean()
```

This returns the estimated Bermudan put price under the chosen model parameters.

## Installation

Clone the repository:

```sh
git clone https://github.com/yourusername/bermudan-pricer.git
cd bermudan-pricer
```

Install dependencies (either from `requirements.txt` if present, or manually):

```sh
pip install -r requirements.txt
```

If you prefer to install packages directly, you will at minimum need the standard scientific Python stack (for example, `numpy`, `scipy`, and plotting libraries such as `matplotlib`).

## Background: Longstaff-Schwartz

The Longstaff-Schwartz method (2001) approximates the continuation value of an American or Bermudan option using regression:

- Simulate many price paths under the risk-neutral measure.  
- Move backwards from maturity through the exercise dates.  
- On in-the-money paths:
  - Regress discounted future cashflows on basis functions of the state.  
  - Estimate the continuation value.  
- Compare continuation value with immediate exercise value and decide whether to exercise.  
- Use the resulting optimal stopping rule to generate cashflows.  
- Discount those optimal cashflows back to time 0.  

This approach makes Monte Carlo pricing of early-exercise options computationally tractable.

## Reference

- Longstaff, F. A., & Schwartz, E. S. (2001). Valuing American options by simulation: a simple least-squares approach. *The Review of Financial Studies*, 14(1), 113–147.

## License

MIT License

---

For questions, feedback, or contributions, please open an issue or submit a pull request.

