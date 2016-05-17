# lect12

## COURSE OVERVIEW: PART 1
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

### [EXAMPLE: PHASE][]

## FACTORS IN THE PROPERTIES OF FILTERS
- Zero placement
- Even/odd symmetry
- Even/odd length

## TYPE 1 FILTER
- SYMMETRY: even
- .$$N$$ is even... length $$N+1$$ is odd.
- Since $$N$$ is even, zeros occur in pairs or groups of $$4$$.
- Either no zeros at $$z=1$$ or $$z=-1$$ OR pairs of zeros at $$z=1$$ or $$z=-1$$ (*i.e.* even numbers of zeros at $$z=1$$ or $$z=-1$$)

### [EXAMPLE: TYPE 1 FILTER][]

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

### [EXAMPLE: TYPE 2 FIR FILTER][]

### [EXAMPLE: TYPE 1 FIR FILTER TIMES (z+1)][]

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

### [EXAMPLE: TYPE 3 FIR FILTER][]

## TYPE 4 FIR FILTER
- SYMMETRY: odd
- .$$N$$ is odd, length $$N+1$$ is even.
- Will have an odd number of zeros at $$z=1$$ and even number or no zeros at $$z=-1$$.
- Using similar logic as before, this cannot be a LPF.
- This is like TYPE 2 except with sines instead of cosines.

### [EXAMPLE: TYPE 4 FIR FILTER][]

## QUICK (BUT IMPORTANT) ASIDE
- FIR VS. IIR
- Linearity of phase — can you say something about this based on the filter being FIR or IIR?
- Where are the poles for an FIR filter?
- Can a filter with feedback be an FIR filter?


## SUMMARY OF FIR FILTERS
| TYPE | ORDER (N) | LENGTH | SYMMETRY | 
| :--: | :-------: | :----: | :------- |
| 1 | EVEN | ODD | EVEN |
| 2 | ODD | EVEN | EVEN (not HPF) |
| 3 | EVEN | ODD | ODD (not LPF/not HPF) |
| 4 | ODD | EVEN | ODD (not LPF) |


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
