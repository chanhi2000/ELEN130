# lab01
Discrete Time Signals with MATLAB

## OBJECTIVES:
- Learn and/or review the basic usage of MATLAB as a tool for computation and visualization.
- Use MATLAB to create sampled signals.
- Learn the units for and relationships between sampling intervals, sampling frequency, number of samples observed, total time observed, signal frequency and normalized frequency.


## BACKGROUND NOTES:
__Sampled Signals__: A mathematically defined continuous signal is a signal that exists for every possible value of t. It has infinite resolution as there is no value of $$t$$ for which the signal is not defined. A function defining a continuous signal uses “$$(t)$$” to denote the argument of continuous time, for example, $$x(t)$$.

A continuous signal can be "sampled" by simply evaluating it at integer multiples of a *sampling interval (or sampling period)*, $$T$$. The corresponding *sampling frequency* is $$F_T\tfrac{1}{T}$$.
 
If $$T$$ is the sampling interval in seconds per sample, then $$F_T$$   is the sampling frequency in $$\text{Hertz}$$ (*i.e.* samples per second).
 
- An analog sinusoidal signal with frequency $$f_0$$ in $$\text{Hertz}$$ (*i.e.* cycles per second) is defined by
$$
x_a(t)=A\cos{(2\pi{f}_0t+\varphi)}
$$
where $$A$$ represents amplitude and $$\varphi$$ represents phase.
- The discrete time signal representaion for a sampling interval $$T$$ is given by
$$
x[n]=A\cos{(2\pi{f}_0(nT)+\varphi)}=A\cos{(2\pi\nu{n}+\varphi)}
$$
where $$\nu=f_0T=\tfrac{f_0}{F_T}$$ cycles per sample.

For sinusoidal signals, we will often use the normalized frequency, $$\nu$$. If the actual frequency of the sinusoid to be sampled is $$f_0$$ cycles per second, the *normalized frequency,* $$\nu$$, is the ratio of the actual frequency to the sampling frequency, $$\nu=\tfrac{f_0}{F_T}$$. Unit analysis shows that the units of the normalized frequency are:
$$
\frac{\text{cycles per second}}{\text{samples per second}}=\text{cycles per sample}
$$

The value of the normalized frequency, $$\nu$$ , should be between $$-0.5$$ and $$0.5$$ for accurate representation of the continuous sinusoidal signal by the discrete time sampled signal.


## MATLAB
There are several ways to generate MATLAB instructions.

- 
