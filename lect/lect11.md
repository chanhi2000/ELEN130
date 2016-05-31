# lect11

## COURSE OVERVIEW:
- ~~Discrete-Time Signals in the Time Domain~~
	- ~~Operations, Classifications, Sampling~~
- ~~Discrete-Time Systems~~
	- ~~Impulse/Step Responses, LTI Classification, Stability, Response to sinusoidal input~~
- ~~Discrete-Time Signals in the Frequency Domain~~
	- ~~Transforms, Applications, Sampling and reconstruction~~
- ~~Finite-Length Discrete Transforms~~
	- ~~DFT, FFT, Zero-padding, Fourier Domain filtering~~, __Linear and Circular convolution__
- __Z-transform__
- Basic filter structures: All pass, LPF, band pass, HPF, comb filter, prototype LPF
- Digital filter structures and representations; 2nd order building blocks
- FIR Design, Windowing
- IIR Design, Bilinear transformation
- IIR filter design with MATLAB
- Review of signal acquisition and reconstruction from frequency domain perspective, interpolating filters, zero-padding, A/D and D/A converters, anti-aliasing filter, sample-and-hold, anti-imaging filter
- Multirate DSP, up-sampling and down-sampling
- Implementation considerations—quantization and dynamic range
- Specific Applications (if time)


## OVERVIEW
- More Z-transform
- Inverse Z-transform techniques
- Type 1, 2, 3, 4 filters


## BIG PICTURE (REVIEW)
- what can we do?
	- $$
	\begin{matrix}
	\text{block diagram}&\leftrightarrow&\text{diff. eqn.}&\leftrightarrow&{h}[n]&\leftrightarrow&{H}\left(e^{j\omega}\right)&\leftarrow&\text{frequency specification for filter}
	\end{matrix}
	$$
	- Quickly compute DTFT and IDTFT using DFT, IDFT through MATLAB’s FFT and IFFT or freqz
- What do we want to be able to do?
	- Have another way to look at the frequency response to quickly determine stability
	- Have a way to design a filter with feedback
	- __These will be done with z-transform__


## OTHER WAYS TO COMPUTE INVERSE Z-TRANSFORM
- Partial Fraction Expansion
- Long Division of Polynomials


## PATIAL FRACTION EXPANSION: EXAMPLE
- Consider:
$$
y[n]=(1.5)y[n-1]-(0.5)y[n-2]+2x[n]+x[n-1]-(0.5)x[n-2]
$$
- Z-transform both sides and group:
$$
Y(z)-(1.5)Y(z)z^{-1}+(0.5)Y(z)z^{-2}=2X(z)+X(z)z^{-1}-(0.5)X(z)z^{-2}
$$
- So
$$
\begin{align*}
H(z)&=\frac{Y(z)}{X(z)}=\frac{\left(2+z^{-1}-0.5z^{-2}\right)}{\left(1-1.5z^{-1}+0.5z^{-2}\right)}\\
&=\frac{2z^2+z-0.5}{z^2-1.5z+0.5}
\end{align*}
$$
- Now, we want to find $$h[n]$$... *i.e.* the inverse Z-transform w/partial fraction expansion (PFE)
- PFE requires order of denominator to be higher than numerator — so, first divide:
$$
\require{enclose}
\begin{array}{r}
2\\[-3pt]
z^2-1.5z+0.5\enclose{longdiv}{2z^2+z-0.5} \\[-3pt]
\:\underline{2z^{2}-3z+1}\phantom{2} \\[-3pt]
4z-1.5\\[-3pt]
\end{array}
$$
So,
$$
\begin{align*}
H(z)&=\frac{2z^2+z-0.5}{z^2-1.5z+0.5}\\
&=2+\underset{H^\prime(z)}{\underline{\frac{4z-1.5}{z^2-1.5z+0.5}}}\\
&=2+\frac{A}{z-0.5}+\frac{B}{z-1}\\
\end{align*}
$$
Solving for $$A$$ and $$B$$:
$$
\begin{align*}
A=\left.H^\prime(z)(z-0.5)\right|_{z=0.5}&=\left.\frac{4z-1.5}{z-1}\right|_{z=0.5}\\
&=\frac{4(0.5)-1.5}{(0.5)-1}\\
&=-1;\\\\
B=\left.H^\prime(z)(z-1)\right|_{z=1}&=\left.\frac{4z-1.5}{z-0.5}\right|_{z=1}\\
&=\frac{4(1)-1.5}{(1)-0.5}\\
&=5;
\end{align*}
$$
So,
$$
\begin{align*}
H(z)&=2-\frac{1}{z-0.5}+\frac{5}{z-1}\\
&=2-\left(\frac{z}{z-0.5}\right)z^{-1}+5\left(\frac{z}{z-1}\right)z^{-1}
\end{align*}
$$
- Thus, the inverse transform of this can be easily looked up in a table:
$$
h[n]=2\delta[n]-0.5^{(n-1)}\mu[n-1]+5\mu[n-1]
$$
- __MATLAB NOTE__: `impz([2, 1, -0.5], [1, -1.5, 0.5])` yields the plot

![fig01]()


## LONG DIVISION OF POLYNOMIALS: EXAMPLE
$$
H(z)=\frac{2z^2+z-0.5}{z^2-1.5z+0.5}
$$
Doing the division directly will yield:
$$
H(z)=2+4z^{-1}+4.5z^{-2}+4.75z^{-3}+\cdots
$$
Which leads directly to
$$
h[n]=2\delta[n]+4\delta[n-1]+4.5\delta[n-2]+4.75\delta[n-3]+\cdots
$$


## STABILITY
- __(1)__ Unit circle must be included in the region of convergence for DTFT to exist.
- __(2)__ Poles CANNOT be included in the region of convergence for stability.
- Thus, if poles are INSIDE the unit circle, causal system is BIBO stable.
- Converse is also true...


### [ANALYSIS EXAMPLE: SOLVE AND SHARE][1]

### [DESIGN EXAMPLE: SOLVE AND SHARE][2]

## MORE BIG PICTURE

## BASIC FILTER STRUCTURE — FIR SYMMETRY
- In filter design by IDTFT for ideal LP filter or differentiation, we got infinite duration result
	- For LP filter, was symmetric about $$n=0$$
	- For difference filter was anti-symmetric
- We truncated filters to $$-N\leq{n}\leq{N}$$ and then shifted to make them causal to approximate desired response.
- Consider general behavior of FIR symmetric and anti-symmetric filters
	- All will have linear phase
	- Depending on order, limits to type of filter.
- Why is linear phase desirable?
	- Pure bulk delay
	- no differentiated impact on different frequencies

[1]:


