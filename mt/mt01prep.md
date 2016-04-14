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
	- __Range/Length of result__
	- __Performing the convolution__
- __Finite-Length Sequence Operations__
	- __Modulo operation__
	- __Circular time reversal and Circular time shift__
- __Classification of Sequences__
	- __Finite-length vs. infinite-length__
	- __Symmetry (conjugate-symmetric, even, odd)__
	- __Periodic or aperiodic__
	- Summability
		- Bounded
		- Absolutely Summable 
		- Square Summable
	- __Energy/Power__
		- __Energy__
		- __Power of a Periodic Sequence__
		- __Power of an Aperiodic Sequence__
- __Fundamental Period/Frequency__
- __Basic Sequences__
	- __Unit Sample__
	- __Unit Step__
	- __Real Sinusoidal/Exponential__
		- __Periodicity of Sinusoidal/Exponential sequences__
	- __Sampling/Aliasing__
	- __Correlation__
	- __Autocorrelation__

### DISCRETE-TIME SYSTEMS
- __Some common DT systems__
	- __Accumulator__
	- __M-Point Moving Average__
	- __Exponentially Weighted Running Average__
	- __Factor-of-n Interpolation__
	- Median Filter
- __Classification of Systems__
	- __Linear__
	- __Shift-Invariant__
	- __Causal__
	- __Stable__
	- Passive/Lossless
- __Impulse Responses/Step Responses__
	- __Stability/Causality determination from impulse response, $$h[n]$$__
- __LTI Systems -> Convolution as a means to obtain output__
- __Interconnected Systems__
	- __Parallel__
	- __Series__
- Finite-dimensional LTI DT Systems
	- Difference Equation Manipulation
	- Total Solution
	- Zero-Input Response and Zero-State Responses 
	- Impulse Response Calculation from the above
- __Classification of LTI DT Systems__
	- __Impulse Response Length (FIR vs. IIR)__
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
	- __Band-limited or not__
	- __Ideal vs. practical__
	- __Low Pass, Band Pass, High Pass__
- __Discrete-Time Fourier Transform (DTFT)__
	- __Definition and how to calculate it__
	- __Inverse DTFT__
	- __Magnitude and Phase Spectra__
	- __Manipulating the Summation __
	- __DTFT Properties__
		- __Convolution property__
- Phase wrapping (more on this later)
- __Sampling in the frequency domain__

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