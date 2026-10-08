# Labour Market Imbalance in Canada — Summary Report

*Forecasting the vacancy-to-unemployment ratio (theta) with classical, deep-learning, and foundation-model methods. Data: Statistics Canada monthly series, January 1980 – December 2025.*

## 1. Objective

Measure how tight Canada's labour market is, determine which forecasting method predicts it most accurately, and produce 2026 forecasts of theta, unemployment, and vacancies for workforce planning.

Theta = job vacancies / unemployment. High theta means employers compete for scarce workers; low theta means workers compete for scarce jobs.

## 2. Data

552 months of Statistics Canada data (Labour Force Survey + job vacancies), processed into an analysis-ready panel (`1_data/processed/final_dataset_full.csv`): month, employment, unemployment, labour_force, population, vacancies, theta.

## 3. Method

Candidate models — SARIMAX, TBATS+Prophet, BiLSTM with attention, VAR, a dynamic weighted ensemble, and an RF-residual hybrid — were compared against a Chronos pretrained time-series benchmark using rolling-origin backtesting over January 2023 – December 2025 (36 monthly origins). Pairwise Diebold-Mariano tests checked whether accuracy differences are statistically significant.

## 4. Results

### 4.1 Model comparison (test-set errors, Jan 2023 – Dec 2025)

| Target | Best model | RMSE | MAE | MAPE |
|---|---|---|---|---|
| Theta | BiLSTM with attention | 0.112 | 0.074 | 14.0% |
| Unemployment | Chronos (univariate) | 75,470 | 57,281 | 3.7% |
| Vacancies | Dynamic weighted ensemble | 75,778 | 65,604 | 11.7% |

No single model dominates. Nearly all pairwise Diebold-Mariano tests reject equal accuracy at the 5% level, so the ranking reflects real differences rather than noise. Full tables: `4_predictive_forecasting/notebooks/exports/final_comparison/tables/`.

### 4.2 2026 forecast (Jan–Dec 2026)

| | Theta | Unemployment | Vacancies |
|---|---|---|---|
| 2026 average | ~0.265 | ~1.56 million | ~421,000 |
| 80% interval (Jan) | 0.235 – 0.299 | 1.506M – 1.611M | 386K – 455K |

Interpretation: the market stays **loose through 2026** — about one vacancy per 3.8 unemployed workers, far below the 2022 peak near 1.0. The flat profile means no strong directional signal; workforce plans should not assume a re-tightening.

![Theta 1980–2026](../4_predictive_forecasting/notebooks/exports/final_forecasts/figures/theta_labour_imbalance_forecast.png)

## 5. Limitations

National monthly aggregates hide provincial, sectoral, and demographic variation. Forecasts assume no major regime change. Chronos was evaluated on 12 months versus 36 for the other models. The RF-residual hybrid (unemployment RMSE ~48.8K on its own 300-forecast evaluation) was not part of the final aligned panel.

## 6. Reproducibility

All results are exported under `4_predictive_forecasting/notebooks/exports/` (CSVs) and the figures folders. Notebooks are numbered in run order; the comparison and forecast notebooks are `05_backtesting_rolling_origin.ipynb`, `07_chronos_benchmark.ipynb`, `ensemble.ipynb`, and `08_final_forecast_2025.ipynb`.
