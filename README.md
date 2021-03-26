**Acea Smart Water Analysis (Petrignano)**

**Overview**

The [Acea Group](https://www.gruppo.acea.it/en) is one of the leading Italian multiutility operators. Listed on the Italian Stock Exchange since 1999, it is foremost Italian operator in the water services sector supplying 9 million inhabitants in Lazio, Tuscany, Umbria, Molise, Campania.

The Acea Group deals with four different type of waterbodies:

1. water springs
1. lakes
1. rivers
1. aquifers.

As it is easy to imagine, a water supply company struggles with the need to forecast the water level in a waterbody (aquifer) to handle yearly consumption.

This project focusing on Forecast the depth to groundwater of an aquifer located in Petrignano, Italy. Also forecast the underground(aquifers) water level, for each year. 

Depth of underground water vary from -18 to -34 so maximum depth of groundwater is -34.


**Data**

Although the dataset contains multiple water bodies, we will only be looking at the Aquifer\_Petrignano.csv file.

[https://www.kaggle.com/c/acea-water-prediction/data/ ](https://www.kaggle.com/c/acea-water-prediction/data/%20)

CSV file: - Aquifer\_Petrignano.csv

**Method**

This is a time series problem, so our first task is to check given data is stationary or not if data is not stationary then we must make it stationary to conduct time series analysis. Time series forecasting is a technique for the prediction of events through a sequence of time. The techniques predict future events by analyzing the trends of the past, on the assumption that future trends will hold like historical trends.

I choose to work with. ARIMA and Facebook prophet to analyze future prediction of underground water label. In the future, I would love to use seasonal ARIMA and LSTM neural network on top of this.

**Data Cleaning**

Data cleaning Report

After reviewing the dataset get to know there are 1024 missing values in most columns, so I drop first 1024 rows to make meaningful data. We have two target feature Depth\_to\_Groundwater\_P24 and Depth\_to\_Groundwater\_P25. To make univariate prediction drop one of target variable Depth\_to\_Groundwater\_P24 so we can focus on one target.

We can see that our target variable (Depth\_to\_Groundwater) has missing values. We will have to clean them by replacing them by nan values and filling them afterwards. After doing statistical operation, the best option in this case, is interpolate.


**EDA**

![](Aspose.Words.22897037-2ef4-4861-964b-481202f26803.001.png)

After reviewing the trend, we find there is no such trend sometime water level goes up and sometime it's goes down. There is some seasonal pattern water goes down in month of august, September and October

**Seasonal Shift** 


![](Aspose.Words.22897037-2ef4-4861-964b-481202f26803.002.png)

###
###
**Checking Stationarity of a Time Series**

Stationarity is requirement of most of the time series Model

There are three basic criteria for a time series to understand whether it is stationary series or not.

1. constant mean and mean are not time dependent.
1. constant variance and variance are not time dependent.
1. constant covariance and covariance are not time dependent.

Statistical properties of time series such as mean, variance & covariance should remain constant over time, to call **time series is stationary.**



![](Aspose.Words.22897037-2ef4-4861-964b-481202f26803.003.png)

After review rolling mean and Rolling std of 365 days window we do not have constant standard deviation and constant mean. I have conducted some test to check same.

### **Augmented Dickey-Fuller (ADF)**
Augmented Dickey-Fuller (ADF) test is a type of statistical test called a unit root test. Unit roots are a cause for non-stationarity.

**Null Hypothesis (H0):** Time series has a unit root. (Time series is not stationary).

**Alternate Hypothesis (H1):** Time series has no unit root (Time series is stationary).

**If the null hypothesis can be rejected, we can conclude that the time series is stationary.**

There are two ways to rejects the null hypothesis:

On the one hand, the null hypothesis can be rejected if the p-value is below a set significance level. The defaults significance level is 5%

**p-value > significance level (default: 0.05)**: Fail to reject the null hypothesis (H0), the data has a unit root and is non-stationary. **p-value <= significance level (default: 0.05)**: Reject the null hypothesis (H0), the data does not have a unit root and is stationary. On the other hand, the null hypothesis can be rejects if the test statistic is less than the critical value.

**ADF statistic > critical value**: Fail to reject the null hypothesis (H0), the data has a unit root and is non-stationary. **ADF statistic < critical value**: Reject the null hypothesis (H0), the data does not have a unit root and is stationary.

**ADF statistic** (-2.899836995568036,

` `**p-value** 0.04536695595343471,

` `28,

` `4170,

` `**critical value**

{'1%': -3.4319191438819407,

`  `'5%': -2.8622333615468443,

`  `'10%': -2.567139082403142},

` `-11587.395288114172)

After reviewing the ADF result we say data is nonstationary.

**Preprocessing**

**Making Stationary** 

The two most common methods to achieve stationarity are:

1. Transformation: e.g., log or square root to stabilize non-constant variance
1. Differencing: subtracts the current value from the previous

After Differencing when we check ADF statistic and p-value I find data become stationary because it fulfills the significant criteria

# **Time Series Forecasting Algorithm**
I choose to work on ARIMA and Facebook Prophet

For test data I use 365 days data and remaining I use for testing data. For ARIMA I take order (1,1,1)

I did forecast for next 1 year (365 days) from 2020/06/30 to 2021/06/30.

We will evaluate the Mean Absolute Error (MAE) and the Root Mean Square Error (RMSE) of the models. For metrics are better the smaller they are.

**WINNER: Facebook Prophet**

**ARIMA Residual**

![](Aspose.Words.22897037-2ef4-4861-964b-481202f26803.003.png)

Forecast

![](Aspose.Words.22897037-2ef4-4861-964b-481202f26803.004.png)

Forecast 365 days ahead data and get to know forecasted water label is -26 to -26.2 and it did not exceed the confidence interval i.e.  -25.5 to -30. According to result water label varies between these points. Solid blue line represents the predicted value. forecasted period is from 2020/06/30 to 2021/06/30.

**Facebook Prophet**


**Forecast**

![](Aspose.Words.22897037-2ef4-4861-964b-481202f26803.005.png)

Forecast 365 days ahead data and get to know forecasted water label is -25 to -26.4 and it did not exceed the confidence interval i.e.  -19 to -30. According to result water label varies between these points. Solid blue line represents the predicted value. forecasted period is from 2020/06/30 to 2021/06/30.

**Model Evaluation Result**

![](Aspose.Words.22897037-2ef4-4861-964b-481202f26803.006.png)




**Challenge**

There are some improvements to be made, for example the model failed on some data sets due to lack of usable data. I could fix this by looking at imputation methods, as the dataset provided in the ACEA Smart Water Analytics Competition contains a lot of missing data points.













