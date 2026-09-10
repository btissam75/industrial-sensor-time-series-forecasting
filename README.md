# Industrial Sensor Forecasting with SARIMAX

Time-series forecasting of industrial sensor measurements from a UR3 collaborative robot. This project evaluates whether statistical forecasting can capture the short-term dynamics of robot currents, temperatures, and joint speeds, with a focus on `Tool_current`.

## Project overview

Industrial robots continuously generate sensor data that can support condition monitoring, operational planning, and predictive maintenance. This project develops and evaluates a SARIMAX forecasting workflow using the multivariate [UR3 CobotOps dataset](https://archive.ics.uci.edu/dataset/963/ur3%2Bcobotops) from the UCI Machine Learning Repository.

The analysis covers:

- data cleaning and hourly resampling;
- exploratory time-series analysis and seasonal decomposition;
- stationarity testing with the Augmented Dickey-Fuller test;
- parameter selection using ACF and PACF diagnostics;
- SARIMAX modelling with a 24-hour seasonal period;
- temporal train/test evaluation and `TimeSeriesSplit` cross-validation;
- integration of temperature and speed as exogenous variables;
- comparison with ARIMA, Prophet, and LSTM approaches.

## Dataset

The UR3 CobotOps dataset contains 7,409 multivariate observations from a UR3 collaborative robot. It includes electrical current, temperature, and speed measurements for joints J0-J5, along with tool current and operational indicators.

The source dataset is licensed under CC BY 4.0 and is not duplicated in this repository. Download it from the [official UCI dataset page](https://archive.ics.uci.edu/dataset/963/ur3%2Bcobotops).

## Methodology

1. Inspect and clean missing sensor measurements.
2. Resample measurements to an hourly frequency.
3. Explore trend and daily seasonality.
4. Test stationarity with ADF and inspect ACF/PACF plots.
5. Fit `SARIMAX(1,1,1)(1,1,1,24)` on the training period.
6. Evaluate forecasts on the final 20% of observations.
7. Validate stability with five chronological cross-validation folds.
8. Add temperature and speed signals as exogenous regressors.

## Results

The final exogenous SARIMAX experiment reported:

| Metric | Result |
| --- | ---: |
| RMSE | 0.1264 |
| MAE | 0.1021 |
| MAPE | 277.85% |
| SMAPE | 175.19% |

The five temporal cross-validation folds produced RMSE values between `0.0761` and `0.1477`.

Percentage errors are very high because several target values are close to zero. In this setting, RMSE and MAE are more informative than MAPE. The results should be treated as an exploratory proof of concept rather than production-ready predictive-maintenance performance.

## Key findings

- SARIMAX captures the general temporal pattern but misses some abrupt sensor fluctuations.
- Stable temperature signals are easier to forecast than noisy speed or current measurements.
- Exogenous temperature and speed variables improve the forecasting setup.
- A non-seasonal ARIMA model is too rigid for the observed hourly pattern.
- The short observation window limits confidence in the assumed 24-hour seasonality.

## Limitations

- The analysed period covers approximately six days, providing only a few complete daily cycles.
- Missing values and measurements close to zero complicate model evaluation.
- The seasonal structure requires validation on a longer time range.
- Additional backtesting and operational failure labels are needed before deployment.

## Repository contents

```text
.
├── data/
│   └── README.md                              # Dataset access and citation
├── notebooks/
│   └── sarimax-industrial-forecasting.ipynb  # Analysis and modelling workflow
├── reports/
│   └── sarimax-project-report.pdf             # Detailed academic report
├── .gitignore
├── README.md                                  # Project overview
└── requirements.txt                           # Python dependencies
```

## Run the notebook

The notebook was developed in Google Colab and currently loads the dataset through its upload interface.

1. Download the UR3 CobotOps dataset from UCI.
2. Open `notebooks/sarimax-industrial-forecasting.ipynb` in Google Colab or Jupyter Notebook.
3. Install the required libraries.
4. Run the notebook and upload the dataset when prompted.

Main dependencies include `pandas`, `numpy`, `matplotlib`, `seaborn`, `statsmodels`, `scikit-learn`, `prophet`, and `tensorflow`/`keras`.

## Skills demonstrated

Python · pandas · Statistical modelling · Time-series analysis · SARIMAX · Feature engineering · Temporal cross-validation · Forecast evaluation · Data visualization

## Authors

- Btissam Arehal
- Aya Belhadji
- Wijdane Hrour

Academic project completed for the Master's program in Data Science at École Normale Supérieure de Tétouan, Abdelmalek Essaâdi University (2025-2026).

## Contact

Btissam Arehal — [arehalbtissam@email.com](mailto:arehalbtissam@email.com)

## Data citation

Tyrovolas, M., Aliev, K., Antonelli, D., & Stylios, C. (2024). *UR3 CobotOps* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5J891
