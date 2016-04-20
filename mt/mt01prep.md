# mt01prep

## MIDTERM REVIEW: MATERIAL

### BIG PICTURE
- Digital Signals
- What is DSP?
- Advantages of DSP over analog processing (easy, stable, flexible)

### DISCRETE-TIME SIGNALS
- [__Representation ($$x[n]$$, what $$n$$ means)__][1]
- [__Sampling, $$x[n]$$ from $$x(t)$$__][2]
	- __Continuous-time vs. Discrete-time__
- __Signal classifications__
	- [__Real vs. Complex__][3]
	- [__Continuous-valued vs. Discrete-valued__][4]
	- [__Finite vs. Infinite__][5]
- [__.$$L_p$$-norm__][6]
- __Operations ([product][7], [multiplication, addition][8], [time-shifting][9], [time-reversal][10], [branching][11])__]
- __Block Diagram representations of systems__
- [__Signal manipulation__][12]
- Sampling Rate alteration
- __[Upsampling][13] / [downsampling][14] — to the degree that it was talked about in lecture__
- [__Convolution__][15]
	- Range/Length of result__
	- [__Performing the convolution__][16]
- [__Finite-Length Sequence Operations__][17]
	- [__Modulo operation__][18]
	- [__Circular time reversal and Circular time shift__][19]
- [__Classification of Sequences__][20]
	- [__Finite-length vs. infinite-length__][5]
	- [__Symmetry (conjugate-symmetric, even, odd)__][21]
	- [__Periodic or aperiodic__][22]
	- [Summability][23]
		- Bounded
		- Absolutely Summable
		- Square Summable
	- [__Energy/Power__][24]
		- __Energy__
		- __Power of a Periodic Sequence__
		- __Power of an Aperiodic Sequence__
- [__Fundamental Period/Frequency__][22]
- __Basic Sequences__
	- [__Unit Sample__][25]
	- [__Unit Step__][26]
	- [__Real Sinusoidal/Exponential__][27]
		- [__Periodicity of Sinusoidal/Exponential sequences__][28]
	- [__Sampling/Aliasing__][29]
	- [__Correlation__][43]
	- [__Autocorrelation__][44]

### DISCRETE-TIME SYSTEMS
- __Some common DT systems__
	- [__Accumulator__][30]
	- [__M-Point Moving Average__][31]
	- [__Exponentially Weighted Running Average__][32]
	- [__Factor-of-n Interpolation__][33]
	- Median Filter
- __Classification of Systems__
	- [__Linear__][34]
	- [__Shift-Invariant__][35]
	- [__Causal__][36]
	- [__Stable__][37]
	- Passive/Lossless
- [__Impulse Responses/Step Responses__][38]
	- __Stability/Causality determination from impulse response, $$h[n]$$__
- [__LTI Systems -> Convolution as a means to obtain output__][39]
- [__Interconnected Systems__][40]
	- __Parallel__
	- __Series__
- Finite-dimensional LTI DT Systems
	- Difference Equation Manipulation
	- Total Solution
	- Zero-Input Response and Zero-State Responses
	- Impulse Response Calculation from the above
- [__Classification of LTI DT Systems__][41]
	- [__Impulse Response Length (FIR vs. IIR)__][42]
	- Output Calculation Process
	- Impulse Response Coefficients

### FREQUNECTY DOMAIN REPRESENTATION OF SIGNALS
- __Big Picture—what is the purpose of frequency-domain analysis__
- __Continuous-Time Fourier Transform (CTFT)__
	- __Definition and how to calculate it__
	- __Inverse CTFT__
	- Dirichlet Conditions
	- __Properties__
	- Energy Density Spectrum
	- Parseval’s Theorem
- __Classifications based on frequency-domain__
	- [__Band-limited or not__][https://chanhi2000.gitbooks.io/elen133/content/lect/lect06.html#bandlimited-ct-signals]
	- [__Ideal vs. practical__][https://chanhi2000.gitbooks.io/elen133/content/lect/lect06.html#ideal-vs-practical]
	- [__Low Pass, Band Pass, High Pass__][https://chanhi2000.gitbooks.io/elen133/content/lect/lect06.html#signal-classification-by-frequency-content]
- __Discrete-Time Fourier Transform (DTFT)__
	- [__Definition and how to calculate it__][https://chanhi2000.gitbooks.io/elen133/content/lect/lect06.html#discretetime-fourier-transform-dtft]
	- [__Inverse DTFT__][https://chanhi2000.gitbooks.io/elen133/content/lect/lect06.html#periodicity-of-dtft]
	- [__Magnitude and Phase Spectra__][https://chanhi2000.gitbooks.io/elen133/content/lect/lect06.html#discretetime-fourier-transform-dtft]
	- __Manipulating the Summation __
	- [__DTFT Properties__][https://chanhi2000.gitbooks.io/elen133/content/lect/lect06.html#dtft-properties]
		- __Convolution property__
- Phase wrapping (more on this later)
- [__Sampling in the frequency domain__][https://chanhi2000.gitbooks.io/elen133/content/lect/lect06.html#sampling-frequency-domain-perspective]

[1]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect01.html#representation-of-discretetime-signals
[2]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect01.html#sampling-continuous-to-discrete-time
[3]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect01.html#real-vs-complex-sequences
[4]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect01.html#continuousvalued-vs-discretevalued-signals
[5]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect02.html#finitelength-vs-infinitelength
[6]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect02.html#norm-size-of-the-signal
[7]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect02.html#product-modulation-operation
[8]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect02.html#multiplication-and-addition
[9]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect02.html#timeshifting
[10]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect02.html#time-reversal
[11]: https://chanhi2000.gitbooks.io/elen133/content/lect/ex02.html#4
[12]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect02.html#signal-operations-review
[13]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect02.html#unsampling
[14]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect02.html#downsampling
[15]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect02.html#convolution
[16]: http://chanhi2000.gitbooks.io/elen133/content/lect/ex02.html#5
[17]: http://chanhi2000.gitbooks.io/elen133/content/lect/ex03.html#1
[18]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#modulo-operation-a-building-block
[19]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#circular-timereversal
[20]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#classification-of-sequences
[21]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#symmetry-classification
[22]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#periodicity
[23]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#other-classifications
[24]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#energy-and-power
[25]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#unit-sample-sequence
[26]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#unit-step-sequence
[27]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#real-sinusoidal-sequence
[28]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect03.html#periodicity-of-sinusoidals-and-exponentials
[29]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect04.html#aliasing
[30]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect04.html#accumulator
[31]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect04.html#mpoint-movingaverage-system
[32]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect04.html#exponentially-weighted-running-average-filter
[33]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect04.html#factorof-interpolator-linear
[34]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect04.html#linearity
[35]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect04.html#shiftinvariant-system
[36]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect05.html#casual-system
[37]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect05.html#stability
[38]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect05.html#impulse-and-step-responses
[39]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect05.html#linear-timeinvariant-lti-system
[40]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect05.html#interconnected-systems
[41]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect05.html#classifications-of-lti-discretetime-systems
[42]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect05.html#more-on-finite-impulse-response-fir
[43]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect06.html#correlation
[44]: https://chanhi2000.gitbooks.io/elen133/content/lect/lect06.html#autocorrelation
