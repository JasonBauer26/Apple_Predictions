# Apple_Predictions
In this project I will be attempting to predict Apple closing prices using Time Series Analysis techniques.
Since this project was done in Google Colab, the dataset can be loaded in as a package from Yahoo Finance.

The first thing I want to do after loading in the data set is to plot the Autocorrelation Function (ACF) and the Partial Autocorrelation Function (PACF) to check the data for seasonality.

![Apple_pred_ACFPAFC](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/81f35b2b-e56e-4e92-939e-9f421e5eaefe)

These plots do not suggest any sort of seasonality, but in general stock prices are seasonal, so I will move on to the check for stationarity with that in mind. I will be using the Augmented Dickey-Fuller test to check for stationarity.

![AppleStationarity](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/e8f44c33-a115-4026-8e62-4a8338b4bb72)

Since the p-value of 0.216 > 0.05, it is safe to say that the data is not stationary. Differencing is a technique used in time series analysis and forecasting to transform a non-stationary time series into a stationary one. I will use this technique to make the data stationary.

![Apple_diff_plot](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/55e7a8b5-d3c9-4321-85c9-e8bb8a08e4b8)

Now that I have performed the differencing technique, I will once again use the Augmented Dickey-Fuller test to check for stationarity. 

![AppleStationarity_diff](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/756fa39e-c699-4b76-8edb-c29203495885)

A p-value of 1.04e-22 is well below my threshold of 0.05, so it is safe to proceed.

My next step is to plot a new ACF and a new PACF plot to account for the differencing. 

![Apple_diff_ACFPACF](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/bd1e3218-f36d-43f0-ad1f-472f0c1c9056)

These plots did not offer me any obvious components for my model. I decided to perform a stepwise search to minimize AIC to find my best model. 

![image](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/97bfcfcf-6b7d-4cec-9450-56d70157ef90)

![image](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/7be699f2-e4ad-4d45-8608-2d7cb94486fe)

The stepwise search selected an ARIMA model ARIMA(10,1,0)(0,0,0) as the best model. I believe the AIC could have been even lower if I raised the max_p parameter beyond 10, but due to computational constraints, I believe max_p = 10 is the best tuning.

Once I had the model in place, it was time to split the dataset into train and test. I selected the range '2021-01-01':'2023-11-30' as the training set, and selected '2023-12-01': as the test set. I thought one month of predictions would be sufficient for assessing accuracy of predictions.

![Untitled](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/cd396f5e-d94d-49b1-b432-b94b44c22939)

Above is the plot of forecasted stock prices. It is important to note that the values forecasted are not the actual stock prices. Rather, they are predictions for the difference in closing price between consecutive days. After I evaluate the model, I will build in a section that uses the 'last known closing price' and adds it to the 'predicted close price' in order to obtain the actual closing price.

To evaluate how the model had performed, I calculated the Mean Absolute Error(MAE), the Mean Squared Error(MSE), and the Root Mean Squared Error (RMSE). I think an MAE of 1.429 is troubling for predictions in the short term, but it is not necessarily unreliable for long term predictions.

![image](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/ecf90e5e-b655-44ce-a86f-90ff3b477323)

I developed a tool allowing users to input a date for predicting the closing price. Additionally, if the user inputs a date within the test set, the tool will display both the predicted and actual closing prices.

![image](https://github.com/JasonBauer26/Apple_Predictions/assets/145518855/e1b1ab57-b398-4fdf-ad07-dcde94e16135)

My forecast for December 7, 2023, deviated by approximately $2 from the actual closing price. While these predictions may not be ideal for day trading due to their slight variance, they offer valuable insights into potential trends in future stock prices.
