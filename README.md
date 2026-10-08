# AI-Driven Labour Market Imbalance Analysis — Canada

**Question:** How tight is Canada's labour market, which forecasting method predicts it best, and what does 2026 look like?

Imbalance is measured with **theta = job vacancies / unemployment**. When theta is high, employers compete for scarce workers; when it is low, workers compete for scarce jobs. This project builds and compares forecasting models for theta, unemployment, and vacancies, then produces 2026 forecasts to support workforce planning.

![Theta history and 2026 forecast](4_predictive_forecasting/notebooks/exports/final_forecasts/figures/theta_labour_imbalance_forecast.png)
*Monthly theta (vacancies/unemployment), 1980–2025, with the 2026 forecast. The post-2021 spike and unwind is the pandemic-era tightness resolving.*

## Key findings

**Best model per target** (rolling-origin backtest, Jan 2023 – Dec 2025, 36 monthly origins; errors are test-set RMSE/MAE/MAPE):

| Target | Winner | RMSE | MAE | MAPE |
|---|---|---|---|---|
| Theta (imbalance ratio) | BiLSTM with attention | 0.112 | 0.074 | 14.0% |
| Unemployment | Chronos (pretrained time-series model) | 75,470 | 57,281 | 3.7% |
| Vacancies | Dynamic weighted ensemble | 75,778 | 65,604 | 11.7% |

No single model wins everywhere: the neural model fits the nonlinear imbalance ratio best, the pretrained Chronos benchmark wins on unemployment, and the ensemble wins on vacancies. Pairwise Diebold-Mariano tests show the accuracy differences are statistically significant at the 5% level (see `4_predictive_forecasting/notebooks/exports/final_comparison/tables/dm_test_results.csv`). Full comparison: `final_metrics_summary.csv` in the same folder.

**2026 forecast** (12-month horizon, Jan–Dec 2026, with 80% and 95% intervals):

| Month | Theta | Unemployment | Vacancies |
|---|---|---|---|
| 2026-01 | 0.267 (0.235–0.299 at 80%) | ~1,558,000 | ~420,500 |
| 2026-06 | 0.265 | ~1,555,000 | ~420,700 |
| 2026-12 | 0.265 | ~1,555,000 | ~420,700 |

The forecast implies a **loose labour market through 2026**: roughly one vacancy for every 3.8 unemployed workers, well below the 2022 peak near 1.0. Forecasts: `4_predictive_forecasting/notebooks/exports/final_forecasts/`.

The repository's selected production models for the 2026 forecasts are BiLSTM (theta) and the dynamic weighted ensemble (unemployment, vacancies).

## Dataset

Monthly Statistics Canada labour market data, **January 1980 – December 2025** (552 months), assembled in `1_data/`:

- `1_data/raw/maindata.csv` — Labour Force Survey characteristics (employment, unemployment, labour force, population)
- `1_data/raw/job_vacancies.csv` — job vacancy levels
- `1_data/processed/final_dataset_full.csv` — analysis-ready panel: month, employment, unemployment, labour_force, population, vacancies, theta
- `1_data/processed/final_dataset_modeling.csv` — modeling subset

## Approach

Each folder is one phase of the analytics lifecycle:

- `0_proposal/` — project proposal and scope
- `1_data/` — raw and processed Statistics Canada datasets
- `2_data_preparation/` — cleaning, alignment, and feature preparation
- `3_descriptive_diagnostic/` — imbalance trends, Beveridge curve, volatility analysis (figures in `3_descriptive_diagnostic/figures/phase1_descriptive/`)
- `4_predictive_forecasting/` — candidate models (SARIMAX, TBATS+Prophet, BiLSTM-attention, VAR, ensemble, RF-residual hybrid), Chronos benchmark, rolling-origin backtesting, Diebold-Mariano tests, and the 2026 forecast generation
- `5_scenario_analysis/` — scenario evaluation for workforce planning
- `6_visualization/` — charts and visual outputs
- `7_results_exports/` — exported results and tables
- `8_report/` — [summary report](8_report/labour_market_imbalance_report.md) with findings, tables, and charts

## Limitations

- Forecasts are national-level monthly aggregates; they do not capture provincial, sectoral, or demographic differences.
- The 2026 forecast assumes no major policy or economic regime change; prediction intervals widen if conditions shift.
- Chronos was evaluated on a 12-month window (2025) versus 36 months for the other models, so its win on unemployment comes with a shorter track record.
- The RF-residual hybrid was evaluated separately (300 forecasts, unemployment RMSE ~48.8K, MAPE 2.8%) and was not part of the final aligned comparison panel.

## How to run

1. Clone the repository.
2. Install the Python dependencies (a GPU is recommended for the deep-learning notebooks):
   `pandas, numpy, matplotlib, seaborn, scikit-learn, statsmodels, prophet, tbats, ruptures, torch, tensorflow, chronos, jupyter`
3. Open the notebooks in phase order (starting with `2_data_preparation/`) and run them top to bottom.
4. The forecasting notebooks in `4_predictive_forecasting/notebooks/` reproduce the model comparison; `08_final_forecast_2025.ipynb` generates the 2026 forecasts. Exported results are already in `4_predictive_forecasting/notebooks/exports/` if you want the findings without re-running.

## My contribution

Solo project: data assembly from Statistics Canada sources, the full modeling pipeline (classical, deep learning, and foundation-model benchmarks), the rolling-origin evaluation framework with Diebold-Mariano testing, and the 2026 forecast production.
