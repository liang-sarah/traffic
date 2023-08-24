# traffic
The goals and concepts used in this time series analysis project are explained below. The presentation document for the project is [here](https://liang-sarah.github.io/traffic/174_FinalProj.pdf).

<br />

#### R packages used:
<table border = "0">
  <tr>
    <td>astsa</td> <td>dplyr</td> <td>tidyr</td> <td>vars</td>
  </tr>
</table>

<br />

## Abstract
Using traffic flow and volume data, I fitted SARIMA and ARMAX model to the data, with the goal to
forecast and predict traffic volume in the next 15 minutes in the locations at which my data was recorded.

<br />

## Introduction
I decided to use traffic flow Kaggle data. My data set contains 1261 timesteps in the training split (and 840 timesteps
in the testing split), involving traffic volume measurements along two major highways in Northern Virginia
and Washington D.C. capital region, measured every 15 minutes at 36 sensor locations. The reason I chose
traffic volume was that it was something that directly affected people. Prediction of traffic in certain areas
can actually improve the livelihoods of people living in those areas. My analysis and prediction using this
data can be used to analyze and improve the sustainability of cities.

<br />

## Methodology
With a SARIMA model, it is easier to apply ARIMA model concepts to real life data that is not automatically
stationary, or might have seasonal components that aren’t considered in a basic ARIMA model. Often
times, dependence on the past can induce seasonality in our data. As an overview, the ARIMA and
SARIMA models rely heavily on the interactions of autoregression and moving average. Autoregression
is when a time series depends on a linear combination of lagged instances of the times series itself and
white noise residual. Moving average is behavior when a time series depends on instances of lagged white noise.

A normal ARIMA model has parameters p, d, and q, which represent autoregressive operator, difference, and
moving average operator respectively.

A SARIMA model is an normal ARIMA model with four more parameters that have to do with the seasonal
part of the model: P, D, Q, and S which represent seasonal autoregressive operator, seasonal difference,
seasonal moving average operator, and seasonal period respectively.

An ARMAX model is an ARMA model with the incorporation of exogeneous variables.

Since my data set also includes other variables, we can explore the dependencies between our traffic data and
the exogenous variables using an ARMAX model.

<br />

## Conclusion
The SARIMA model is very accurate in capturing the seasonal components in the original traffic flow
data. Forecasting using the SARIMA model is also impressive and accurate to the actual traffic behavior.
On the other hand, the ARMAX model provides a vague, innaccurate forecast result. It is somewhat
constant in mean, which does somewhat line up with the actual mean of the actual data. However, the
essence of the actual data comes from the seasonality, which is something the ARMAX model fails to represent.

Regardless, fitting these models on actual data and seeing results is still one of my goals here. I achieved a
fantastic result from the SARIMA model, so that is something I achieved well. In this project, we discovered
how traffic flow varies in these 36 locations within the Northern Virginia and Washington D.C. areas (which
results in the seasonality of the data). Though, we now know that the overall increasing variance in the
original data is likely correlated with the hour of the day. As the hour of the day grows from midnight to
midnight, the variance grows. The number of lanes can also affect traffic flow. Intuitively this makes sense,
the more lanes the less congested it is to drive. Overall, the SARIMA model produced a great result, and
I can grow further in this project to achieve better results, perhaps in considering other models. Or I can
search for other traffic data for better and more variety in variables to use in the ARMAX model
