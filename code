# READ IN DATA, CREATE TIME SERIES ORBJECT
set.seed(2536)
library(astsa)
library(dplyr)
ut_train <- read.csv("urbantraffic_train.csv")
ut_test <- read.csv("urbantraffic_test.csv")
ut <- ut_train %>%
select(timestep, traffic)
ut_data <- ts(ut$traffic, start=0, end=1260)
plot.ts(ut_data)


# TRANSFORM DATA
  # goes from about 0.1 to 0.5 now instead of 0.6 (variance is a little less volatile)
ut_log <- log(ut_data+1)
plot.ts(ut_log)
  # variance looks much more stable here
ut_cube <- (ut_data)ˆ(1/3)
plot.ts(ut_cube)


# ACF/PACF OF ORIGINAL DATA
par(mfrow=c(1,2))
acf(ut_data, lag.max=40)
pacf(ut_data, lag.max=40)


# SEASONAL DIFFERENCE OF TRANSFORMED DATA
ut_logdiff <- diff(ut_log, lag=36)
plot.ts(ut_logdiff)
ut_cubediff <- diff(ut_cube, lag=36)
plot.ts(ut_cubediff)


# ACF/PACF OF DIFFERENCED TRANSFORMED DATA
par(mfrow=c(2,2))
  # log transform + differenced
acf(ut_logdiff, lag.max=150)
pacf(ut_logdiff, lag.max=150)
  # cube root transform + differenced
acf(ut_cubediff, lag.max=150)
pacf(ut_cubediff, lag.max=150)


# FITTING SARIMA MODELS
sarima(ut_cubediff, p=1,d=0,q=2, P=0,D=1,Q=2,S=36)
  # the following ARIMA(1,0,2) model is from auto.arima
sarima(ut_cubediff, p=1,d=0,q=2)


# SARIMA FORECAST
sarima.for(ut_data, n.ahead=36, p=1,d=0,q=2, P=0,D=1,Q=2,S=36,
main="Predicted traffic volume at each of the 36 locations")


# COMBINING HOUR OF THE DAY DUMMY VARIABLES
library(tidyr)
  # combining all hour of the day dummy columns
ut_hourdata <- ut_train %>%
mutate(hour_2 = replace(hour_2, hour_2==1, 2)) %>%
mutate(hour_3 = replace(hour_3, hour_3==1, 3)) %>%
mutate(hour_4 = replace(hour_4, hour_4==1, 4)) %>%
mutate(hour_5 = replace(hour_5, hour_5==1, 5)) %>%
mutate(hour_6 = replace(hour_6, hour_6==1, 6)) %>%
mutate(hour_7 = replace(hour_7, hour_7==1, 7)) %>%
mutate(hour_8 = replace(hour_8, hour_8==1, 8)) %>%
mutate(hour_9 = replace(hour_9, hour_9==1, 9)) %>%
mutate(hour_10 = replace(hour_10, hour_10==1, 10)) %>%
mutate(hour_11 = replace(hour_11, hour_11==1, 11)) %>%
mutate(hour_12 = replace(hour_12, hour_12==1, 12)) %>%
mutate(hour_13 = replace(hour_13, hour_13==1, 13)) %>%
mutate(hour_14 = replace(hour_14, hour_14==1, 14)) %>%
mutate(hour_15 = replace(hour_15, hour_15==1, 15)) %>%
mutate(hour_16 = replace(hour_16, hour_16==1, 16)) %>%
mutate(hour_17 = replace(hour_17, hour_17==1, 17)) %>%
mutate(hour_18 = replace(hour_18, hour_18==1, 18)) %>%
mutate(hour_19 = replace(hour_19, hour_19==1, 19)) %>%
mutate(hour_20 = replace(hour_20, hour_20==1, 20)) %>%
mutate(hour_21 = replace(hour_21, hour_21==1, 21)) %>%
mutate(hour_22 = replace(hour_22, hour_22==1, 22)) %>%
mutate(hour_23 = replace(hour_23, hour_23==1, 23)) %>%
mutate(hour_24 = replace(hour_24, hour_24==1, 24)) %>%
mutate(across(hour_1:hour_24, na_if, 0))%>%
unite(hour, hour_1:hour_24, sep="", na.rm=TRUE)%>%
select(timestep, hour)
  # number of lanes
ut_lanesdata <- ut_train %>%
select(timestep, no_roads)


# CREATING TIME SERIES OBJECTS FOR POTENTIAL PREDICTORS
ut_hour <- ts(ut_hourdata$hour, start=0, end=1260)
ut_lanes <- ts(ut_lanesdata$no_roads, start=0, end=1260)
par(mfrow=c(3,1))
plot.ts(ut_data)
plot.ts(ut_hour)
plot.ts(ut_lanes)


# USING VARSELECT
library(vars)
x <- cbind(ut_data, ut_lanes)
VARselect(x, type="both")
var1 <- VAR(x, p=10, type="both")
summary(var1)$varresult$ut_data


# RESIDUAL DIAGNOSTICS OF ARMAX MODEL
acf(resid(var1))
serial.test(var1, lags.pt=36, type="PT.adjusted")


# ARMAX FORECAST
armax_pred <- predict(var1, n.ahead=36, ci=.95)
armax_forecast <- ts(armax_pred$fcst$ut_data[,1], start=1260, end=1296)
par(mfrow=c(1,2))
ts.plot(ut_data, armax_forecast, col=c("blue", "purple"))
title("ARMAX forecast")
legend("topleft",legend= c("actual", "ARMAX"), col=c("blue","purple"), lty=1)
ts.plot(ut_data, armax_forecast, col=c("blue", "purple"), xlim=c(1150, 1296))
title("ARMAX forecast - Closer Look")
legend("topleft",legend= c("actual", "ARMAX"), col=c("blue","purple"), lty=1)
