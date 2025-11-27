# HKUTG (Quant Division): Bermudan Option Pricing

This project demonstrates how to price Bermudan (discretely exercisable American) options using the Longstaff–Schwartz Least-Squares Monte Carlo (LSMC) method in Python. It includes implementations of various regression techniques for estimating continuation values, and provides a framework for comparing their effectiveness.

## Features

- Monte Carlo simulation of Black-Scholes stock price paths
- Bermudan/American option pricing using the Longstaff–Schwartz algorithm
- Multiple regression methods for continuation value estimation:
  - Black-Scholes basis
  - Piecewise-linear regression
  - Kernel (Nadaraya–Watson) regression
- Out-of-sample lower bound estimation
- Confidence interval calculation for price estimates
- Data analysis and visualization tools

## Project Structure

```
.
├── [1] LinearRegression.ipynb
├── [2] ConditionalExpectation.ipynb
├── [3] Bermudan.ipynb
├── bermudan-pricer.ipynb
├── stock.csv
└── .github/
    └── instructions/
        ├── copilot-instructions.instructions.md
        └── copilot.instructions.md
```

- `[1] LinearRegression.ipynb`: Linear regression and data exploration
- `[2] ConditionalExpectation.ipynb`: Conditional expectation and regression methods
- `[3] Bermudan.ipynb`: Main notebook for Bermudan option pricing and LSMC
- `bermudan-pricer.ipynb`: Standalone/practical implementation of the pricer
- `stock.csv`: Historical stock data for analysis

## Quick Start

1. **Clone the repository**  
   ```sh
   git clone <your-repo-url>
   cd quant-training
   ```

2. **Install dependencies**  
   This project uses Python 3.8+ and the following packages:
   - numpy
   - pandas
   - matplotlib
   - scipy
   - scikit-learn

   Install with:
   ```sh
   pip install numpy pandas matplotlib scipy scikit-learn
   ```

3. **Run the notebooks**  
   Open any notebook (`[3] Bermudan.ipynb` recommended) in Jupyter or VS Code and run the cells.

## Usage

- Modify parameters such as `S0`, `K`, `vol`, `r`, `q`, and `T` in the notebooks to experiment with different option contracts.
- Try different regression methods and compare their pricing accuracy and computational efficiency.
- Use the provided `stock.csv` for real data analysis or plug in your own data.

## Example: Pricing a Bermudan Put

```python
from bermudan_pricer import blackscholes_mc, train_ls_policy, apply_ls_policy

# Set up parameters
ts = np.linspace(0, T, 13)
paths = blackscholes_mc(ts, 10000, S0, vol, r, q)
policy, train_price, mse, ci_low, ci_high = train_ls_policy(ts, paths, K, r, q, vol, method='bs')
test_paths = blackscholes_mc(ts, 100000, S0, vol, r, q)
lower_bound, ci_low_test, ci_high_test = apply_ls_policy(ts, test_paths, K, r, q, policy, vol, method='bs')
```

## References

- Longstaff, F. A., & Schwartz, E. S. (2001). Valuing American options by simulation: a simple least-squares approach. *The Review of Financial Studies*, 14(1), 113-147.

## License

MIT License

---

*For questions or contributions, please open an issue or pull request.*

