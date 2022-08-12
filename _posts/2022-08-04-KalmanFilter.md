---
layout: post
title: How to transform noise into meaningful readings
---

Often, when reading data from sensors, we have to deal with measurement noise and time changing biases.

For example let's consider a gyroscope, a sensor which output is the angular speed. Gyroscopes output is noisy and its bias is described by a Brownian motion, it changes randomly in time. If we can mitigate the noise with a low pass filter, how can we estimate the gyroscope bias?

Another problem that we want to solve is related to the best operative conditions of a sensor. 

Let's consider a spinning satellite. A possible way to estimate its orientation is to use a star sensor, a camera that recognize the stars in the sky and, since their position is constant in time, it outputs the satellite absolute orientation. Since star sensors have a slow refresh rate, they usually have a bad estimate of the angular velocity, useful for control purposes. Moreover, a slow refresh rate means a "mostly coarse" orientation estimate since the orientation is accurate only near the sensor update. 

A gyroscope, on the other hand, has a higher refresh rate. The problem with it is that it doesn't measure the orientation but the angular velocity. In an ideal world the orientation is the integral in time of the angular velocity but, since in the real world the measurements are not perfect, when integrating in time the error builds up. If for shorter integration periods the estimate of the orientation is accurate, for longer time periods this stops to be true.

One last question is: is it possible to mix an imprecise theoretical model with the sensors readings in a way to improve the quality of the measurements?

A possible answer to all the discussed doubts and questions is "[Kalman filter](https://en.wikipedia.org/wiki/Kalman_filter)".

A Kalman filter, in its original flavour, is a statistically optimal linear observer.

This complicated words means that, for a linear system, the Kalman filter estimate error is on average 0 and its covariance is as small as possible.

Note: the average is not enough since it doesn't say anything on the entity of the error. Let's consider two sets $S_1 = \{-1; 1\}$ and $S_2 = \{-100; 100\}$. Even if the average value of both sets is 0, we are intuitively convinced that we want the error to be more like $S_1$ rather than $S_2$. This distinction can be done by looking at the covariance since the covariance of $S_1$ is much smaller than the covariance of $S_2$.