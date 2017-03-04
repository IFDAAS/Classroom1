# Classroom1
R For Quantitative Finance
install.packages("forecast")
install.packages("zoo")
library(zoo)
library(forecast)
hp <- read.zoo("C:/Directory/UKHP.csv", sep = ",",header = TRUE, format = "%Y-%m", FUN = as.yearmon)
## twelve subperiods (called months) in a period (called year)##
frequency(hp)
## use simple returns for our analysis, (1.sy) ##
hp_ret <- diff(hp) / lag(hp, k = -1) * 100
hp_ret
## (1.m)Model identification and estimation: we use the (2.sy)auto.arima function provided by the forecast package to identify the optimal model and estimate the coefficients in one step. The function takes several arguments besides the return series (hp_ret). By specifying stationary = TRUE, we restrict the search to stationary models. In a similar vein, seasonal = FALSE restricts the search to non-seasonal models. Furthermore, we select the (1.t)Akaike information criteria as the measure of relative quality to be used in model selection##
mod <- auto.arima(hp_ret, stationary = TRUE, seasonal = FALSE,ic="aic")
mod

## If the model contains coefficients that are insignificant, we can estimate the model a new using the arima function with the fixed argument, which takes as input a vector of elements 0 and NA. NA indicates that the respective coefficient shall be estimated and 0 indicates that the respective coefficient should be set to zero##
confint(mod)
## (1.s)(2.m)Model diagnostic checking: A quick way to validate the model is to plot time-series diagnostics using the following command##
tsdiag(mod)
Pic 1
## To be continued##
Notes (pages 13-14)
## To assess how well the model represents the data in the sample, we can plot the raw monthly returns (the thin black solid line) versus the fitted values (the thick red dotted line).##
plot(mod$x, lty = 1, main = "UK house prices: raw data vs. fitted values", ylab = "Return in percent", xlab = "Date")
Pic 2
## (2.s)Furthermore, we can calculate common measures of accuracy: this command returns the mean error, root mean squared error, mean absolute error, mean percentage error, mean absolute percentage error, and mean absolute scaled error.##
accuracy(mod)
Answer:
                                        ME     RMSE       MAE          MPE     MAPE     MASE              ACF1
Training set 0.001229708 1.048264 0.8025635   -Inf          Inf     0.717157         -0.004507474
## Forecasting: The forecasts are given in the first batch of values under $pred and the standard errors of the forecast errors are given in the last line in the batch of results under $se. (3.s)To predict the monthly returns for the next three months (June to Aug 2013), use the following command.##
predict(mod, n.ahead=3) 
Answer:

                    Jun                Jul             Aug
$pred
2013 | 0.7111306 | 0.8406391 | 0.6272879
$se
2013 | 1.054180 | 1.081631      | 1.162169
## Expect a slight increase in the average home prices over the next three months, but with a high standard error of around 1%. To (3.m)plot the forecast with standard errors, we can use the following command:##
plot(forecast(mod))
Pic 3
**We will skip to the chapter related to “Portfolio Optimization” but will still continue to analyze theoretical material for the R forecasting work above.
## To Be Continued ##   
