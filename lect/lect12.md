# lect12

## COURSE OVERVIEW:
- ~~Discrete-Time Signals in the Time Domain~~
	- ~~Operations, Classifications, Sampling~~
- ~~Discrete-Time Systems~~
	- ~~Impulse/Step Responses, LTI Classification, Stability, Response to sinusoidal input~~
- ~~Discrete-Time Signals in the Frequency Domain~~
	- ~~Transforms, Applications, Sampling and reconstruction~~
- ~~Finite-Length Discrete Transforms~~
	- ~~DFT, FFT, Zero-padding, Fourier Domain filtering, Linear and Circular convolution~~
- ~~Z-transform~~
- __Basic filter structures: All pass, LPF, band pass, HPF, comb filter, prototype LPF__
- Digital filter structures and representations; 2nd order building blocks
- FIR Design, Windowing
- IIR Design, Bilinear transformation
- IIR filter design with MATLAB
- Review of signal acquisition and reconstruction from frequency domain perspective, interpolating filters, zero-padding, A/D and D/A converters, anti-aliasing filter, sample-and-hold, anti-imaging filter
- Multirate DSP, up-sampling and down-sampling
- Implementation considerations—quantization and dynamic range
- Specific Applications (if time)


## OVERVIEW
- Type 1, 2, 3, 4 Filters

## BASIC FILTER STRUCTURE — FIR SYMMETRY
- In filter design by IDTFT for ideal LP filter or differentiation, we got infinite duration result
	- For LP filter, was symmetric about $$n=0$$
	- For difference filter was anti-symmetric
- We truncated filters to $$–N\leq{n}\leq{N}$$ and then shifted to make them causal to approximate desired response.
- Consider general behavior of FIR symmetric and anti-symmetric filters
	- All will have linear phase
	- Depending on order, limits to type of filter.
- Why is linear phase desirable?
	- Pure bulk delay — no differentiated impact on different frequencies


## MAGNITUDE / PHASE OF FILTERS
$$
H\left(e^{j\omega}\right)=\left|H\left(e^{j\omega}\right)\right|e^{j\theta(\omega)}
$$
- Effect of magnitude and phase are both important
- Phase determines position in time and alignment of frequency components in time.

### PHASE EXAMPLE
$$
x[n]\underrightarrow{DTFT}X\left(e^{j\omega}\right)
$$
Given $$x_1[n]=x[n-n_0]$$, What is $$X_1 \left(e^{j\omega}\right)$$?
$$
\begin{align*}
h[n]&=\delta[n-n_0]\\
H\left(e^{j\omega}\right)&=e^{-\omega{n}_0}
\end{align*}
$$
So,
$$
\begin{align*}
x_1[n]&=x[n-n_0]\\
X_1\left(e^{j\omega}\right)&=e^{-\omega{n}_0}X\left(e^{j\omega}\right)
\end{align*}
$$
- Linear phase of $$-\omega{n}_0$$ that corresponds to a delay of $$n_0$$.
- __Group delay__:
$$
\tau_g(\omega)=\frac{-d\delta(\omega)}{d\omega}
$$
- Thus, time domain delay maps to linear phase.
- Consider example:
$$
\cos{\left(\omega(n-n_0)\right)}=\cos{\left(\omega{n}-\omega{n}_0)\right)}
$$
- Phase is a linear function of time shift.

## EVEN SYMMETRIC FIR FILTER OF ORDER N
- Consider general class of even symmetric FIR filter of order $$N$$, length $$N+1$$ with real-valued coefficients
	- Even symmetry means
	$$
	h[n]=h[N-n]
	$$
	- Odd symmetry means:
	$$
	h[n]=-h[N-n]
	$$
- Claim that if $$z_0$$ is a zero of $$H(z)$$, then $${z_0}^{-1}$$ is also a zero of $$H(z)$$.
$$
\begin{align*}
H\left(z\right)&=\sum_{n=0}^{N}{h[n]z^{-n}}\\
H\left(z^{-1}\right)&=\sum_{n=0}^{N}{h[n]z^n}\\
&=z^{N}\sum_{n=0}^{N}{h[n]z^{n-N}}\\
&=z^{N}\sum_{m=N}^{0}{h[N-m]z^{-m}}\\
&=z^{N}\sum_{m=0}^{N}{h[m]z^{-m}}\\
&=z^{N}H(z)
\end{align*}
$$
So,
$$
\therefore\:H(z)=z^{-N}H\left(z^{-1}\right)
$$
- zeros can be at:
	- .$$+1$$ or $$-1$$
	- .$$p$$ or $$\tfrac{1}{p}$$
	- .$$e^{+j\omega_0}$$ or $$e^{-j\omega_0}$$
	- .$$pe^{+j\omega_0}$$ or $$\tfrac{1}{p}e^{-j\omega_0}$$



## FACTORS IN THE PROPERTIES OF FILTERS
- Zero placement
- Even/odd symmetry
- Even/odd length


## TYPE 1 FILTER
- SYMMETRY: even
- .$$N$$ is even... length $$N+1$$ is odd.
- Since $$N$$ is even, zeros occur in pairs or groups of $$4$$.
- Either no zeros at $$z=1$$ or $$z=-1$$ OR pairs of zeros at $$z=1$$ or $$z=-1$$ (*i.e.* even numbers of zeros at $$z=1$$ or $$z=-1$$)
- __example__:
$$
h[n]=\delta[n]+\alpha\delta[n-1]+\delta[n-2]
$$
	- __order__: 2
	- __length__: 3
	- __symmetry__: even
	$$
	h[0]=h[2]=1
	$$
$$
\begin{align*}
H(z)&=1+\alpha{z}^{-1}+z^{-2}\\
&=\frac{z^2+\alpha{z}+1}{z^2}
\end{align*}
$$
- So, zeros are at:
$$
z=\frac{-\alpha\pm\sqrt{\alpha^2-4}}{2}
$$
	- if $$|\alpha|<2$$,
	$$
	z=\frac{-\alpha\pm{j}\sqrt{4-\alpha^2}}{2}
	$$
	*i.e.*, 2 zeros in unit circle, $$z_0$$ and $${z_0}^*$$
	- if $$\alpha=2$$, 2 zeros at $$z=-1$$
	- if $$\alpha=-2$$, 2 zeros at $$z=1$$
	- if $$|\alpha|>2$$,
	$$
	z=\frac{-\alpha\pm\sqrt{\alpha^2-4}}{2}
	$$
	*i.e.* 2 zeros at $$z_0$$ and $$\tfrac{1}{z_0}$$.
- It has a __linear phase__
- Z-transform:
$$
\begin{align*}
H(z)&=\sum_{n=0}^{N}{h[n]z^{-n}}\\
&=\sum_{n=0}^{\tfrac{N}{2}-1}{h[n]z^{-n}}+h\left[\tfrac{N}{2}\right]z^{-\tfrac{N}{2}}+\sum_{n=\tfrac{N}{2}+1}^{N}{h[n]z^{-n}}\\
&=\sum_{n=0}^{\tfrac{N}{2}-1}{h[n]z^{-n}}+h\left[\tfrac{N}{2}\right]z^{-\tfrac{N}{2}}+\sum_{n=\tfrac{N}{2}+1}^{N}{h[N-n]z^{-n}}\\
&\left<m=N-n\right>\\
&=\sum_{n=0}^{\tfrac{N}{2}-1}{h[n]z^{-n}}+h\left[\tfrac{N}{2}\right]z^{-\tfrac{N}{2}}+\sum_{m=\tfrac{N}{2}-1}^{0}{h[m]z^{-(N-m)}}\\
&=z^{-\tfrac{N}{2}}\left\{\sum_{n=0}^{\tfrac{N}{2}-1}{h[n]z^{-\left(n+\tfrac{N}{2}\right)}}+h\left[\tfrac{N}{2}\right]+\sum_{m=0}^{\tfrac{N}{2}-1}{h[m]z^{\left(m-\tfrac{N}{2}\right)}}\right\}\\
&=z^{-\tfrac{N}{2}}\left\{h\left[\tfrac{N}{2}\right]+\sum_{n=0}^{\tfrac{N}{2}-1}{h[n]\left(z^{-\left(n-\tfrac{N}{2}\right)}+z^{\left(n-\tfrac{N}{2}\right)}\right)}\right\}\\
H\left(e^{j\omega}\right)&=e^{-j\omega\tfrac{N}{2}}\left\{h\left[\tfrac{N}{2}\right]+\sum_{n=0}^{\tfrac{N}{2}-1}{h[n]\left(e^{-j\omega}{\left(n-\tfrac{N}{2}\right)}+e^{j\omega\left(n-\tfrac{N}{2}\right)}\right)}\right\}\\
&=e^{-j\omega\tfrac{N}{2}}\underset{\text{real-valued}}{\underline{\left\{h\left[\tfrac{N}{2}\right]+\sum_{n=0}^{\tfrac{N}{2}-1}{h[n]\left(2\cos{\left(\omega\left(n-\frac{N}{2}\right)\right)}\right)}\right\}}}\\
\end{align*}
$$
	- linear phase delay of $$\tfrac{N}{2}$$
	- Since $$N$$ is even, this delay is an integer number of samples


## TYPE 2 FIR FILTER
- SYMMETRY: even
- .$$N$$ is odd... length $$N+1$$ is even.
- Same type of analysis
- Even number of zeros or no zeros at $$z=1$$
- Odd number of zeros at $$z=-1$$.
- On a DTFT plot, where are the zeros and where are the non-zeros?
$$
\begin{matrix}
z=1&\to&w=0\\
z=-1&\to&w=\pm\pi
\end{matrix}
$$
- Therefore, TYPE 2 FILTER CANNOT be a HPF because at least one zero at $$z=-1$$.
- __example__:
$$
h[n]=a\delta[n]+b\delta[n-1]+b\delta[n-2]+a\delta[n-3]
$$
	- __order__: 3
	- __length__: 4
	- __symmetry__: even
$$
H\left(e^{j\omega}\right)=a+be^{-j\omega}+be^{-j2\omega}+ae^{-j3\omega}
$$
- Reduce this to standard $$e^{j\theta(\omega)}\left|H\left(e^{j\omega}\right)\right|$$.
$$
H\left(e^{j\omega}\right)=e^{-j\omega\tfrac{3}{2}}\underset{\text{real-valued}}{\underline{\left(2a\cos{\left(\tfrac{3}{2}\omega\right)}+2b\cos{\left(\tfrac{1}{2}\omega\right)}\right)}}
$$
	- linear phase delay of $$\tfrac{3}{2}$$

## EXERCISE: TYPE 1 FIR FILTER TIMES (z+1)
- What is the result of taking a TYPE 1 FIR FILTER and multiplying it (in the Z-domain) by $$(z+1)$$?
- Let's try an example:
$$
\begin{align*}
h[n]&=\delta[n]+a\delta[n-1]+\delta[n-2]\\
H(z)&=1+az^{-1}+z^{-2}\\
H(z)(z+1)&=\left(1+az^{-1}+z^{-2}\right)(z+1)\\
&=z+a+z^{-1}+1+az^{-1}+z^{-2}\\
&=z+(1+a)+(1+a)z^{-1}+z^{-2}
\end{align*}
$$
	- __symmetry__: even
	- __order__: 3
	- __length__: 4...
- Type 2 FIR Filter!
- It’s looks funny... generally, when we add a zero, we also add a pole at $$z=0$$.


## EXERCISE: TYPE 1 FIR FILTER TIMES (z+1)/z
- What is the result of taking a TYPE 1 FIR FILTER and multiplying it (in the Z-domain) by $$\tfrac{z+1}{z}$$?
$$
\begin{align*}
h[n]&=\delta[n]+a\delta[n-1]+\delta[n-2]\\
H(z)&=1+az^{-1}+z^{-2}\\
H(z)\frac{z+1}{z}&=\frac{\left(1+az^{-1}+z^{-2}\right)(z+1)}{z}\\
&=\frac{z+(1+a)+(1+a)z^{-1}+z^{-2}}{z}\\
&=1+(1+a)z^{-1}+(1+a)z^{-2}+z^{-3}
\end{align*}
$$
- Looks less funny... Taking the inverse yields the time-domain:
$$
h[n]=\delta[n]+(1+a)\delta[n-1]+(1+a)\delta[n-2]+\delta[n-3]
$$


## WHAT IS MULTIPLYING (z+1)/z IN THE TIME-DOMAIN
- Same as convolving our time-domain filter with Inverse Z-transform of $$\tfrac{z+1}{z}$$
- Which is $$1+\tfrac{1}{z}$$
- So,it is the same as convolving with
$$
\delta[n]+\delta[n-1]
$$


## TYPE 3 FIR FILTER
- SYMMETRY: odd
$$
h[n]=-h[N-n]
$$
- .$$N$$ is even, length $$N+1$$ is odd.
- __NOTE__:
$$
h\left[\frac{N}{2}\right]=0
$$
because of odd symmetry
- We now have an odd number of zeros at $$z=1$$ and $$z=-1$$.
- What does this mean in terms of what type of filter this can be?


## TYPE 3 FIR FILTER: EXAMPLE
$$
\begin{align*}
h[n]&=\delta[n]-\delta[n-2];\\
H(z)&=1-z^{-2}\\
&=\frac{z^2-1}{z^2}\\
&=\frac{(z-1)(z+1)}{z^2};\\
H\left(e^{j\omega}\right)&=e^{-j\omega}\left(2j\sin{\left(\omega\right)}\right);
\end{align*}
$$



## TYPE 4 FIR FILTER
- SYMMETRY: odd
- .$$N$$ is odd, length $$N+1$$ is even.
- Will have an odd number of zeros at $$z=1$$ and even number or no zeros at $$z=-1$$.
- Using similar logic as before, this cannot be a LPF.
- This is like TYPE 2 except with sines instead of cosines.
- __example__:
$$
\begin{align*}
h[n]&=\delta[n]-\delta[n-1]\\
H(z)&=1-z^{-1}\\
&=\frac{z-1}{z}\\
H\left(e^{j\omega}\right)&=e^{-j\omega\tfrac{1}{2}}\left(2j \sin{\left(\frac{1}{2}\omega\right)}\right)
\end{align*}
$$


## QUICK (BUT IMPORTANT) ASIDE
- FIR VS. IIR
- __Linearity of phase__: can you say something about this based on the filter being FIR or IIR?
	- FIR filters CAN have linear phase. The TYPE 1, 2, 3, and 4 always have linear phase!
- Where are the poles for an FIR filter?
	- FIR filters MUST have poles at only the origin.
- Can a filter with feedback be an FIR filter?
	- Technically, yes, but it would be contrived. No *useful* feedback will result in an FIR filter.
	- *e.g.*:
	$$
	y[n]=x[n]+x[n-1]-y[n-1]
	$$
	- What is the resulting $$h[n]$$?
	$$
	h[n]=\delta[n]
	$$


## SUMMARY OF FIR FILTERS
| TYPE | ORDER (N) | SYMMETRY | LPF/HPF |
| :--: | :-------: | :------: | :-----: |
| 1 | EVEN | EVEN | |
| 2 | ODD | EVEN | NO LPF |
| 3 | EVEN | ODD | NO LPF / NO HPF |
| 4 | ODD |  ODD | NO HPF |



## EXAMPLE OF SYMMETRIC FILTERS
- ideal LPF
$$
H\left(e^{j\omega}\right)=\begin{cases}
1&|\omega|<\omega_0\\
0&\omega_c<|\omega|<\pi
\end{cases}
$$
- IDTFT:
$$
\begin{align*}
h[n]&=\frac{\sin{\left(\omega_cn\right)}}{\pi{n}}\\
&=\frac{\omega_c}{\pi}\frac{\sin{\left(\omega_cn\right)}}{\omega_cn}
\end{align*}
$$


## PRACTICAL LPF
- approximate implementation (practical LPF):
	- make finite duration
	- make casual — delay by $$M$$
$$
\begin{align*}
h_2[n]&=\begin{cases}
h[n]&-M\leq{n}\leq{M}\\
0&|n|>M
\end{cases}\\
\text{length: }&2M+1\\
\text{order }&2M\\\\
h_3[n]&=\underset{\text{causal}}{h_2[n-M]}
\end{align*}
$$
- Symmetry
$$
\begin{align*}
h_2[n]:&\text{symmetric about }0,\\
h_3[n]:&\text{symmetric about }n=M=\frac{N}{2}
\end{align*}
$$

## EXAMPLE OF ANTI-SYMMETRIC FILTERS
- Have odd symmetry
- Ideal differentiator:
$$
\begin{align*}
H\left(e^{j\omega}\right)&=j\omega&&0\leq|\omega|<\pi\\\\
h[n]&=\begin{cases}
0&n=0\\
\frac{(-1)^n}{n}&|n|>0
\end{cases}
\end{align*}
$$
- Ideal Hilbert Transformer: $$90^{\circ}$$ phase shift
$$
\begin{align*}
H\left(e^{j\omega}\right)&=\begin{cases}
j&-\pi<\omega<0\\
-j&0<\omega<\pi\\
0&\omega=0
\end{cases}\\
h[n]&=\begin{cases}
0&\underset{\text{even}}{\forall{n}}\\
\frac{2}{\pi{n}}&\underset{\text{odd}}{\forall{n}}\\
\end{cases}
\end{align*}
$$


## DISCUSSION QUESTION (1)
- An order $$N$$ filter with even symmetry is increased to order $$N+1$$ by adding a zero $$z=-1$$.
- Show that the new filter will have even symmetry.
- What does it mean to add a zero at $$z=-1$$?
- What can this look like in the time domain?
- How do we test for even symmetry?


## DISCUSSION QUESTION (2)
- ideal differentiator:
$$
\begin{align*}
H\left(e^{j\omega}\right)&=j\omega&&0\leq|\omega|<\pi\\\\
h[n]&=\begin{cases}
0&n=0\\
\frac{(-1)^n}{n}&|n|>0
\end{cases}
\end{align*}
$$
- Causeal FIR approximation is implemented with TYPE 3 symmetric (odd symmetry) filter of order $$N=2M$$ by:
$$
h_\text{diff}[n]=\begin{cases}
h[n-M]&0\leq{n}\leq{N}\\
0&\text{oterwise}
\end{cases}
$$
- Find filter coefficients for $$N=2$$ and for $$N=4$$.
- For $$N=4$$, and using the limitations of positions of zeros for a TYPE 3 symmetric filter, find the zeros for this filter.


## CONVERTING A LPF TO A HPF
- __METHOD 1__: ideal, no delay — SUBTRACT
$$
\begin{align*}
H_{HP}(z)&=1-H_{LP}(z);\\
H_{HP}(e^{j\omega})&=1-H_{LP}(e^{j\omega});\\
h_{HP}[n]&=\delta[n]-h_{LP}[n]
\end{align*}
$$
- For even, order $$N$$, causal FIR filter:
$$
\begin{align*}
H_{HP}(z)&=z^{-\tfrac{N}{2}}-H_{LP}(z)\\\\
h_{HP}[n]&=\begin{cases}
-h_{LP}[n]&n\neq\frac{N}{2};\:\:\:\:0\leq{n}\leq{N}\\
1-h_{LP}[n]&n=\frac{N}{2}
\end{cases}
\end{align*}
$$
- __METHOD 2__: ideal, no delay: ROTATE BY $$\pi$$
$$
\begin{align*}
H_{HP}(z)&=H_{LP}(-z)\\
H_{HP}(e^{j\omega})&=H_{LP}(-e^{j\omega})\\
&=H_{LP}(e^{j(\omega-\pi)})\\
h_{HP}[n]&=(-1)^nh_{LP}[n]
\end{align*}
$$
- For an even, order $$N$$, causaal FIR filter:
$$
\begin{align*}
H_{HP}(z)&=(-1)^{-\frac{N}{2}}H_{LP}(-z)\\
h_{HP}[n]&=(-1)^{-\frac{N}{2}}(-1)^nh_{LP}[n]
\end{align*}
$$

### [EXAMPLE: SIMPLE FIR LPF][]

### [EXAMPLE: BETTER FIR LPF][]


## LPF FILTER COMPARISON
- Assess filters by looking at $$3\:\text{dB}$$ point and also looking at shape in passband and stopband.
- Compare previous filter to moving average of same length: *i.e.*
$$
H(z)=\frac{1}{4} \left(1+z^{-1}+z^{-2}+z^{-3}\right)
$$

### [EXAMPLE: SIMPLE HPF]


## COMB FILTER
- Multiple passbands and stop bands from prototype filter
$$
G(z)=H(z^L)
$$
has frequency response which is a periodic functionof $$\omega$$ with a period of $$\frac{2\pi}{L}$$.

### [EXAMPLE: COMB FILTER(1)][]

### [EXAMPLE: COMB FILTER(2)][]


## ALL PASS FILTER
- Magnitude of the frequency response is $$1$$, but phase controlled by filter coefficients.
- Consider (like the HW)
$$
H(z)=\frac{a-z^{-1}}{1-az^{-1}}
$$
so
$$
H(z)=\frac{a-e^{-j\omega}}{1-ae^{-j\omega}}
$$
- Math
$$
\begin{align*}
|\text{numerator}|&=|\text{denominator}|\\
\text{phase}&=\angle{\text{numerator}}-\angle{\text{denominator}}\\
&=\frac{\sin{\left(\omega\right)}}{a-\cos{\left(\omega\right)}}-\frac{a\sin{\left(\omega\right)}}{1-a\cos{\left(\omega\right)}}\\
&=\frac{\sin{\left(\omega\right)}}{a-\cos{\left(\omega\right)}}-\left(\frac{\sin{\left(\omega\right)}}{\tfrac{1}{a}-\cos{\left(\omega\right)}}\right)
\end{align*}
$$
- In general,
$$
H(z)=\pm\frac{\left(z^{-M}\right)D_M\left(z^{-1}\right)}{D_M(z)}
$$
will be all pass, Why?
$$
D_M(z)=1+d_1z^{-1}+\cdots+d_{M-1}z^{-(M-1)}+d_Mz^{-M}
$$
is the denominator polynomial
- Numerator is mirror image polynomial
$$
=d_M+d_{M-1}z^{-1}+\cdots+d_1z^{-(M-1)}+z^{-M}
$$
So
$$
\begin{align*}
H(z)H(z^{-1})&=\left(\frac{z^{-M}D_M(z^{-1})}{D_M(z)}\right)\left(\frac{z^{M}D_M(z)}{D_M(z^{-1})}\right)\\
&=1;
\end{align*}
$$


## STRUCTURE FOR FILTER IMPLEMENTATION
- Tapped Delay Line:


- This is called "DIRECT FORM" — *i.e.* coefficients of transfer function are multipliers in filter structure.
- What is this implementing?


## ANOTHER STRUCTURE FOR FILTER
- Transpose structure:
	- Reverse inputs and outputs
	- Reverse direction
	- Interchange adders and multipliers
- Is this the same?


## DIRECT FORM vs. TRANSPOSE STRUCTURE
- Memory of the System is $$x[n-1]$$, $$x[n-2]$$, $$x[n-3]$$
	- Do this with memory and addressing strategy
	- FIFO, circular queue
	- Hardware support – not actual shifting
-  Input enters system and shifts through
- Output computed by multiplying coefficients by most recent four inputs and adding
- No specific memory of input history
- Input is multiplied by all coefficients and added to various partial products


## FILTER STRUCTURE COMPARISON
- Same amount of computation, but different use of memory. Output can be available after a single multiply/add


## STRUCTURES FOR IIR?
$$
\begin{align*}
H(z)&=\frac{Y(z)}{X(z)}\\
&=\frac{b_0+b_1z^{-1}+b_1z^{-2}+b_3z^{-3}}{1+a_1z^{-1}+a_2z^{-2}+a_3z^{-3}}
\end{align*}
$$
- To make block diagram, consider as cascade of two systems:
$$
\begin{align*}
H_1(z)&=b_0+b+b_2z^{-2}+b_3z^{-3}\\
H_2(z)&=\frac{1}{1+a_1z^{-1}+a_2z^{-2}+a_3z^{-3}}
\end{align*}
$$


## IIR: DIRECT FORM 1
$$
\begin{align*}
H(z)&=\frac{Y(z)}{X(z)}\\
&=\frac{b_0+b_1z^{-1}+b_1z^{-2}+b_3z^{-3}}{1+a_1z^{-1}+a_2z^{-2}+a_3z^{-3}}
\end{align*}
$$

- __Not canonical__: number of delay elements is $$>$$ order of filter (6 vs. 3)


## DIRECT FORM 2 STRUCTURE
- can do either
	- former
	- or latter
- Middle section doesn't have to be doen twiece...
- \# of delays = order
- This is canonical!


## IIR STRUCTURES
-  Possible to do “transpose” of both IIR structures
- Can extend all 4 direct forms to ANY ORDER — just increase length of delay line
- When might you want to use a form that is not direct?
	- Build out of multiple standard second order components
	- Optimization for reduced multiplies or adds
	- Dynamic range control within filter stages
	- Coefficient range control
- MANY different block diagram structures will implement the same difference equation


## CASCADING: SERIES REALIZATION
$$
\begin{align*}
H(z)&=\frac{\left(P_1(z)\right)\left(P_2(z)\right)\left(P_3(z)\right)}{\left(D_1(z)\right)\left(D_2(z)\right)\left(D_3(z)\right)}\\
&=\left(\frac{P_1(z)}{D_1(z)}\right)\left(\frac{P_2(z)}{D_2(z)}\right)\left(\frac{P_3(z)}{D_3(z)}\right)\\
&=p_0\prod_{k}{\left(\frac{1+\beta_{1k}z^{-1}+\beta_{2k}z^{-2}}{1+\alpha_{1k}z^{-1}+\alpha_{2k}z^{-2}}\right)}
\end{align*}
$$
- This allows one to make higher order filters out of second order blocks!
- Need to factor numerator and denominator to get list of poles and zeros.
- Goal is to strategically group poles and zeroes. Mathematically, this makes no difference, but may make difference for finite dynamic range.
