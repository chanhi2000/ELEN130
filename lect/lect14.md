# lect14

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
- ~~Basic filter structures: All pass, LPF, band pass, HPF, comb filter, prototype LPF~~
- __Digital filter structures and representations; 2nd order building blocks__
- __FIR Design, Windowing__
- IIR Design, Bilinear transformation
- IIR filter design with MATLAB
- Review of signal acquisition and reconstruction from frequency domain perspective, interpolating filters, zero-padding, A/D and D/A converters, anti-aliasing filter, sample-and-hold, anti-imaging filter
- Multirate DSP, up-sampling and down-sampling
- Implementation considerations—quantization and dynamic range
- Specific Applications (if time)


## OVERVIEW
- Fiter structures
- Algebraic Stability Test
- Windowing


## STRUCTURE FOR FILTER IMPLEMENTATION
- __tapped delay line__:
![fig01a]()
- This is called "Direct Form"—*i.e.* coefficient of transfer function are multipliers in filter structure.
- What is this implementing?


## ANOTHER STRUCTURE FOR FILTER
- __transpose structure__:
	![fig01b]()
	- Reverse inputs and outputs
	- Reverse direction
	- Interchange adders and multipliers
- Is this the same?


## DIRECT FORM vs. TRANSPOSE STRUCTURE
- Memory of the system is $$x[n-1]$$, $$x[n-2]$$, and $$x[n-3]$$
	- Do this with memory and addressing strategy
	- FIFO, circular queue
	- Hardware support — not actual shifting
- __Input__ enters system and shifts through.
- __Output__ computed by multiplying coefficients by most recent 4 inputs and adding.
- No specific memory of input history
- Input is multiplied by all coefficients and added to various partial products.


## FILTER STRUCTURE COMPARISON
- -Same amount of computation, but different use of memory. - Output can be available after a single multiply/add


## STRUCTURE FOR IIR?
$$
\begin{align*}
H(z)&=\frac{Y(z)}{X(z)}=\frac{b_0+b_1z^{-1}+b_2z^{-2}+b_3z^{-3}}{1+a_1z^{-1}+a_2z^{-2}+a_3z^{-3}}
\end{align*}
$$
- To make block diagram, consider as cascade of two systems:
![fig02]()
$$
\begin{align*}
H_1(z)&=b_0+b_1z^{-1}+b_2z^{-2}+b_3z^{-3}\\
H_2(z)&=\frac{1}{1+a_1z^{-1}+a_2z^{-2}+a_3z^{-3}}\\
\end{align*}
$$


## IIR: DIRECT FORM I
$$
H(z)=\frac{Y(z)}{X(z)}=\frac{b_0+b_1z^{-1}+b_2z^{-2}+b_3z^{-3}}{1+a_1z^{-1}+a_2z^{-2}+a_3z^{-3}}
$$
![fig03]()
- __not canonical__: number of delay elements is larger than order of filter (__6__ vs. __3__)


## DIRECT FORM II STRUCTURE
- Can do either first (theoretically)
![fig04a]()
![fig04b]()
- # of delays = order
![fig04c]()
- This is canonical!

## DIRECT FORM II ANALYSIS
- This is simplified from the previous slide in that it is the top two stages AND the adders in the lower left and lower right have been removed (they are superfluous for order 2.)
- __Time-domain method__
$$
\begin{align*}
w[n]&=-a_1w[n-1]+x[n]\\
w[n-1]&=-a_1w[n-2]+x[n-1]\\\\
y[n]&=b_0w[n]+b_1w[n-1]\\
&=b_0\left(-a_1w[n-1]+x[n]\right)+b_1\left(-a_1w[n-2]+x[n-1]\right)\\
&=-a_1b_0w[n-1]+b_0x[n]-a_1b_1w[n-2]+b_1x[n-1]\\
&=-a_1\left(b_0w[n-1]+b_1w[n-2]\right)+b_0x[n]+b_1x[n-1]\\
&=-a_1y[n-1]+b_0x[n]+b_1x[n-1]\\
y[n]+a_1y[n-1]&=b_0x[n]+b_1x[n-1]\\\\
H(z)&=\frac{Y(z)}{X(z)}=\frac{b_0+b_1z^{-1}}{1+a_1z^{-1}}
\end{align*}
$$
- __Z-domain method__
$$
\begin{align*}
W(z)&=X(z)-a_1z^{-1}W(z)\\
W(z)\left(1+a_1z^{-1}\right)&=X(z)\\
W(z)&=\frac{X(z)}{1+a_1z^{-1}}\\\\
Y(z)&=b_0W(z)+b_1z^{-1}W(z)\\
&=\left(b_0+b_1z^{-1}\right)\left(\frac{X(z)}{1+a_1z^{-1}}\right)\\\\
H(z)&=\frac{Y(z)}{X(z)}=\frac{b_0+b_1z^{-1}}{1+a_1z^{-1}}
\end{align*}
$$
- Time-domain method is longer and requires much more cleverness.  
- Z-transform approch is much simpler.


## IIR STRUCTURES
- Possible to do “transpose” of both IIR structures
- Can extend all 4 direct forms to ANY ORDER — just increase length of delay line
- When might you want to use a form that is not direct?
	- Build out of multiple standard second order components
	- Optimization for reduced multiplies or adds
	- Dynamic range control within filter stages
	- Coefficient range control
- MANY different block diagram structures will implement the same difference equation.


## CASCADING: SERIES REALIZATION
$$
\begin{align*}
H(z)&=\frac{\left[P_1(z)\right]\left[P_2(z)\right]\left[P_3(z)\right]}{\left[D_1(z)\right]\left[D_2(z)\right]\left[D_3(z)\right]}\\
&=\left(\frac{P_1(z)}{D_1(z)}\right)\left(\frac{P_2(z)}{D_2(z)}\right)\left(\frac{P_3(z)}{D_3(z)}\right)\\
&=p_0\prod_{k}\left(\frac{1+\beta_{1k}z^{-1}+\beta_{2k}z^{-2}}{1+\alpha_{1k}z^{-1}+\alpha_{2k}z^{-2}}\right)
\end{align*}
$$
- This allows one to make higher order filters out of second order blocks!
- Need to factor numerator and denominator to get list of poles and zeros.
- Goal is to strategically group poles and zeroes. Mathematically, this makes no difference, but may make difference for finite dynamic range.
- Poles of $$H_1(z)$$ ,$$H_2(z)$$, $$H_3(z)$$ are poles of $$H(z)$$.
- Zeros of $$H_1(z)$$ ,$$H_2(z)$$, $$H_3(z)$$ are zeros of $$H(z)$$.
![fig05]()


## PARALLEL REALIZATION
$$
H(z)=\gamma_0+\sum_{k}\left(\frac{\gamma_{0k}+\gamma_{1k}z^{-1}}{1+\alpha_{1k}z^{-1}+\alpha_{2k}z^{-2}}\right)
$$
- Need to factor denominator and do partial fractions to obtain numerator polynomials
![fig06]()
- Poles of $$H_1(z)$$ ,$$H_2(z)$$, $$H_3(z)$$ are poles of $$H(z)$$.
- Zeros of $$H_1(z)$$ ,$$H_2(z)$$, $$H_3(z)$$ are NOT zeros of $$H(z)$$.
- MATLAB functions can do this for you:
	- `factor`
	- `residue`


## ALGEBRAIC STABILITY TEST
- Can verify the stability from coefficients directly for second order denominator polynomial.
- Assume:
$$
\begin{align*}
D(z)&=1+a_1z^{-1}+a_2z^{-2}\\
&=\frac{z^2+a_1z^+a_2}{z^2}\\
\end{align*}
$$
- Where are the poles?
$$
z^2+(d_1)z+(d_2)=(z-\lambda_1)(z-\lambda_2)\\
\begin{cases}
d_1=-(\lambda_1+\lambda_2)\\
d_2=\lambda_1\lambda_2
\end{cases}
$$
- Recall that we need all poles inside unit circle for stability
- Thus,
$$
\begin{cases}
|\lambda_1|<1\\
|\lambda_2|<1\\
|\lambda_1\lambda_2|<1\\
\end{cases}
$$
- Solve for $$\lambda_1$$ and $$\lambda_2$$
- in general:
$$
\lambda_{1,2}=\frac{-d_1\pm\sqrt{d_1^2-4d_2}}{2}
$$
	- 2 real roots if $${d_1}^2\geq4d_2$$, and
	- 2 complex roots if $${d_1}^2<4d_2$$
- Takes some algebra... but you get to:
$$
|d_1|<1+d_2
$$


## TRIANGLE OF STABILITY
$$
\begin{align*}
|d_1|&<1+d_2;\\
|d_2|=|\lambda_1\lambda_2|&<1;\\
\end{align*}
$$
![fig07]()


## ALL PASS FILTER
$$
\begin{align*}
H(z)&=\frac{d_1+z^{-1}}{1+d_1z^{-1}}\\
y[n]+d_1y[n-1]&=d_1x[n]+x[n-1]
\end{align*}
$$
- What does this look like in a Direct Form I structure?
![fig08a]()
- This uses 2 multipliers and two delays... Can we do better?


## ALL PASS SIMILAIRY
$$
\begin{align*}
H(z)&=\frac{a-z^{-1}}{1-az^{-1}}\\\\
H_2(z)&=\frac{d_1+z^{-1}}{1+d_1z^{-1}}
\end{align*}
$$
- Showing these are both all-pass filters can be done by first negating the numerator of $$H_2(z)$$.
- This leaves:
$$
H_2(z)=\frac{-d_1-z^{-1}}{1+d_1z^{-1}}
$$
This has the same MAGNITUDE — thus, if we show this looks like $$H(z)$$ — we've shown it is an ALL-PASS.
$$
H(z)=\frac{a-z^{-1}}{1-az^{-1}}
$$
I can say $$a=-d_1$$. Thus, this becomes:
$$
H_2(z)=\frac{a-z^{-1}}{1-az^{-1}}=H(z)
$$


## ALL PASS FILTER: DIRECT FORM II
![fig08b]
- Let’s look at one more way... the Type 1A – uses only one delay, but has two multipliers
- Can manipulate this to another form


## LATTICE FILTER
- Output of signals from four adders:
![fig09]()


## BIG PICTURE: FIR MINIMIZING ITEGRAL SQURE ERROR
- Now, we can implement filters w/several structures and we have one design method to match a desired $$H_d\left(e^{j\omega}\right)$$
	1. specify $$H_d\left(e^{j\omega}\right)$$ *e.g.* Low pass, band pass, differentiator, etc.
	2. Compute IDTFT to get
	$$
	h_d[n]=\frac{1}{2\pi}\int_{-\pi}^{\pi}{H_d\left(e^{j\omega}\right)e^{j\omega{n}}d\omega}
	$$
	3. Choose an odd length for a realizable filter, $$2M+1$$
	4. Keep only $$2M+1$$ coefficients centered around $$n=0$$
	5. Shift by $$M$$ to make it causal.


## QUESTIONS:
- How do we choose M?
- For choice of M, is this the best we can do?
- How good is this — how do we quantify it?
- Does the length have to be odd?
- We will address these questions as we move forward.


## LPF REVIEW
- We have an ideal LPF such that a time domain $$\operatorname{sinc}(h_d[n])$$ has a DTFT equal to a perfect $$\operatorname{rect}$$ function in the frequency domain with a cutoff at some frequency, $$\omega_c$$.
- We have been truncating the time-domain sinc to have length of $$2M+1$$.
- How do we choose $$M$$?


## HOW DO WE CHOOSE M?
- At this point, we don’t have a good method.
- First of all — for a given $$M$$, can we do better in terms of integral squared error?
$$
\varnothing=\frac{1}{2\pi}\int_{-\pi}^{pi}{\left|H\left(e^{j\omega}\right)-H_d\left(e^{j\omega}\right)\right|^2d\omega}
$$
- Using Parseval's relationship:
$$
\begin{align*}
\varnothing&=\sum_{n=-\infty}^{\infty}{\left|h[n]-h_d[n]\right|^2}\\
&=\sum_{n=-\infty}^{-M-1}{\left|h_d[n]\right|^2}+\sum_{n=M+1}^{\infty}{\left|h_d[n]\right|^2}
\end{align*}
$$
- Cannot change this outside range $$-M$$ to $$M$$ and inside range $$-M$$ to $$M$$ — this is zero!
- So, increasing $$M$$ reduces the integral squared error, so, could put a goal of that being lower than some threshold.
- Unfortunately, this goal does not translate into other performance measures:
	- *e.g.* pass band uniformity or stop-band attenuation


## WINDOW IN THE TIME-DOMAIN
- What will the frequency domain look like?
![fig10a]()


## WINDOW IN THE TIME-DOMAIN = sinc IN FREQ. DOMAIN
![fig10b]()


## MORE MATH ON WINDOWING
$$
\begin{align*}
W_R\left(e^{j\omega}\right)&=\sum_{n=-\infty}^{\infty}{w_R(n)e^{-j\omega{n}}}\\
&=\sum_{n=-M}^{M}{e^{-j\omega{n}}}\\
&=\frac{\sin{\left(\tfrac{(2M+1)\omega}{2}\right)}}{\sin{\left(\tfrac{\omega}{2}\right)}}
\end{align*}
$$
- This is a "digital sinc"
- (We will show this is true in a HW problem)
- Now, going back to our practical LPF — we have started with an ideal LPF (infinite sinc in the time domain) and truncated it to length $$2M+1$$.
- This truncation is the same as multiplying by a time-domain “window”
- What does this look like in the frequency domain?


## PRACTICAL LPF
$$
\begin{matrix}
N=16,&\omega_c=\frac{pi}{2}
\end{matrix}
$$
- Given pervious discussion, can you explain the oscillations?
![fig11a]()


## IMPACT OF INCREASING $$M$$
- As $$M$$ increases, we convolve (in the frequency domain) by a narrower and narrower $$\operatorname{sinc}$$.
- Passband is flatter, but discontinuity at $$\omega_c$$ keeps constant error at edge, but over smaller range.
![fig11b]()


## GIBBS PHENOMENON
- Transition from pass-band to stop-band gets narrower — more sharply defined
- Stop-band attenuation does not decrease very fast $$\tfrac{1}{\sin{\left(\tfrac{\omega}{2}\right)}}$$ with frequency, max does not decrease with $$M$$.
- Can we get $$h[n]$$ from $$h_d[n]$$ in another way — *e.g.*, use a smoother window function?
- If we do, what is the cost that we already discussed?


