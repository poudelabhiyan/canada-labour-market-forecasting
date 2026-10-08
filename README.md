# AI-Driven Labour Market Imbalance Analysis — Canada

Forecasting model that measures and predicts labour market imbalance across Canada using Statistics Canada data (2015-2024). Imbalance is measured with the vacancy-to-unemployment ratio and analyzed through descriptive, diagnostic, predictive, and scenario-based methods to support evidence-based workforce planning.

## Data

Statistics Canada labour market data covering 2015-2024: job vacancies, unemployment, and related indicators. The data is cleaned and prepared in the early phases of the pipeline before analysis.

## Approach

The project follows a structured analytics lifecycle, with each folder representing one phase:

- `0_proposal/` - project proposal and scope
- `1_data/` - raw Statistics Canada datasets
- `2_data_preparation/` - data cleaning and feature preparation
- `3_descriptive_diagnostic/` - descriptive and diagnostic analysis of imbalance trends
- `4_predictive_forecasting/` - forecasting models with diagnostics, a Chronos time-series benchmark, Diebold-Mariano tests, and 2026 forecast generation
- `5_scenario_analysis/` - scenario evaluation for workforce planning
- `6_visualization/` - charts and visual outputs
- `7_results_exports/` - exported results and tables
- `8_report/` - final report

## Key findings

The forecasting pipeline compares candidate models against a Chronos time-series benchmark, using Diebold-Mariano tests to check whether differences in forecast accuracy are statistically significant. It produces 2026 forecasts of the vacancy-to-unemployment ratio to support forward-looking workforce planning. See the notebooks in `4_predictive_forecasting/` and the report in `8_report/` for the full results.

## How to run

1. Clone the repository.
2. Install the Python dependencies: pandas, numpy, matplotlib, scikit-learn, jupyter.
3. Open the notebooks in phase order (starting with `2_data_preparation/`) and run them top to bottom. The forecasting notebooks in `4_predictive_forecasting/` generate the 2026 forecasts.
