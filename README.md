# Macro Arbitrage Foreign Exchange Strategy

### Author: Jocelyn Guan, 5/20/2025
This project implements a macro-driven foreign exchange trading strategy that combines **Taylor Rule Gap** analysis with **Covered Interest Parity (CIP) Deviation** signals to generate long/short positions in major currency pairs.


## Strategy Overview

The strategy is based on two key macroeconomic signals:

- **Taylor Rule Gap**: Measures the deviation between a central bank’s policy rate and the interest rate implied by the Taylor Rule. It captures whether a currency is under tight or loose monetary policy relative to fundamental
- **CIP Deviation**: Captures the arbitrage gap between interest rate differentials and forward FX pricing. A persistent deviation may reflect capital controls, credit risk, or market inefficiencies


## Model Assumptions

- Model assume FX market is rational and efficient
- Model is based on monthly macroeconomics data from 1980Q1 – 2024Q4
- Model are based in US dollar: USD is the base currency in all comparisons that are against USD 
- Model assumes consistency in monetary policy response across the sample
- Model assume no capital controls and only monetary policy effect
- Model ignore non-monetary derivers of currency movement
- Model values equal importance in both Taylor Rule Gap and CIP deviation in trade decision


## Trading Logic

Capture only when two indicators (Taylor Gap & CIP derivation) signal the same direction and extreme enough

- **Taylor Rule Gap: directional filter**: Positive (short) / Negative (long)
- **CIP deviation: magnitude filter**: Trade when |CIP deviation| > 2.5 * rolling std


Trade when both indicators suggest mispricing:
- **Long** when Taylor Rule Gap < 0 and CIP deviation < +2.5σ → underpricing of appreciation
- **Short** when Taylor Rule Gap > 0 and CIP deviation > +2.5σ → overpricing of depreciation

Exit: Stop loss limit & Trailing z-score reversal


## Backtest Results

| USD/JPY             | Value         |
|---------------------|---------------|
| Total Return        | 18.09%        |
| Sharpe Ratio        | 0.27          |
| Max Drawdown        | -18.01%       |
| Win Rate            | 47.37%        |
| Number of Trades    | 95            |

| USD/GBD             | Value         |
|---------------------|---------------|
| Total Return        | 5.02%         |
| Sharpe Ratio        | 0.71          |
| Max Drawdown        | -6.24%        |
| Win Rate            | 66.67%        |
| Number of Trades    | 27            |


## Project Structure
```
├── data  
│   ├── japan_cpi.csv          
│   ├── japan_pgdp.csv    
│   ├── japan_rates.csv          
│   ├── uk_pgdp.csv    
│   └── uk_rates.csv
├── fx_strategy.ipynb
└── README.md
```

## Limitations

**Model Limitations**
- International data is very hard to get/ not accuracy
- Taylor Rule may not capture actual policy behavior
- Lag in macroeconomics data like inflation, outputs and rates are reported with delays
- CIP deviation can also reflect capital controls, not necessarily arbitrage

**Performance Inconsistency**
- Low Sharpe ratio
- Limited trade opportunities – few trade timing that met both Taylor Gap and CIP deviation
- Completely different performance between different currency pairs

**Success Case – Japan**
- Successfully to forecasts stable monetary policy 
- Japan had a unique monetary policy regime for decades
- Near-zero or negative rates since the 1990s
- These extreme policies create predictable Taylor Rule deviations and frequent CIP distortions


## Future Improvements

**Model Enhancements**
- Analyze effects of different monetary policy
- Test alternative Taylor Rule specifications
- Add macro trend indicators in Taylor Gap
- Incorporate real-time macro estimates

**Risk Management**
- Develop additional exit strategies
- Add filter like VIX and other risk factors
- Optimize position sizing algorithms

**Cross-Validation**
- Expand to additional currency pairs
- Test JPY/GBP cross-validation
- Compare US vs Japan Taylor Gap


## References
- FRED, BOJ, and BOE databases
- www.sciencedirect.com/science/article/pii/S0022199621000246 
- www.nber.org/system/files/working_papers/w23170/w23170.pdf
- www.sciencedirect.com/topics/economics-econometrics-and-finance/taylor-rule