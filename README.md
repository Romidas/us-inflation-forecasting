# Forecasting U.S. Inflation Under Structural Change

A time-series forecasting project evaluating statistical, machine learning and pretrained foundation models for U.S. year-on-year CPI inflation.

## 1. Project Overview

This project investigates the forecasting performance of different modelling approaches for U.S. CPI inflation, with particular attention to changing economic conditions across pre-COVID, COVID and post-COVID periods.

The study compares conventional statistical methods, machine learning and a pretrained time-series foundation model against a Random Walk benchmark.

## 2. Research Objectives

- Develop and evaluate models for forecasting U.S. year-on-year CPI inflation.
- Compare statistical, machine learning and foundation-model approaches.
- Assess forecast performance across different economic regimes.
- Evaluate forecasts at multiple horizons.
- Examine prediction-interval coverage and statistical differences in forecast accuracy.

## 3. Data Sources

Monthly macroeconomic data were obtained from [Federal Reserve Economic Data (FRED)](https://fred.stlouisfed.org/).

| Series | Description |
|---|---|
| CPIAUCSL | Consumer Price Index for All Urban Consumers |
| INDPRO | Industrial Production Index |
| UNRATE | Civilian Unemployment Rate |
| FEDFUNDS | Effective Federal Funds Rate |

The target variable is year-on-year CPI inflation, calculated from seasonally adjusted CPI.

## 4. Models Evaluated

- Random Walk – primary benchmark
- Seasonal Naïve – secondary benchmark
- SARIMAX – statistical time-series model
- LightGBM – gradient boosting model
- Chronos-2 – pretrained time-series foundation model

## 5. Methodology

The analysis includes:

1. Monthly data alignment and preprocessing.
2. Missing-observation assessment and handling.
3. Exploratory time-series analysis.
4. Stationarity and autocorrelation diagnostics.
5. Training-only SARIMAX candidate selection.
6. Expanding-window annual refitting for SARIMAX and LightGBM.
7. Rolling-origin one-step-ahead forecast evaluation.
8. Multi-horizon evaluation at 1, 3, 6 and 12 months.
9. Diebold–Mariano forecast comparison tests with HAC variance estimation and Holm adjustment.
10. Evaluation of Chronos-2 prediction intervals.

## 6. Key Results

### Overall one-step-ahead performance

All models were evaluated on the same 104 valid observations.

| Model | RMSE | MAE |
|---|---:|---:|
| Random Walk | 0.4168 | 0.3052 |
| Chronos-2 | 0.4188 | 0.2885 |
| SARIMAX | 0.4209 | 0.2877 |
| LightGBM | 1.5225 | 0.9258 |
| Seasonal Naïve | 2.4963 | 1.7713 |

Random Walk achieved the lowest RMSE, while SARIMAX achieved the lowest MAE. The Diebold–Mariano comparisons did not identify statistically significant differences among the leading models after Holm adjustment.

### Multi-horizon evaluation

Chronos-2 was compared against Random Walk at 1-, 3-, 6- and 12-month horizons. Random Walk achieved lower RMSE at each evaluated horizon.

Chronos-2's empirical 80% prediction-interval coverage was below the nominal level across the evaluated horizons.

### Regime-wise evaluation

Forecast performance varied across the pre-COVID, COVID and post-COVID evaluation periods. SARIMAX performed best by RMSE in the pre-COVID and post-COVID periods, while Chronos-2 performed best during the COVID period.

## 7. Final Forecast

The final Chronos-2 forecast covers October 2026 to September 2027.

The notebook includes:
- Monthly point forecasts
- Nominal 80% prediction intervals
- Random Walk reference path

The complete forecast is available in `results/US_CPI_Inflation_Forecast_Oct2026_Sep2027_Final.csv`.

## 8. Repository Structure

```text
us-inflation-forecasting/
│
├── data/
│   └── raw/
│       ├── CPIAUCSL.csv
│       ├── INDPRO.csv
│       ├── UNRATE.csv
│       └── FEDFUNDS.csv
│
├── notebooks/
│   └── US_Inflation_Forecasting_Final.ipynb
│
├── reports/
│   └── US_Inflation_Forecasting_Report.docx
│
├── results/
│   ├── US_Inflation_OneStep_Backtest_Final.csv
│   ├── US_Inflation_Overall_Metrics_Final.csv
│   ├── US_Inflation_Regime_Metrics_Final.csv
│   ├── US_Inflation_MultiHorizon_Backtest_Final.csv
│   ├── US_Inflation_MultiHorizon_Metrics_Final.csv
│   └── US_CPI_Inflation_Forecast_Oct2026_Sep2027_Final.csv
│
└── figures/
    ├── 01_exploratory_cpi_and_macro_predictors.png
    ├── 02_exploratory_correlation_matrix.png
    ├── 03_sarimax_training_acf_pacf.png
    ├── 04_sarimax_residual_diagnostics.png
    ├── 05_rolling_one_step_forecasts.png
    ├── 06_multi_horizon_accuracy.png
    └── 07_final_12_month_cpi_forecast.png
```

## 9. Reproducibility

To reproduce the analysis:

1. Clone or download this repository.
2. Install the required Python dependencies.
3. Open the notebook in Jupyter.
4. Run all cells from a fresh kernel.

Chronos-2 requires internet access to download its pretrained model weights.

## 10. Limitations

- Regime boundaries are descriptive calendar periods; no formal structural-break test was used to estimate break dates.
- SARIMAX and LightGBM are re-estimated annually, while Chronos-2 operates in zero-shot mode with expanding historical context.
- Chronos-2 pretraining-data overlap with publicly available macroeconomic data cannot be ruled out.
- Residual diagnostics indicate remaining autocorrelation and departures from normality and constant variance in the initial SARIMAX fit.

## 11. References

- Atkeson, A., & Ohanian, L. E. (2001). Are Phillips curves useful for forecasting inflation? *Federal Reserve Bank of Minneapolis Quarterly Review*.
- Stock, J. H., & Watson, M. W. (2007). Why has U.S. inflation become harder to forecast? *Journal of Money, Credit and Banking*.
- Diebold, F. X., & Mariano, R. S. (1995). Comparing predictive accuracy. *Journal of Business & Economic Statistics*.
- Ansari, A. F., et al. (2025). Chronos-2: From univariate to universal forecasting.
- Hyndman, R. J., & Athanasopoulos, G. *Forecasting: Principles and Practice*.

**Data source:** [Federal Reserve Economic Data (FRED)](https://fred.stlouisfed.org/)
