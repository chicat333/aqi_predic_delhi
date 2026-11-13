# Air Quality Prediction for Delhi

**Author:** Prachi Bhatia, ...

**Course:** MCSC105 – Data Mining, Semester 1 Project

**Submitted to:** Dr. Bharti Rana, Associate Professor, Department of Computer Science, University of Delhi

This project computes the Air Quality Index (AQI) for Delhi from CPCB monitoring data (2010–2023) and forecasts future AQI using an ARIMA time-series model. The full write-up is in [`DM_report copy.pdf`](DM_report%20copy.pdf).

---

## Objectives

1. Inform the Delhi public about overall air quality through a single parameter that is easy to understand.
2. Help compare air quality conditions at different locations within the National Capital.
3. Determine how air quality has changed (degraded or improved) over time, so that it can be forecast and pollution control measures can be planned.
4. Reduce a large environmental dataset to a comprehensible form for scientific research.

## Repository contents

| Path | Description |
|---|---|
| `AQI2025.ipynb` | Main notebook: data loading, cleaning, AQI calculation, stationarity testing, ARIMA modelling, evaluation and forecasting |
| `data/` | Air quality data from multiple CPCB monitoring stations across Delhi (`DL001.csv` – `DL040.csv`) |
| `DM_report copy.pdf` | Project report |

## Methodology

1. **Data acquisition.** Concentrations of PM10, PM2.5, NO₂, O₃, CO, SO₂ and NH₃ for 2010–2023 at multiple stations, obtained from the Central Pollution Control Board (CPCB), Delhi.
2. **Data loading.** The station `.csv` files are concatenated into a single dataset of mean values representing the whole region.
3. **Cleaning and preprocessing.** Redundant and insignificant attributes are removed. Missing values are handled with interpolation and median filling. Hourly data is resampled to monthly means.
4. **AQI calculation.** Sub-indices use the CPCB breakpoints and the linear segmented formula:

   `Ii = [(IHI − ILO) / (BHI − BLO)] × (Cp − BLO) + ILO`

   The overall AQI uses the **maximum operator approach** (Sharma et al.): AQI is the highest sub-index among all pollutants.
5. **Stationarity testing.** The Augmented Dickey-Fuller (ADF) test showed the series was non-stationary, so first-order differencing was applied.
6. **Model fitting.** An ARIMA(p, d, q) model was fitted, with orders selected by the Akaike Information Criterion (AIC). The selected model was **ARIMA(2,1,2)**.
7. **Forecasting.** The data is split 80:20 into training and test sets. Predictions on the test set are evaluated, and AQI is then forecast for April 2023 – April 2025.

## Results

| Performance measure | Value |
|---|---|
| Mean Absolute Error (MAE) | 99.98 |
| Mean Squared Error (MSE) | 13,331.33 |
| Mean Absolute Percentage Error (MAPE) | 48.57% |
| Mean Absolute Scaled Error (MASE) | 1.02 |
| Normality test p-value (residuals) | 0.2384 |
| Autocorrelation of residuals (Ljung-Box) p-value | > 0.05 (uncorrelated) |

**What the results show:**

- The residuals are uncorrelated and likely normally distributed, so the model's assumptions are satisfied.
- A MASE of 1.02 means the model performs comparably to a naive forecast.
- A MAPE of 48.57% shows that predictions can be inaccurate in some periods.

The future-scope work below is aimed at these limitations.

## Future scope

1. **GARCH-ARIMA integration:** model heteroscedasticity, meaning error variance that changes over time, in the residuals.
2. **Cyclicity:** capture seasonal patterns in AQI with seasonal decomposition or **SARIMA**.
3. **Exogenous variables:** add wind speed, temperature, humidity and gusts through **ARIMAX**.

## How to run

**Requirements:** Python 3.10+, `pandas`, `numpy`, `matplotlib`, `statsmodels`, `pmdarima`, `scipy`

```bash
git clone https://github.com/chicat333/aqi_predic_delhi.git
cd aqi_predic_delhi
pip install pandas numpy matplotlib statsmodels pmdarima scipy
jupyter notebook AQI2025.ipynb
```

**Run in Google Colab, no installation needed:**
[Open AQI2025.ipynb in Colab](https://colab.research.google.com/github/chicat333/aqi_predic_delhi/blob/main/AQI2025.ipynb)

## References

- Central Pollution Control Board. *National Air Quality Index*. https://cpcb.nic.in
- Central Pollution Control Board. *AQI Repository*. https://airquality.cpcb.gov.in/ccr/#/caaqm-dashboard-all/caaqm-landing/aqi-repository
- Brockwell, P. J., & Davis, R. A. (2008). *Introduction to Time Series and Forecasting* (2nd ed.). Springer.
