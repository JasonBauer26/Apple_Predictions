# Apple_Predictions
In this project I will be attempting to predict Apple closing prices using Time Series Analysis techniques.
Since this project was done in Google Colab, the dataset can be loaded in as a package from Yahoo finance.

The first thing I want to do after loading in the data set is to plot the Autocorrelation Function (ACF) and the Partial Autocorrelation Function (PACF) to check the data for seasonality.

![Apple_pred_ACFPAFC](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/81f35b2b-e56e-4e92-939e-9f421e5eaefe)

These plots do not suggest any sort of seasonality, so I will move on to the check for stationarity. I will be using the Augmented Dickey-Fuller test to check for stationarity.

![AppleStationarity](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/e8f44c33-a115-4026-8e62-4a8338b4bb72)

Since the p-value of 0.216 > 0.05, it is safe to say that the data is not stationary. Differencing is a technique used in time series analysis and forecasting to transform a non-stationary time series into a stationary one. I will use this technique to make the data stationary.

![Apple_diff_plot](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/55e7a8b5-d3c9-4321-85c9-e8bb8a08e4b8)

Now that I have performed the differencing technique, I will once again use the Augmented Dickey-Fuller test to check for stationarity. 

![AppleStationarity_diff](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/756fa39e-c699-4b76-8edb-c29203495885)

A p-value of 1.04e-22 is well below my threshold of 0.05, so it is safe to proceed.

My next step is to plot a new ACF and a new PCAF plot to account for the differencing. 

![Apple_diff_ACFPACF](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/bd1e3218-f36d-43f0-ad1f-472f0c1c9056)

These plots did not offer me any obvious components for my model. I decided to perform a stepwise search to minimize AIC to find my best model. 

![Apple_Stepwise_AIC](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/8162da17-8041-4088-b0df-b3054630206b)

The model I have chose is the ARIMA(0,1,0) model. My next step is to forecast with this model and evaluate the results.

![Apple_predictions_ARIMA](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/56177460-b3ec-47e6-9732-e0c68cb476e4)

Above is the plot of Actual vs Forecasted differences. After December 5th the predictions become unstable and unreliable. This could be potentially explained by volatility in the data. I will attempt to use a GARCH model to account for this potential volatility.

![Apple_GARCH_pred](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/ca5838b2-8b81-4f0d-892e-9160ca2dd3f0)

I selected November 30th for comparison and the GARCH model was about $4 off of the actual closing price for that day. While the GARCH model performed better than the ARIMA model in general, I think this problem could be refined even further with multivariate time series techniques. This will be the focus of my next iteration.
