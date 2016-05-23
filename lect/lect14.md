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
\lambda_{1,2}=\frac{-d_1\pm\sqrt{{d_1}^2-4d_2}}{2}
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
H(z)&=\frac{d_1+z^{-1}}{1-d_1z^{-1}}\\
y[n]+d_1y[n-1]&=d_1x[n]+x[n-1]
\end{align*}
$$
- What does this look like in a Direct Form I structure?
![fig08a]()
- This uses 2 multipliers and two delays... Can we do better?


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


## MOVING AVERAGE FILTER
$$
\begin{matrix}
M=1,&\text{length}=3,&\omega_c=\tfrac{pi}{2}
\end{matrix}
$$
![fig12a]()
$$
\begin{matrix}
M=5,&\text{length}=11,&\omega_c=\tfrac{pi}{2}
\end{matrix}
$$
![fig12b]()


## IMPACT OF CONVOLVING WINDOW WITH IDEAL LPF
- When all of main lobe is in passband or all in stop band, the resulting convolution is reasonably smooth, as wc passes through mainlobe we get transition.
![fig13]()


## MEETING SPEC vs. NOT ATTAINING IDEAL RESPONSE
- It behooves us to start thinking of how to meet specifications instead of how to attain the ideal response for a certain range.
- Important specs include:
	- Pass band
	- Stop band
	- Transition band
	- Pass band deviation, $$\delta_p$$
	- Stop band deviation, $$\delta_s$$
	- Main lobe width


## OTHER WINDOW CHOICES
### TAPER WINDOW:
$$
w_T(n)=\begin{cases}
1-\frac{|n|}{M+1},&-M\leq{n}\leq{M}\\
0,&\text{otherwise}
\end{cases}
$$
![fig14]()
- What is the frequency domain of this?

### RAISED COSINE WINDOWS
- __Hanning__:
$$
\begin{matrix}
w[n]=\frac{1}{2}\left[1+\cos{\left(\frac{\pi{n}}{M}\right)}\right],&-M\leq{n}\leq{M}
\end{matrix}
$$
- __Hamming__:
$$
\begin{matrix}
w[n]=0.54+0.46\cos{\left(\frac{\pi{n}}{M}\right)},&-M\leq{n}\leq{M}
\end{matrix}
$$
- __Blackman__:
$$
\begin{matrix}
w[n]=0.42+0.5\cos{\left(\frac{\pi{n}}{M}\right)}+0.08\cos{\left(\frac{2\pi{n}}{M}\right)},&-M\leq{n}\leq{M}
\end{matrix}
$$
- All defined as $$0$$ outside of range $$-M\leq{n}\leq{M}$$.
- Will see these listed with the $$\cos$$ term (or center cos term for Blackman) as negative... that is for the range $$0\leq{n}\leq{N}$$



## WINDOW SPECIFICATIONS

| kind | Mian Lobe Width | Trnasition Band | Relative Side Lobe Level | Stop-Band Attenuation |
| :--- | :-------------: | :-------------: | :----------------------: | :-------------------: |
| Rectangular | $$\frac{4\pi}{2M+1}$$ | $$\frac{0.92\pi}{M}$$ | -13.3 dB | -21 dB |
| Hanning | $$\frac{8\pi}{2M+1}$$ | $$\frac{3.11\pi}{M}$$ | -31.5 dB | -44 dB |
| Hamming | $$\frac{8\pi}{2M+1}$$ | $$\frac{3.37\pi}{M}$$ | -42.7 dB | -53 dB |
| Blackman | $$\frac{12\pi}{2M+1}$$ | $$\frac{5.56\pi}{M}$$ | -58.1 dB | -74 dB |


## GOALS OF WINDOW SELECTION
- Narrower transition band
- Better stop-band suppression
- Design based on specs rather than closeness to ideal


## NEW APPROACH
- Specify $$\omega_p$$ and $$\omega_s$$
- Specify ripple tolerance $$\delta_p$$ and $$\delta_s$$
- Design filter that can be implemented
- Real world constraints include:
	- Choosing order
	- Choosing FIR or IIR — tradeoff for phase behavior.


## RIPPLE TOLERANCE DEFINITIONS
- If $$G\left(e^{j\omega}\right)$$ is a filter.
$$
\begin{matrix}
1-\delta_p\leq\left|G\left(e^{j\omega}\right)\right|\leq1+\delta_p,&\text{for }|\omega|\leq\omega_p\\
\left|G\left(e^{j\omega}\right)\right|\leq\delta_s,&\text{for }\omega_s\leq|\omega|\leq\pi
\end{matrix}
$$
- Can also specify as attenuation in dB
$$
\alpha_s=-20\log_{10}{\left(\delta_s\right)}\:\text{dB}
$$
- If $$\delta_s=10^{-3}$$, *i.e.*, attenation of 1000.
$$
\begin{align*}
\alpha_s&=-20\log_{10}{\left(10^{-3}\right)}\\
&=60\log_{10}{\left(10\right)}\\
&=60\:\text{dB};
\end{align*}
$$


## HOW DO WE DESIGN TO THIS SPEC?
- For IIR — use extensive experience in analog design:
- Design methodology
- Closed-form solutions
- Empirical work
- Testing table
- If $$H_a(s)=\frac{P_a(s)}{D_a(s)}$$, what is $$G(z)=\frac{P(z)}{D(z)}$$
- Need a mapping function — wish to preserve the important characteristics of $$H_a(s)$$.


## ANALOG FILTER REVIEW
- An analog filter can be described by its system function:
$$
\begin{align*}
H_a(s)&=\frac{B(s)}{A(s)}\\
&=\frac{\sum_{k=0}^{M}{\beta_ks^k}}{\sum_{k=0}^{N}{\alpha_ks^k}}
\end{align*}
$$
- Where $$\alpha_k$$ and $$\beta_k$$ are filter coefficients that are related to $$H_a(s)$$ by the Laplace Transform:
$$
H_a(s)=\int_{-\infty}^{\infty}{h(t)e^{-st}dt}
$$
- Alternatively, the LCCDE can be determined:
$$
\sum_{k=0}^{N}{\alpha_k\frac{d^ky(t)}{dt}}=\sum_{k=0}^{M}{\beta_k\frac{d^kx(t)}{dt}}
$$
where $$x(t)$$ is input signal and $$y(t)$$ is an output signal.


## ANALOG FILTER -> DIGITAL FILTER
- Each of the three characterizations on the previous slide leads to alternatives for converting the filter from analog to digital:
	- Approximation of Derivatives,
	- Impulse Invariance, and
	- Bilinear Transformation.
- The first two are limited to LPFs and limited class of BPFs — we will focus on a more general __Bilinear Transformation__ method.
- An important thing to remember: An analog filter is stable if all of the poles are in the left-half of the $$s$$-plane
- So — if this conversion is to be successful, regardless of method:
$$
\begin{matrix}
j\Omega\text{ axis of}s\text{-plane}&\leftrightarrow&\text{unit circle of }z\text{ plane}
\end{matrix}
$$
- Left-half plane (LHP) of the $$s$$-plane should map to the inside of the unit circle in the $$z$$-plane


## ASIDE: CAN AN IIR HAVE LINEAR PHASE?
- A linear-phase filter must have a system function that satisfies:
$$
H(z)=\pm{z}^{N}H\left(z^{-1}\right)
$$
(*i.e.* it must have __even__ or __odd__ symmetry)
- If this was the case for an IIR, the filter would have a mirror-image pole outside the unit circle for every pole inside the unit circle. Thus, it would be __unstable__.
- Consequently, a causal and stable IIR filter CANNOT have linear phase.
- So, when we design IIR filters, we design to a desired magnitude and accept the phase that is obtained from the methodology.


## S-PLANE to Z-PLANE
$$
\begin{matrix}
j\Omega\text{ axis of}s\text{-plane}&\leftrightarrow&\text{unit circle of }z\text{ plane}
\end{matrix}
$$
- LHP in $$s$$-plane $$\leftrightarrow$$ inside unit circle to maintain stability
- *i.e.* poles in LHP of $$H_a(s)$$ will map to inside unit circle of $$G(z)$$.
- Could sample $$h_a(t)$$, but there is a possibility of aliasing since $$H_a(j\Omega)$$ will not be strictly band-limited.
- On the $$\Omega$$-axis,
	- .$$\tfrac{\Omega_T}{2}$$ maps to $$\pi$$. And
	- .$$\tfrac{-\Omega_T}{2}$$ maps to $$\pi$$, but
- We'd like
	- .$$\Omega=-\infty$$ to map to $$\omega=-\pi$$, and
	- .$$\Omega=\infty$$ to map to $$\omega=\pi$$.


## BILINEAR TRANSFORM
- Bilinear Transform is a conformal mapping that transforms the $$j\Omega$$ axis into the unit circle in the $$z$$-plane only once, thus avoiding aliasing of frequency components. All points in the LHP of s are mapped inside the unit circle of the $$z$$-plane and all points in the RHP of $$s$$ are mapped outside the unit-circle of the $$z$$-plane.
- Bilinear transformation can be linked to the trapezoidal formula for numerical integration. Consider an analog linear filter with system function:
$$
H(s)=\frac{b}{s+a}
$$
- This system can also be characterized by the differential equation:
$$
\frac{d}{dt}y(t)+ay(t)=bx(t)
$$


## BILINEAR TRANSFORM (THE MATH)
- We can say:
$$
y(t)=\int_{t_0}^{t}{y^\prime(\tau)d\tau}+y(t_0)
$$
where $$y^\prime(t)$$ denotes the derivatives of $$y(t)$$
- The approximation of the integral in the above expression by the trapezoidal formula at $$t=nT$$ and $$t_0=nT-T$$ yields:
$$
y(nT)=\frac{T}{2}\left[y^\prime(nT)+y^\prime(nT-T)\right]+y(nT-T)
$$
- Now, the differential equation, $$\left(\tfrac{d}{dt}y(t)+ay(t)=bx(t)\right)$$, evaluated at $$t=NT$$ becomes:
$$
y^\prime(nT)=-ay(nT)+bx(nT)
$$
Plugging this into the expression above yields:
$$
\begin{align*}
y(nT)&=\frac{T}{2}\left[-ay(nT)+bx(nT)+-ay(nT-T)+bx(nT-T)\right]+y(nT-T)\\
y[n]&=\frac{T}{2}\left(-ay[n]+bx[n]-ay[n-1]+bx[n-1]\right)+y[n-1]\\
\left(1+\frac{aT}{2}\right)y[n]-\left(1-\frac{aT}{2}\right)y[n-1]&=\frac{bT}{2} \left(x[n]+x[n-1]\right)\\
\end{align*}
$$
And, the $$z$$-transform of this equation is:
$$
\begin{align*}
\left(1+\frac{aT}{2}\right)Y(z)-\left(1-\frac{aT}{2}\right)Y(z)z^{-1}&=\frac{bT}{2}\left(X(z)+X(z)z^{-1}\right)\\
H(z)&=\frac{Y(z)}{X(z)}\\
&=\frac{\left(\tfrac{bT}{2}\right)\left(1+z^{-1}\right)}{1+\tfrac{aT}{2}-\left(1-\tfrac{aT}{2}\right)z^{-1}}\\
&=\frac{b}{\underset{s}{\underline{\tfrac{2}{T}\left(\tfrac{1-z^{-1}}{1+z^{-1}}\right)}}+a}
\end{align*}
$$
- .$$T$$: step size for numerical integration for trapezoidal numerical integration to differential equation for $$H_a(s)$$ to difference equation of $$G(z)$$. NOT THE SAMPLING INTERVAL!
$$
s=\frac{2}{T}\left(\frac{1-z^{-1}}{1+z^{-1}}\right)
$$
So,
$$
G(z)=\left.H_a(s)\right|_{s=\tfrac{2}{T}\left(\tfrac{1-z^{-1}}{1+z^{-1}}\right)}
$$
- And, to reverse this,
$$
z=\frac{1+\tfrac{sT}{2}}{1-\tfrac{sT}{2}}
$$
- How does mapping treat $$j\Omega$$ axis of $$s$$-plane and left-hand side of $$s$$-plane?
- If $$s_0=\sigma_0+j\Omega_0$$ (a point in the $$s$$-plane)
$$
\begin{align*}
z&=\frac{1+\tfrac{(\sigma_0+j\Omega_0)T}{2}}{1-\tfrac{(\sigma_0+j\Omega_0)T}{2}}\\
&=\frac{\left(1+\sigma_0\tfrac{T}{2}\right)+j\Omega\tfrac{T}{2}}{\left(1-\sigma_0\tfrac{T}{2}\right)-j\Omega\tfrac{T}{2}}\\
|z|&=\frac{\left(1+\sigma_0\tfrac{T}{2}\right)^2+\left(\Omega\tfrac{T}{2}\right)^2}{\left(1-\sigma_0\tfrac{T}{2}\right)^2+\left(\Omega\tfrac{T}{2}\right)^2}
\end{align*}
$$
	- If $$\sigma_0<0$$
	$$
	\left(1-\sigma_0\frac{T}{2}\right)^2>\left(1+\sigma_0\frac{T}{2}\right)^2
	$$
	*i.e.*
	$$
	|z|^2<1
	$$
	- If $$\sigma_0=0$$
	$$
	|z|^2=1=\text{unit circle}
	$$
	- If $$\sigma_0>0$$
	$$
	\left(1-\sigma_0\frac{T}{2}\right)^2<\left(1+\sigma_0\frac{T}{2}\right)^2
	$$
	*i.e.*
	$$
	|z|^2>1
	$$
- If $$\sigma_0=0$$, $$s_0=j\Omega_0$$, and since $$|z|=1$$,
$$
\begin{align*}
z&=\frac{1+\tfrac{(\sigma_0+j\Omega_0)T}{2}}{1-\tfrac{(\sigma_0+j\Omega_0)T}{2}}\\
&=\frac{\left(1+j\Omega\left(\tfrac{T}{2}\right)\right)}{\left(1-j\Omega\left(\tfrac{T}{2}\right)\right)}\\
&=e^{j\omega_0}\\
\left(1+j\Omega_0\left(\frac{T}{2}\right)\right)&=e^{j\omega_0}\left(1-j\Omega_0\left(\frac{T}{2}\right)\right)\\
j\Omega_0\left(\frac{T}{2}+e^{j\omega_0}\left(\frac{T}{2}\right)\right)&=e^{j\omega_0-1}\\
j\Omega_0&=\frac{2}{T}\left(\frac{e^{j\omega_0-1}}{e^{j\omega_0+1}}\right)\\
&=\frac{2}{T}\left(\frac{e^{j\omega_0-1}}{e^{j\omega_0+1}}\right)\\
&=\frac{2}{T}\left(\frac{\left(j2\sin{\left(\tfrac{\omega_0}{2}\right)}\right)}{\left(2\cos{\left(\tfrac{\omega_0}{2}\right)}\right)}\right)\\
&=j\frac{2}{T}\tan{\left(\frac{\omega_0}{2}\right)};\\
\Omega_0&=\frac{2}{T}\tan{\left(\frac{\omega_0}{2}\right)};\\
\omega_0&=2\tan^{-1}{\left(\frac{\Omega_0T}{2}\right)};
\end{align*}
$$


## RELATIONSHIP BETWEEN Ω and ω
- Note:
	- .$$\Omega=0$$ maps to $$\omega=0$$
	- .$$\Omega=\tfrac{2}{T}$$ maps to $$\omega=\tfrac{\pi}{2}$$
	- .$$\Omega=\infty$$ maps to $$\omega=\pi$$
![fig14]()


## BILINEAR TRANSFORM EXAMPLE
- Design a lowpass digital filter with a $$3\:\text{dB}$$ bandwidth of $$0.2\pi$$, using the bilinear transformation applied to the analog filter:
$$
H(s)=\frac{\Omega_c}{s+\Omega_c}
$$
where $$\Omega_c$$ is the 3-dB bandwidth of the analog filter.
- The digital filter is specified to have its $$-3\:\text{dB}$$ gain at $$\omega_c=0.2\pi$$
- We use the equation:
$$
\begin{align*}
\Omega_c&=\frac{2}{T}\tan{\left(\frac{\omega}{2}\right)}\\
&=\frac{2}{T}\tan{\left(0.1\pi\right)}\\
&=\frac{0.65}{T}
\end{align*}
$$
- Thus, the analog filter has a system function:
$$
H(s)=\frac{\tfrac{0.65}{T}}{s+\tfrac{0.65}{T}}
$$
- Applying $$s=\tfrac{2}{T}\left(\tfrac{1-z^{-1}}{1+z^{-1}}\right)$$, we get:
$$
H(z)=\frac{\tfrac{0.65}{T}}{\tfrac{2}{T}\left(\tfrac{1-z^{-1}}{1+z^{-1}}\right)+\tfrac{0.65}{T}}
$$

### [EXAMPLE][]



## COMMON IIR FILTERS
### Butterworth
- Maximally flat at $$\Omega=0$$
- Monotonic in passband AND stopband

### Type I Chebyshev
- Equiripple in passband
- Monotonic in stopband

### Type II Chebyshev
- Monotonic in passband
- Maximally flat at $$\Omega=0$$
- Equiripple in stopband

### Elliptic
- Equiripple in both passband and stopband


## COMPARISON OF COMMON IIR FILTERS
- For identical specifications: order of Butterworth is the largest, but has monotonic behavior (maximally flat). Order of Elliptic is smallest, but has ripples in passband and stopband and has sharp phase changes.
- Need to use tools to design filters:
	- Start with specs
	- Estimate filter order needs
	- Use specs and filter order to get filter coefficients
	- Test result to see if it I meets specs
- Iterative process


## BUTTERWORTH FILTER (MATLAB example)
```matlab
>> [num, den] = butter(8, 0.5)

num =
	0.0093    0.0741    0.2595    0.5190    0.6487    0.5190    0.2595    0.0741    0.0093
den =
	1.0000    -0.0000   1.0609    -0.0000   0.2909   -0.0000    0.0204    -0.0000   0.0002
>> [H, w] = freqz(num, den, 1024, 'whole');
>> plot(w-pi, fftshift(abs(H)));
```
![fig15a]()
```matlab
>> [num, den]=butter(7, 0.3)

num =
	0.0093    0.0741    0.2595    0.5190    0.6487    0.5190    0.2595    0.0741    0.0093
den =
	1.0000    -0.0000   1.0609    -0.0000   0.2909   -0.0000    0.0204    -0.0000   0.0002
>> [H, w] = freqz(num, den, 1024, 'whole');
>> plot(w-pi, fftshift(abs(H)));
```
![fig15b]()


## TYPE I CHEBYSHEV FILTERS AND MATLAB example
- All-pole filters that exhibit equiripple behavior in the passband and a monotonic characteristic in the stopband.
- MATLAB script `cheby1`:
```matlab
[num, den] = cheby1(N, R, Wp)
```
	- `N`-order,
	- `R`-passband ripple in dB,
	- `Wp`is passband frequency over $$\pi$$.
- `[num, den] = cheby1(8, 1, 0.5)`
![fig16a]()
- `[num, den] = cheby1(8, 0.5, 0.5)`
![fig16b]()


## TYPE I CHEBYSHEV FILTERS AND POLE-ZERO PLOT
- `[num, den] = cheby1(8, 1, 0.5)`
![fig17a]()
![fig17b]()


## TYPE II CHEBYSHEV FILTERS AND MATLAB example
- Monotonic in passband (maximally flat) @ $$\Omega=0$$ and equiripple in stopband
- MATLAB script `cheby2`:
```matlab
[num, den] = cheby2(N, R, Wst)
	- `N`-order,
	- `R`-stopband ripple attenuation,
	- `Wst` is stopband edge frequency over $$\pi$$.
- `[num, den] = cheby2(8, 20, 0.5)`
![fig18a]()
- `[num, den] = cheby2(8, 40, 0.5)`
![fig18b]()


## TYPE II CHEBYSHEV FILTERS AND POLE-ZERO PLOT
- `[num, den] = cheby1(8, 20, 0.3)`
![fig19a]()
![fig19b]()


## ELLIPTIC FILTERS AND MATLAB example
- Equiripple in passband and stopband
- MATLAB script `ellip`:
```matlab
[num, den] = ellip(N, Rp, Rs, Wp)
```
	- `N`-order,
	- `Rp`-peak-to-peak passband ripple,
	- `Rs`-stopband ripple attenuation,
	- `Wp` is passband-edge frequency over $$\pi$$.
- `[num, den] = ellip(8, 1, 20, 0.5)`
![fig20a]()
- `[num, den] = ellip(8, 0.5, 40, 0.5)`
![fig20b]()


## ELLIPTIC FILTERS AND POLE-ZERO PLOT
- `[num, den] = ellip(7, 0.5, 20, 0.3)`
![fig21a]()
![fig21b]()


## HIGH PASS, BAND PASS, AND BAND STOP?
- Thus far, we’ve just designed for low pass filters... Can we use this for highpass?
	- Yes — `butter`, `cheby1`, `cheby2`, and `ellip` – for all of them, just add ‘high’ to the end.
- What about for bandpass? Or band stop?
	- __Bandpass__: when you enter the passband edge (or stopband edge), instead, enter a two point vector that gives the range of the pass frequencies.
	- __Bandstop__: same as bandpass, then add the parameter, `stop`
	- The above works for `butter`, `cheby1`, `cheby2` and `ellip`.


## GENERAL METHOD FOR IIR DESIGN
- Define specs: $$\omega_p$$, $$\omega_s$$, $$\delta_p$$, $$\delta_s$$
- Convert frequencies to $$s$$ with
$$
\Omega_0=\frac{2}{t}\tan{\left(\frac{\omega_0}{2}\right)}
$$
- Design filter in $$s$$ of desired order
- Convert to $$z$$:
$$
G(z)=\left.H_a(s)\right|_{s=\tfrac{2}{T}\left(\tfrac{1-z^{-1}}{1+z^{-1}}\right)}
$$
- Let $$T=2$$ for since it does not matter when we start with $$\omega_0$$


## DESIGN IN MATLAB
```matlab
[N, W?] = *****ord(Wp, Ws, Rp, Rs)
```
- returns the order `N` of the lowest order digital Butterworth filter which has a passband ripple of no more than `Rp` dB and a stopband attenuation of at least `Rs` dB. `Wp` and `Ws` are the passband and stopband edge frequencies, normalized from 0 to 1 (where 1 corresponds to $$\pi$$ radians/sample). Also returns `W?` – the frequency to use to achieve the results.
- `buttord`
- `cheb1ord`
- `cheb2ord`
- `ellipord`


## FREQUENCY MAPPING: $$s$$-DOMAIN
- Low frequency filters can be mapped to other types of filters by mapping $$s$$:
- __LOWPASS to LOWPASS__:
$$
\begin{matrix}
s&\to&\frac{\Omega_p}{\Omega_p^\prime}s
\end{matrix}
$$
- __LOWPASS to HIGHPASS__:
$$
\begin{matrix}
s&\to&\frac{\Omega_p\Omega_p^\prime}{s}
\end{matrix}
$$
- __LOWPASS to BANDPASS__:
$$
\begin{matrix}
s&\to&\Omega_p\frac{s^2+\Omega_l\Omega_u}{s(\Omega_u-\Omega_l)}
\end{matrix}
$$
- __LOWPASS to BANDSTOP__:
$$
\begin{matrix}
s&\to&\Omega_p\frac{s(\Omega_u-\Omega_l)}{s^2+\Omega_l\Omega_u}
\end{matrix}
$$
- Then, in each case, $$s$$ gets mapped to $$z$$.


## FREQUENCY TRANSFORMATION: DIGITAL DOMAIN
- The mapping $$z^{-1}\to{g}(z^{-1})$$ must map points inside the unit circle in the $$z$$-plane into self.
- The unit circle must also be mapped into itself.
- Condition 2 implies that for $$r=1$$,
$$
\begin{align*}
e^{-j\omega}&=g\left(e^{-j\omega}\right)\\
&\equiv{g}(\omega)=|g(\omega)|e^{j\angle[g(\omega)]}
\end{align*}
$$
- So, it is clear that we must have
$$
\begin{matrix}
|g(\omega)|=1,&\forall\omega
\end{matrix}
$$
- Thus, we must create an all pass filter of the form:
$$
\begin{align*}
g\left(z^{-1}\right)&=\pm\prod_{k=1}^{n}{\frac{z^{-1}-a_k}{1-a_kz^{-1}}},&&|a_k|<1
\end{align*}
$$
to ensure that a stable filter is transformed into another stable filter (satisfy the first bullet point above).
- Using this, a number of $$z$$-transformations can be derived
- __LOWPASS to LOWPASS__:
$$
\begin{matrix}
z^{-1}&\to&\frac{z^{-1}-a}{1-az^{-1}};
\end{matrix}
$$
where,
$$
a=\frac{\sin{\left(\frac{\omega_p-\omega^\prime_p}{2}\right)}}{\sin{\left(\frac{\omega_p+\omega^\prime_p}{2}\right)}}
$$
- __LOWPASS to HIGHPASS__:
$$
\begin{matrix}
z^{-1}&\to&-\frac{z^{-1}+a}{1+az^{-1}};
\end{matrix}
$$
where,
$$
a=\frac{\cos{\left(\frac{\omega_p+\omega^\prime_p}{2}\right)}}{\cos{\left(\frac{\omega_p-\omega^\prime_p}{2}\right)}}
$$
- __LOWPASS to BANDPASS__:
$$
\begin{matrix}
z^{-1}&\to&-\frac{z^{-2}-a_1z^{-1}+a_2}{a_2z^{-2}-a_1z^{-1}+1};
\end{matrix}
$$
where,
$$
\begin{align*}
a_1&=\frac{2\alpha{K}}{K+1};\\
a_2&=\frac{K-1}{K+1};\\
\alpha&=\frac{\cos{\left(\frac{\omega_u+\omega_l}{2}\right)}}{\cos{\left(\frac{\omega_u-\omega_l}{2}\right)}};\\
K&=\cot{\left(\frac{\omega_u-\omega_l}{2}\right)}\tan{\left(\frac{\omega_p}{2}\right)};
\end{align*}
$$


## EQUIRIPPLE WITH FIR FILTER WITH LINEAR PHASE
- Approximation of ideal response w/windowed finite duration causal filter did NOT give
equiripple — *e.g.* for rectangular window got ideal convolved with $$\tfrac{\sin{\left(\tfrac{N\omega}{2}\right)}}{\sin{\left(\tfrac{\omega}{2}\right)}}$$. $$N$$ is window length, ripples dicrease at $$\tfrac{1}{\sin{\left(\tfrac{\omega}{2}\right)}}$$.
- Instead of minimizing integral squared error, minimize the peak absolute value of the
weighted error — iteratively adjusting filter parameters to get equiripple filter.
- The error we are minimizing is:
$$
\begin{matrix}
\epsilon=\int{\left|W\left(e^{j\omega}\right)\left(G\left(e^{j\omega}\right)-D\left(e^{j\omega}\right)\right)\right|^pd\omega;},&\forall{p}>0\in\mathbb{I}
\end{matrix}
$$
- The __Parks-McClellan Algorithm__ (a variation of the Remez exchange algorithm) uses the “Chebyshev approximation” to minimize the error in the passband and stopband.


## PARKS-MCCLELLAN in MATLAB
1. Order (`N`)
2. Vector of pairs of frequencies (`F`)
3. Vector of amplitude at end poitns of pairs (`A`)
- So, for example, a low pass filter, of order $$8$$, with a gain of $$1$$ for frequencies $$\omega=0:0.4$$ and a gain of $$0$$ for frequencies $$\omega=0.5:1$$ would have the following assignments:
```matlab
N=8;
F=[0, 0.4, 0.5, 1];
A=[1, 1, 0, 0];
```
- And, `h=firpm(N, F, A)`, will creaate a LINEAR PHASE FIR as close to these characteristics as possible.
- Desired response is line connecting $$\left(F(k),\:A(k)\right)$$ to $$\left(F(k+1),\:A(k+1)\right)$$ for $$k$$ odd. Region between $$f(k+1)$$ and $$f(k+2)$$ is a transition.


## `firpm` EXAMPLE
```
F=[0, fp, fs, 1];
A=[1, 1, 0, 0];
```
![fig22]()

## `firpm` — WORTH NOTHING.
- 'Hilbert' and 'differentiator' options allow for `firpm` to have odd symmetry and create Hilbert and differentiator functions


## `firpmord` FUNCTION
- Like the IIR filters, a MATLAB function exists that will determine the order needed to meet certain parameters: firpmord – here is the MATLAB help page:
- `firpmord` Parks-McClellan optimal equiripple FIR order estimator.
```matlab
[N, Fo, Ao, W] = firpmord(F, A, DEV, Fs)
```
finds the approximate order `N`, normalized frequency band edges `Fo`, frequency band magnitudes Ao and weights `W` to be used by the FIRPM function as follows:
```matlab
B = FIRPM(N, Fo, Ao, W)
```
<`W` weights the error in each band allowing you to prioritize>

The resulting filter will approximately meet the specifications given by the input parameters `F`, `A`, and `DEV`.
- `F` is a vector of cutoff frequencies in Hz, in ascending order between 0 and half the sampling frequency `Fs`. If you do not specify `Fs`, it defaults to 2.
- `A` is a vector specifying the desired function's amplitude on the bands defined by `F`. The length of `F` is twice the length of `A`, minus 2 (it must therefore be even). The first frequency band always starts at zero, and the last always ends at `Fs`/2. It is not necessary to add these elements to the `F` vector.
- `DEV` is a vector of maximum deviations or ripples (in linear units) allowable for each band. `DEV` must have the same length as `A`.


## `firpmord` — THINGS TO NOTE
- `F` are actual frequencies and `Fs` is the sampling frequency.
- length of `F` is `2*length(A)-2`


## REVIEW THE A->D->filter->A CHAIN
![fig23]()


## SAMPLING: FREQUENCY DOMAIN PERSPECTIVE
- DT signals are often produced by sampling a CT signal
- Identical DT signals may result from the sampling of more than one distinct CT signal
- We discussed that if we sample at twice the maximum frequency, we can get back the intended signal.
- Let’s refresh our memories regarding what happens in the frequency domain when we sample...
- Assume $$g_a(t)$$ is a CT signal that is sampled at $$t=nT$$, generating the sequence $$g[n]$$, where
$$
g[n]=g_a(nT)
$$
- Once again, $$T$$ is the sampling period and $$F_T$$ is the sampling frequency.
- Frequency domain represenation of $$g_a(t)$$ is given by CTFT:
$$
G_a(j\Omega)=\int_{-\infty}^{\infty}{g_a(t)e^{-j\Omega{t}}dt}
$$
- And, the frequency domain representation of $$g[n]$$ is given by DTFT:
$$
G\left(e^{j\omega}\right)=\sum_{n=-\infty}^{\infty}{g[n]e^{-j\omega{n}}}
$$
- Next, define the periodic impulse train, $$p(t)$$ as:
$$
p(t)=\sum_{n=-\infty}^{\infty}{\delta(t-nT)}
$$
such that, $$g_a(t)p(t)=g_p(t)$$ (this is still a time domain signal with deltas that have 'sampled' the original sequence.)
- .$$p(t)$$ consists of a train of ideal impulses, with period $$T$$.
- The multiplication yields:
$$
\begin{align*}
g_p(t)&=g_a(t)p(t)\\
&=\sum_{n=-\infty}^{\infty}{g_a(nT)\delta(t-nT)}
\end{align*}
$$


## SHAH FUNCTION: TRAIN OF IMPULSES
- The shah function is defined as:
$$
Ш_T(t)=\sum_{n=-\infty}^{\infty}{\delta(t-nT)}
$$
where $$T$$ is some interval spacing (conveniently labeled as the sampling period is a useful separation for these deltas).
- The CTFT of the shah function is... the shah function... well, close:
$$
\begin{align*}
Ш_{\Omega_T}(j\Omega)&=\frac{1}{T}\sum_{n=-\infty}^{\infty}{\delta\left(\Omega-\frac{2\pi{n}}{T}\right)}\\
&=\frac{1}{T} \sum_{n=-\infty}^{\infty}{\delta(\Omega-n\Omega_T)}
\end{align*}
$$


## SAMPLING: FREQUENCY DOMAIN PERSPECTIVE
- So,
$$
\begin{align*}
g_p(t)&=g_a(t)p(t)\\
=\sum_{n=-\infty}^{\infty}{g_a(nT)\delta(t-nT)}
\end{align*}
$$
- Can be rewritten as:
$$
g_p(t)=g_a(t)Ш_T(t)
$$
- And
$$
G_p(j\Omega)=G_a(j\Omega)\otimesШ_{\Omega_T}(j\Omega)
$$
- This means that sampling in the time domain at interval $$T$$ results in (scaled) REPLICATION in the frequency domain at interval $$\tfrac{1}{T}$$!!!
- Thus, $$G_p(j\Omega)$$ is a periodic function of $$\Omega$$ consisting of a sum of shifted and scaled replicas of $$G_a(j\Omega)$$, shifted by integer multiples of $$\Omega_T$$ and scaled by $$\tfrac{1}{T}$$.
- The frequency range:
$$
-\frac{1}{2}\Omega_t\leq\Omega\leq\frac{1}{2}\Omega_T
$$
is called the __baseband__ or __Nyquist band__.
- So, if $$\Omega$ is twice the highest frequency, there will be no aliasing (*i.e.* no disturbance to the baseband spectrum) and the original signal can be recovered exactly by passing the sampled signal through an ideal lowpass filter with a gain of $$T$$ and a cutoff frequency greater than the maximum frequency of the original signal and less than $$\Omega_T$$ __minus the maximum original frequency__.


## SAMPLING RATE ALTERATION
- Used to generate a new sequence, $$y[n]$$, with a sampling rate $${F_T}^\prime$$ higher or lower than that of the original sampling rate $$F_T$$ of a given sequence $$x[n]$$.
$$
R=\frac{{F_T}^\prime}{F_T}
$$
	- If $$R>1$$, the process is called __interpolation__ — meaning, we are interpolating between samples to determine the new samples
	- If $$R<1$$, the process is called __decimation__ — meaning, we are decimating samples that previously existed


## UPSAMPLING
- An integer $$L>1$$ describes upsampling as $$L-1$$ equidistant zero-valued samples are inserted by an up-sampler between each set of two consecutive samples of the input sequence.
- Thus,
$$
x_u[n]=\begin{cases}
x\left[\frac{n}{L}\right],&n=0,\:\pm{L},\:\pm2L,\cdots\\
0,&\text{otherwise}
\end{cases}
$$
![fig24a]()


## DOWNSAMPLING
- An integer $$M>1$$ describes downsampling as every $$M$$-th sample of the input sequence being kept and $$M-1$$ samples between them being removed.
- Thus,
$$
x_d[n]=x[nM]
$$
![fig24b]()


## FACTOR-OF-n INTERPOLATOR (LINEAR)
### FACTOR OF 2 INTERPOLATOR
$$
y[n]=x_u[n]+\frac{1}{2}\left(x_u[n-1]+x_u[n+1]\right)
$$
### FACTOR OF 3 INTERPOLATOR
$$
y[n]=x_u[n]+\frac{1}{3}\left(x_u[n-2]+x_u[n+2]\right)+\frac{2}{3}\left(x_u[n-1]+x_u[n+1]\right)
$$


## LINEAR INTERPOLATION
### FACTOR OF 4 INTERPOLATOR


### FACTOR OF 2 INTERPOLATOR FOR IMAGES


## A-to-D CONVERSION
- Conversion of an analog signal into digital format involves the following steps:
	- __Sampling__: Obtain samples of $$x(t)$$ at uniformly spaced time intervals.
	- __Quantization__: Map continuous-amplitude (infinite precision) samples into a finite number of amplitude levels.
	- __Coding__: Encode quantized (finite precision) samples into digital codewords.
- An __a__nalog-to-__d__igital __c__onverter (__ADC__) incorporates all these functions to carry out digital conversion
	- The ADC is preceded by an anti-aliasing filter to prevent the detrimental effect of aliasing produced in the sampling process


## QUANTIZATION
- A __quantizer__ maps analog input samples into one of a finite set of prescribed amplitudes (“levels”). It is a non-linear operation and non-invertible process
$$
y[n]=Q(x[n])
$$
where $$y[n]$$ is quantized sample corresponding to input sample $$x[n]$$
- If the ADC utilizes m bits to represent a sample, the number of distinct approximation levels is $$M=2^m$$
- The peak-to-peak (__full-scale__) amplitude range of the quantizer input is partitioned by the quantizer into $$M$$ __partition intervals__
- The $$k$$-th interval $$I_k$$ is determined by the __partition levels__ $$x_k$$ and $$x_{k+1}$$.*i.e.*,
$$
\begin{matrix}
I_k=\left\{x_k<x[n]\leq{x}_{k+1}\right\},&k=1,\:2,\:\cdots,\:M
\end{matrix}
$$
- The quantizer maps all input samples in the partition interval $$I_k$$ into some amplitude $$y_k$$, called the __quantization level__.
- The spacing between adjacent partition levels is called the __step size__
![fig25]()
- If the full-scale range of the quantizer is divided into equal-length partition intervals, the quantizer is called a __uniform__ quantizer
- The quantization step size $$\Delta$$ for a uniform quantizer with $$2V$$ full-scale (FS) range is given by
$$
\Delta=\frac{2V}{M}=\frac{2V}{2^{m}}
$$
- Quantizers can be defined with either uniformly or nonuniformly spaced partition levels.
Uniform quantization is normally used in DSP applications
- However, in digital transmission and storage applications of signals such as speech, nonlinear and time-variant quantizers are frequently used


## TWO TYPES OF 8-LEVEL UNIFORM QUNATIZER


## QUANTIZATION NOISE
- The quantization process introduces __quantization error__
$$
e[n]=y[n]-x[n]
$$
- For a uniform quantizer with step size $$\Delta$$, the quantization error is always in the range
$$\left(-\tfrac{\Delta}{2},\:\tfrac{\Delta}{2}\right]$$ assuming, $$-V\leq{x}[n]\leq{V}$$
![fig27]()
- We model quantization error as an additive noise


## NON-UNIFORM QUNATIZATION
- *Real audio signals* — *i.e.*, speech and music — contain both small and large amplitudes, but small amplitudes more likely
- Since the quantization noise for a uniform quantizer is independent of the signal amplitude, the SQNR is degraded for low signal levels
- In non-uniform quantization, we employ a relatively small step size $$\Delta$$ for low level signals and large $$\Delta$$ for larger signal levels
	- Provides relatively constant SQNR over a wide dynamic input signal range using the same number of bits
- Accomplished by the combination of a __compressor__ and an __expander__ — called a __compander__
	- __Compressor__ compresses input samples prior to uniformly quantizing at the transmit end.
	- __Expander__ restores samples to correct relative values.



## $$\mu$$-LAW QUANTIZER
![fig28]()
- Compression characteristics ($$\mu$$-law)
$$
\begin{align*}
y&=F(x)\\
&=x_\text{max}\frac{\log_{e}{\left(1+\mu\tfrac{|x|}{x_\text{max}}\right)}}{\log_{e}{\left(1+\mu\right)}}\operatorname{sgn}(x)
\end{align*}
$$
- In the United States, Canada, and Japan, $$\mu=255$$ is standard



## D-to-A CONVERSION
- Sample and hold


## ANTI-ALIASING FILTER
- Removes out-of-band frequency content BEFORE sampling takes place to avoid aliasing.
- In theory, this removes all of the things that we have been worried about in terms of aliasing. It doesn’t make aliasing a non-issue — you can still have unintended aliasing or things that come AFTER the anti-aliasing filter that cause problems.


## ANTI-IMAGING FILTER
- This is another word for the reconstruction filter as it blocks out anything that may introduce out-of-band frequency content on the reconstruction side.
![fig29]()


## MULTIRATE DIGITAL SIGNAL PROCESSING
- It is often necessary to change the sampling rate of a signal:
	– Match a different rate signal
	– Lower sampling rate to match lower bandwidth
	– Increase sampling rate before time division multiplexing
- One possibility is to put the signal through a D/A and then resample
	– __Advantage__: allows any new sampling rate
	– __Disadvantages__: distortion caused by D/A and quantization effects in A/D
- Sampling rate conversion in digital domain avoids these disadvantages
- Math of the second method can be considered as: “resampling after reconstruction”
- Let $$x(t)$$ be a CT signal that is sampled at a rate $$F_x=\tfrac{1}{T_x}$$ to generate discrete samples $$x(nT_x)$$. From the samples, a CT signal can be re-generated using the interpolation formula:
$$
y(t)=\sum_{n=-\infty}^{\infty}{x(nT_x)g(t-nT_x)}
$$
- In theory, if the bandwidth of $$x(t)$$ is less than $$\tfrac{F_x}{2}$$ and the interpolation function is given by:
$$
\begin{align*}
g(t)&=\frac{\sin{\left(\tfrac{\pi{t}}{T_x}\right)}}{\left(\tfrac{\pi{t}}{T_x}\right)};\\
G(f)&=\begin{cases}
T_x,&|f|\leq\tfrac{F_x}{2}\\
0,&\text{otherwise}
\end{cases}
\end{align*}
$$
- Then, $$y(t)=x(t)$$. If the above is not true, then $$y(t)\neq{x}(t)$$.
- In practice, perfect recovery of $$x(t)$$ is not possible.
- For our purposes, this is a tool to understand the alteration of the sampling rate.
- To perform sampling rate conversion, evaluate $$y(t)$$ (from the previous slide) at time instants $$t=mT_y,$$ where $$F_y=\tfrac{1}{T_y}$$ is the desired sampling frequency...
- Therefore — general formula for sampling rate conversion is:
$$
y(mT_y)=\sum_{n=-\infty}^{\infty}{x(nT_x)g(mT_y-nT_x)}
$$
- This is accurate only if $$F_y>F_x$$.
	- If $$F_y<F_x$$, frequency components above  $$\tfrac{F_y}{2}$$ need to be filtered out before resampling — *i.e.*, the new sampling rate cannot violate the Nyquist limit.
- Let’s look at the sampling chain to understand this better.
![fig30]()




