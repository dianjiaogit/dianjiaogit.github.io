---
layout: post
title:  "Statistic Machine Learning Notes 3 Linear Regression"
date:   2021-3-1 07:00:00 +0800
categories: [Notes, Statistic Mchine Learning]
---

# Linear Models for Regression

y(x, w) = w0 + w1x1 + ... + wDxD  
w: bias parameter  
x: input (independent with each other)  

## Occam's Razor
Being simple. 

Goal is to minimize squared errors and analytic solution  
convex losses and regularizers  

### Conventions
See X as a matrix, rows are data points, columns are input dimentions.  

## Feature Functions

### Polunomial Basis Functions
Infinite magnitude. Extrapolate poorly  

### Gaussian Basis Functions  
Magnitude bounded. Would not vanish.  

### Sigmoidal Basis Functions  
Hyperbolic tangent.  

## Define Likelihood
beta: percision  

Sum-of-squares error function: E_D(w)

Maximise Likelihood function = Minimise Error function


Then find the stationary point.  

Batch learning
