---
layout: post
title: "An Introduction to Time Series Decomposition"
date: 2018-08-09
---

My Mac is Back!
And so am I lol.
I'll do a Time Series Decomposition post today - let's jump right in!

---

__What is Time Series Decomposition?__

First off, a definition of time series: a series (haha) of data points listed in time order.

Time series decomposition begins with an assumption these data points are the result of the combination of some components.
These components can be long-range trend, short-term seasonality, and randomness.
How do we combine them? There are additive models, and multiplicative models. (We'll go more into this later).

Armed with this assumption, we can then separate a time series into each of its components - decomposition!

The below picture shows 4 graphs - 1) observed: the actual values; 2) trend: long-term effect/pattern; 
3) seasonal: recurring short-term effect; 4) random: remaining irregularities unaccounted for by the other components.

<img src="https://user-images.githubusercontent.com/21985915/43876851-62508334-9bc9-11e8-900e-8ba830c2678a.png" width="500">


__What is Time Series Decomposition used for?__

Forecasting! If you are able to break down and figure out the patterns contributing to your desired value, 
you can roughly extrapolate the patterns and predict the next time period's value... right? :p

1) Take a step back - be aware that in using this method of analysis, we are merely examining the past behaviour to predict the future.
Remember to assess how suitable that is in your use case. The future might not be the same as the past, especially when circumstance change!
Time series might be reliable only in the short run.

2) Also, there is no talk of any causality at all! Hence time-series models become particularly useful when you know little about the underlying process you are trying to forecast. 


You can also use it for Outlier Detection! Especially using the Random component - for example, you get the mean and standard deviation
of the Random Component, and then flag those Random points which fall outside of say, 4 standard deviations, as outliers.

An image below to illustrate the process:

<img src="https://user-images.githubusercontent.com/21985915/43878397-770acf8e-9bd1-11e8-869b-eb27e18e48e4.jpeg" width="500">


---

__How does it compare against other alternatives?__
The above is a Classical Decomposition. Read more about its short-comings here: https://otexts.org/fpp2/classical-decomposition.html#fig:classical-elecequip

Essentially: 
1) No estimates for first and last few observations
2) Unable to capture seasonal changes over time
3) No robust to unusual values

Alternatives:
- STL (Seasonal and Trend decomposition using Loess)
- X11
- SEATS

Random:
- ARIMA:
AutoRegressive Integrated Moving Average (ARIMA) is a generalised ARMA model. 
AR: autoregressive - output variable depends linearly on its own previous values + stochastic (possible values are outcome of a random phenomenon).
MA: moving average - output variable depends on residuals/regression error (the difference of the actual from the predicted) of previous forecasts.
I: integrated - differencing (replacing data values with the difference between their values and the previous values) the data to remove the trend and convert a non-stationary time series to a stationary one (values remain at fairly constant levels over time)


That's all for today - more to come next time! 

Such as:
__How does Time Series Decomposition actually work?__

More links for readings (and as credits cause I learnt so much from them!):
- https://www.economicsnetwork.ac.uk/showcase/cook_timeseries
- http://home.ubalt.edu/ntsbarsh/stat-data/forecast.htm#rasofm
- https://www.quora.com/What-are-the-practical-aspects-of-outlier-detection-in-time-series-data
- https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average
- https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model
