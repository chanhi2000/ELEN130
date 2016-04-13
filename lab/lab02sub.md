# lab02sub
Impulse, Step, and Frequency Response

## OBJECTIVES:
- Learn to use MATLAB to determine the response of first and second order Finite Impulse Response (FIR) and Infinite Impulse Response (IIR) systems defined by difference equations. 
- Learn about inputs in this lab, including an impulse, a step function, and a signal with constant frequencies.


## INITIALIZE:
```matlab
clear; clc; clf; cla; close all;
```

## PRELAB
Write two simple MATLAB functions `lab1plot1(t, h, s, ix)` and `lab1plot2(t, y1, y2, y3, f1, f2, f3, ix)` 

### lab1plot1.m
```matlab
function lab1plot1(t, h, s, ix)
% inputs: a time vector (t), two input vectors (h and s); subplot index (ix)
% The two input vectors are plotted in different colors on the same axes 
% against the time vector, (t), in subplot (ix) of a 2x2 subplot array.
```

### lab1plot2.m
```matlab
function lab1plot2(t, y1, y2, y3, f1, f2, f3, ix)
% inputs: a time vector (t), three vectors (y1, y2, y3); 
% three associated frequency values (f1, f2, f3); and subplot index (ix)
% The three input vectors are plotted in different colors on the same axes 
% against the time vector, (t) ,in subplot (ix) of a 2x2 subplot array. 
% The frequency values are printed in the plot title.
```