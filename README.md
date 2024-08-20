# Hydrological-Forecast-Time-Series

After importing and plotting the data we see that this is the time series that we are working with. 
![image](https://github.com/user-attachments/assets/510c8778-5569-4b8e-83be-c64b4be9f814)

I will use the KPSS test to check the time series for stationarity. I get the following output.
![image](https://github.com/user-attachments/assets/0748115e-63f3-477e-b334-e45bbf7da99d)

The data appears to be non-stationary as the p-value from the KPSS test is very small. We will now see the ACF and PACF from the data. We see the results from the ACF and PACF graphs should help us understand the orders of the AR(p) or MA(q) model. From the output, we see that both the ACF and the PACF have a geometric decomposition pattern. So we have an ARIMA model that should work.
![image](https://github.com/user-attachments/assets/15ee967a-b431-4d2c-a5b7-0b51a4d02c7f)

We will run an auto.arima function to get the best ARIMA(p, d, q) model.  
![image](https://github.com/user-attachments/assets/3c99b1cc-ab4d-4742-9655-f358cd96f5a0)


The ARIMA(5, 1, 1) has the following Information Criteria diagnostics. 
<img width="385" alt="image" src="https://github.com/user-attachments/assets/82256c6c-267b-437e-aba6-bd9c28ae801f">

It has the following diagnostics:
<img width="441" alt="image" src="https://github.com/user-attachments/assets/c466641e-b573-4171-b4a1-e402385b8de8">

After seeing the residuals, I am not sure if there is seasonality or not. We see from the Box-Ljung statistics suggest that I can improve this model. So I will try to fit a SARIMA model and then use both models as well as ETS for the best results and then use cross-validation to find the best models.

After multiple tries of testing a SARIMA model, we see that the residuals of a SARIMA(5, 1, 1, 0, 1, 1) with 12 lags of seasonality. We see that the BLP test results in much better p-values. We also see that the ACF plot is much better.
<img width="461" alt="image" src="https://github.com/user-attachments/assets/d44df1aa-69fd-4ce1-95c0-d9d4449beb6f">

The Information Criteria of the SARIMA(5, 1, 1, 0, 1, 1) with 12 lags of seasonality:

<img width="283" alt="image" src="https://github.com/user-attachments/assets/5d30cb29-ec37-49f9-9397-3a97d07bd278">

We now fit an ETS model for reference. We see that an ETS(A, Ad, N) is the best fit with the following diagnostics: 

![image](https://github.com/user-attachments/assets/87d6ef27-1dff-409c-86eb-1aa05fe53903)

This results in the following decomposition:
We now use Cross-validation to get the following mean squared error from	
SARIMA(5, 1, 1, 0, 1, 1) - 43.821
ETS(A, Ad, N) - 155.1527
ARIMA(5, 1, 1) - 51.92954
After CV we see that the SARIMA(5,1,1,0,1,1)12 model is the best. Let’s setup a forecast for the next 24 months.
![image](https://github.com/user-attachments/assets/413eeae2-64ae-4ec2-bd0e-909a6a4f0e06)
