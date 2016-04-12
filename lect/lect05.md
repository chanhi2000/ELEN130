# lect05

## COURSE OVERVIEW: PART 1
- ~~Discrete-Time Signals in the Time Domain~~
	- ~~Operations, Classifications, Sampling~~
- __Discrete-Time Systems__
	- __Impulse/Step Responses, LTI Classification, Stability, Response to sinusoidal input__
- Discrete-Time Signals in the Frequency Domain
	- Transforms, Applications, Sampling and reconstruction
- Finite-Length Discrete Transforms
	- DFT, FFT, Zero-padding, Fourier Domain filtering, Linear and Circular convolution
- Z-transform
- Basic filter structures: All pass, LPF, band pass, HPF, comb filter, prototype LPF
-Digital filter structures and representations; 2nd order building blocks
- FIR Design, Windowing
- IIR Design, Bilinear transformation
- IIR filter design with MATLAB
- Review of signal acquisition and reconstruction from frequency domain perspective, interpolating filters, zero-padding, A/D and D/A converters, anti-aliasing filter, sample-and-hold, anti-imaging filter
- Multirate DSP, up-sampling and down-sampling
- Implementation considerations—quantization and dynamic range
- Specific Applications (if time)


## OVERVIEW
- Linearity, Shift-Invariance/Time-Invariance 
- Cascaded Systems
- Fourier Transform


## LINEAR TIME-INVARIANT (LTI) SYSTEM
- __LTI system__: A system satisfying both the linearity property and the time-invariance property
- Mathematically easy to analyze, characterize and design
- Highly useful signal processing algorithms have been developed utilizing this class of systems over the last several decades


## CASUAL SYSTEM
Recall from [lecture 1]() : $$x[n]=0$$ for $$n<N_1$$—this is called a __causal__ signal
- A causal system, the  $$n_0$$-th output sample, $$y[n_0]$$ depends only on the input samples $$x[n]$$ for $$n\leq{n}_0$$.
- In a causal system changes in the output samples do NOT precede changes in the input samples.
- A non-causal system CAN be implemented, but, it requires delaying the output by an appropriate number of samples.


## STABILITY
- Bounded Input, Bounded Output Stability
	- If $$y[n]$$ is the response to an input $$x[n]$$ and if $$|x[n]|\leq{B}_x$$ for all values of $$n$$, then $$|y[n]|\leq{B}_y$$ for all values of $$n$$.
- Convince yourself that this is easy to verify for a case like the moving filter.


## PASSIVE AND LOSSLESS SYSTEMS
### PASSIVE:
$$
E_y\leq{E}_x<\infty
$$
### LOSSLESS:
$$
E_y=E_x
$$


## IMPULSE AND STEP RESPONSES
- The response of a discrete-time system to a unit sample sequence, $$\delta[n]$$, is called the unit sample response, or simply, the impulse response, and is denoted by $$h[n]$$.
- The response of a discrete-time system to a unit sample sequence, $$\mu[n]$$, is called the unit step response, or simply, the step response, and is denoted by $$s[n]$$.

### [EXAMPLE: IMPULSE RESPONSE][1]


## LTI DISCRETE-TIME SYSTEM
- The consequence of the LTI properties is that an LTI discrete-time system is completely characterized by its impulse response
- This means that knowing the impulse response, one can compute the output of the system for ANY ARBITRARY INPUT!!!

### EXAMPLE
Say
$$
x[n]=\delta[n-2]+3\delta[n-1]-\frac{1}{2}\delta[n+3]
$$
Since the system is linear, we can figure out individual outputs and add them up.

so
$$\left\{
\begin{align*}
\delta[n-2]&\to{h}[n-2]\\
3\delta[n-1]&\to3{h}[n-1]\\
-\frac{1}{2}\delta[n+3]&\to-\frac{1}{2}{h}[n+3]\\
\end{align*}\right\}
$$

$$
\therefore\:y[n]=h[n-2]+3h[n-1]-\frac{1}{2}h[n+3]
$$

- NOW—think about the fact that any input can be written as a sum of weighted impulses.
- So, the input can be expressed as a linear combination of delayed and advanced impulses in the form:
$$
x[n]=\sum_{k=-\infty}{\infty}{x[k]\delta[n-k]}
$$
- So the response would be...


## CONVOLUTION PROPERTIES
### COMMUTATIVE:
$$
x[n]\otimes{h}[n]=h[n]\otimes{x}[n]
$$
### ASSOCIATIVE:
$$
\left(x[n]\otimes{h}[n]\right)\otimes{y}[n]=x[n]\otimes\left(h[n]
\otimes{y}[n]\right)
$$
### DISTRIBUTIVE:
$$
x[n]\otimes\left(h[n]+{y}[n]\right)=x[n]\otimes{h}[n]+x[n]\otimes{y}[n]
$$


## BIBO STABILITY CONDITION
- [From a few slides ago](#stability):
- Bounded Input, Bounded Output Stability
	- If $$y[n]$$ is the response to an input $$x[n]$$ and if $$|x[n]|\leq{B}_x$$ for all values of $$n$$, then $$|y[n]|\leq{B}_y$$ for all values of $$n$$.
- Now, this can be described as: BIBO stability exists for an LTI discrete-time system iff its impulse response, $$h[n]$$ is absolutely summable—that is:
$$
S=\sum_{n=-\infty}^{\infty}{|h[n]}<\infty
$$
- Proof in textbook and is conceptualized by replacing $$y[n]$$ with the convolution sum, bounding $$x$$ and looking at what must be true for $$y$$ to be bounded.


## CAUSALITY
- Earlier it was stated that a system is causal if the $$n_0$$-th output sample,      depends only on the input samples $$x[n]$$ for $$n\leq{n}_0$$. In a causal system changes in the output samples do NOT precede changes in the input samples.
- This can now be stated as: An LTI discrete-time system is causal iff its impulse response, $$h[n]$$ is a causal sequence.


## INTERCONNECTED SYSTEMS
### Cascaded connection (Series)
- Two LTI discrete-time systems in series
- These combine in the following way:
![fig01a](lect04/lect04-fig01a.png)

### Parallel connection
- Two LTI discrete-time systems in parallel
- These combine in the following way:
![fig01b](lect04/lect04-fig01b.png)


## FINITE-DIMENSIONAL LTI DISCRETE-TIME SYSTEMS
- A very important subclass of LTI discrete-time systems are characterized by a linear constant coefficient difference equation of the form
$$
\sum_{k=0}^{N}{d_ky[n-k]}=\sum_{k=0}^{M}{p_kx[n-k]}
$$
- Here, $$x[n]$$ and $$y[n]$$ are the input and output of the system, respectively.
- .$$\{d_k\}$$ and $$\{p_k\}$$ are constants characterizing the system
- The __order__ of the system is given by $$\max{(N,\:M)}$$—which is the order of the difference equation.
- An LTI system characterized by a constant coefficient difference equation (as above) as two finite sums of products.
- Assuming causality, the output can be written as:
$$
\begin{align*}
y[n]&=-\sum_{k=1}^{N}{\left(\frac{d_k}{d_0}y[n-k]\right)}+\sum_{k=0}^{M}{\left(\frac{p_k}{d_0}x[n-k]\right)}\\
&\left<\text{assuming }d_0\neq0\right>
\end{align*}
$$


## SOLVING THE LCCDE
$$
\sum_{k=0}^{N}{\left(d_ky[n-k]\right)}=\sum_{k=0}^{M}{\left(p_kx[n-k]\right)}
$$
where
$$
\begin{matrix}
y[n]=y_c[n]+y_p[n]\\
\left\{\begin{matrix}
y_c[n],&\text{complementary/homogeneous solution (no input)}\\
y_p[n],&\text{particular solution resulting from the input, }x[n]
\end{matrix}\right\}
\end{matrix}
$$

### STEP 1
Find the complementary solution, $$y_c[n]$$ by assuming it has the form: $$\lambda^n$$
so
$$
\begin{align*}
\sum_{k=0}^{N}{\left(d_ky[n-k]\right)}&=\sum_{k=0}^{N}{\left(d_k\left(\lambda^{n-k}\right)\right)}\\
&=\lambda^{n-N}\underset{\text{characteristic polynomial}}{\left(d_0\lambda^{N}+d_1\lambda^{N-1}+\cdots+d_{N-1}\lambda+d_N\right)}\\
&=0
\end{align*}
$$
Assume the above has $$N$$ roots: call them $$\lambda_1,\:\lambda_2,\:\lambda_3,\:\cdots,\lambda_N$$. If all of the roots are unique, the complementary solution is given by
$$
y_c[n]=\alpha_1\lambda_1^{n}+\alpha_2\lambda_2^{n}+\alpha_3\lambda_3^{n}+\cdots+\alpha_N\lambda_N^{n}
$$
And the constants $$\alpha_1,\:\alpha_2,\cdots,\alpha_N$$ are determined from the initial conditions. The __particular__ solution, $$y_p[n]$$, matches the form of the input, $$x[n]$$.

### [EXAMPLE: SOLVING THE LCCDE][2]


## CLASSIFICATIONS OF LTI DISCRETE-TIME SYSTEMS
- Based on impulse response length
	- __F__inite __I__mpulse __R__esponse: impulse response $$h[n]$$ has finite length
	- __I__nfinite __I__mpulse __R__esponse: impulse response $$h[n]$$ has infinite length
- Based on Output Calculation Process
	- __Nonrecursive System__: Output can be calculated sequentially knowing only the present and past input
samples.
	- __Recursive System__: Output computation involves past output samples in addition to the present and past input samples
- Based on the Impulse Response Coefficients
	- __Real Discrete-Time System__: The impulse response samples are real-valued
	- __Complex Discrete-Time System__: The impulse response samples are complex-valued


## MORE ON FINITE IMPULSE RESPONSE (FIR)
If impulse response $$h[n]$$ has finite length, then it is known as __a finite impulse response (FIR) discrete-time system__.
Assuming
$$
\begin{matrix}
h[n]=0\:\text{for}&
\begin{cases}
n<N_1,\\n>N_2,\\N_1<N_2
\end{cases}
\end{matrix}
$$
The convolution sum can be rewritten as:
$$
y[n]=\sum_{k=N_1}^{N_2}{h[k]x[n-k]}
$$
- Output of an FIR LTI discrete-time system can be computed directly from the convolution sum.
- __example__: moving-average system and linear interpolators.

### [EXAMPLE: DISCRETE-TIME ACCUMULATOR][3]


## COURSE OVERVIEW: PART 1
- ~~Discrete-Time Signals in the Time Domain~~
	- ~~Operations, Classifications, Sampling~~
- ~~Discrete-Time Systems~~
	- ~~Impulse/Step Responses, LTI Classification, Stability,~~ __Response to sinusoidal input__
- __Discrete-Time Signals in the Frequency Domain__
	- __Transforms, Applications, Sampling and reconstruction__
- Finite-Length Discrete Transforms
	- DFT, FFT, Zero-padding, Fourier Domain filtering, Linear and Circular convolution
- Z-transform
- Basic filter structures: All pass, LPF, band pass, HPF, comb filter, prototype LPF
-Digital filter structures and representations; 2nd order building blocks
- FIR Design, Windowing
- IIR Design, Bilinear transformation
- IIR filter design with MATLAB
- Review of signal acquisition and reconstruction from frequency domain perspective, interpolating filters, zero-padding, A/D and D/A converters, anti-aliasing filter, sample-and-hold, anti-imaging filter
- Multirate DSP, up-sampling and down-sampling
- Implementation considerations—quantization and dynamic range
- Specific Applications (if time)


## WHAT CAN WE DO?
- Represent a discrete-time system by:
	- Difference equation
	- Block diagram
	- Impulse response (if LTI)
- Compute impulse response
- Use convolution to compute response to any input (assuming system
is LTI
- Analysis is good... we are in need of a tool to help with design. For example—say we wanted to design:
	- A filter to remove 60Hz interference from power lines
	- Pass all signals for channel 2 and suppress others
	- Determine whether a signal is a 4 on the touch tone phone
- __FREQUENCY ANALYSIS__


## RESPONSE TO A SINUSOIDAL INPUT
- Much like an impulse response says how a system will respond to an impulse, it may be useful to know how a system will respond to a sinusoidal input.
- __Why?__ For that answer — we need to think back to Fourier analysis. A signal can be written as a sum of sinusoids at different frequencies.


## FREQUENCY DOMAIN REPRESENTATION
- What is the frequency domain? Physically
	- what does it mean and what does it represent?
- Very useful to look at signals in the frequency domain (*i.e.* it is useful to look at signals in terms of their frequency content)
![fig02](lect05/lect05-fig02.png)

- Fourier Series and Fourier Transform are two mathematical approaches to looking at a signal in the frequency domain
- The Fourier series can be used to represent __periodic__ signals in the frequency domain
- A periodic function $$x_p(t)$$ with fundamental period $$T_0$$ (where $$f_0=\tfrac{1}{T_0}$$ is called the __fundamental frequency__) can be represented by an __exponential__ Fourier series
	- It uses superposition to express a signal as the summation of an infinite number of complex exponential waveforms
	$$
	\begin{matrix}
	\tilde{x}(t)=\sum_{n=-\infty}^{\infty}{c_ne^{j2\pi{f}_0nt}}\\
	c_n=\frac{1}{T_0}\int_{0}^{T_0}{\tilde{x}(t)e^{-j2\pi{f}_0nt}dt}
	\end{matrix}
	$$
- The term $$c_0$$ represents the __DC__ component of the signal:
$$
c_0=\frac{1}{T_0}\int_{0}^{T_0}{\tilde{x}(t)dt}
$$


## FOUREIR SERIES (EXTRA INFO)
$$
\begin{matrix}
\tilde{x}(t)=\sum_{n=-\infty}^{\infty}{c_ne^{j2\pi{f}_0nt}}\\
c_n=\frac{1}{T_0}\int_{0}^{T_0}{\tilde{x}(t)e^{-j2\pi{f}_0nt}dt}
\end{matrix}
$$
$$
\left\{\begin{aligned}
n&=\pm1&&\text{the fundamental frequency component in }x_p(t)\\
n&=\pm2,\:\pm3,\:\cdots&&\text{the harmonic component in }x_p(t)
\end{aligned}\right\}
$$
- Each phasor term in FS can be written as
$$
c_ne^{j2\pi{f}_0nt}=|c_n|e^{j\left(2\pi{f}_0nt+\angle{\left(c_n\right)}\right)}
$$
- Plots of $$|c_n|$$ and $$\angle{c_n}$$ versus *discrte* frequency values $$ \left(\begin{matrix}nf_0,&n=0,\:\pm1,\:\pm2,\:\cdots\end{matrix}\right)$$ are called the __magnitude__ and the __phase line spectra__ of the signal, respectively.


## FREQUENCY DOMAIN REPRESENTATION
- start with a reveiw of the __C__ontinuous-__t__ime __F__ourier __T__ransform (__CTFT__)
- __CTFT__:
$$
\begin{matrix}
\underset{\text{CT time-doamain}}{t}&\to&\underset{\text{CT frequency-doamain}}{f}
\end{matrix}
$$
- This will let us transition smoothly into the __D__iscrete-__t__ime __F__ourier __T__ransform (__DTFT__)
- __DTFT__:
$$
\begin{matrix}
\underset{\text{DT time-doamain}}{n}&\to&\underset{\text{DF frequency-doamain}}{\omega}
\end{matrix}
$$


## FOURIER TRANSFORM
- CTFT is given by:
$$
\underset{\text{Fourier spectrum}}{X_a(j\Omega)}=\int_{-\infty}^{\infty}{x_a(t)e^{-j\Omega{t}}dt}\\
$$
- Inverse CTFT is given by:
$$
x_a(t)=\frac{1}{2\pi}\int_{-\infty}^{\infty}{X_a(j\Omega)e^{j\Omega{t}}d\Omega}
$$
- The pair is denoted as:
$$
\begin{matrix}
x_a(t)&\overset{\text{CTFT}}{\longleftrightarrow}&X_a(j\Omega)
\end{matrix}
$$
where
$$
\left\{\begin{matrix}
\underset{\text{CT angular frequency}}{\Omega}&-\infty<\Omega<\infty
\end{matrix}\right\}
$$
- polar form:
$$
X_a(j\Omega)=|X_a(j\Omega)|e^{j\theta_a(\Omega)}
$$
where
$$
\left\{\begin{matrix}
\underset{\text{magnitude}}{|X_a(j\Omega)|}\\
\underset{\text{phase}}{\theta_a(\Omega)|}\\
\end{matrix}\right\}
$$


## DIRAC DELTA FUNCTION
- The unit impulse signal $$\delta(t)$$ is defined by the equations
$$
\begin{align*}
\delta(t)&=0,&&t\neq0\\
\int_{-\epsilon}^{\epsilon}{\delta(t)dt}&=1,&&\forall{\epsilon>0},\:\:\left(\epsilon\in\Re\right)
\end{align*}
$$
![fig03](lect05/lect05-fig03.png)
- Zero everywhere except the origin
- has unit area
- The value of $$\left.\delta(t)\right|_{t=0}$$ is not defiend. In particular $$\delta(0)\neq\infty$$
- The unit impulse signal can be viewed as a narrow pulse with large amplitude and having a unit area
- Another definition:
$$
\delta(f)=\int_{-\infty}^{\infty}{e^{j2\pi{f}t}dt}
$$

## PROPERTIES OF A DIRECT DELTA FUNCTION
$$
x(t)\delta(t-t_0)=x(t_0)\delta(t-t_0)
$$
$$
\delta(\alpha{t})=\frac{1}{|\alpha|}\delta(t)
$$
- sampling (or sifting) property:
$$
\begin{align*}
\int_{-\infty}^{\infty}{x(t)\delta(t-t_0)dt}&=\int_{-\infty}^{\infty}{x(t_0)\delta(t-t_0)dt}\\
&=x(t_0)\int_{-\infty}^{\infty}{\delta(t-t_0)dt}\\
&=x(t_0)
\end{align*}
$$
- convolution property:
$$
\begin{align*}
x(t)\otimes\delta(t)&=\int_{-\infty}^{\infty}{x(\tau)\delta(t-\tau)dt}\\
&=x(t)
\end{align*}
$$

### [EXAMPLE: DIRAC DELTA][4]

### [EXAMPLE: CTFT][5]

### [EXAMPLE: INVERSE CTFT][6]

## CTFT LIMITATIONS
- __DIRICHLET CONDITIONS__
	- the CT function, $$x_a(t)$$ should have a finite number of discontiunities and a finite number of maxima and minima in any finite interval
	- The signal is absolutely integrable, *i.e.*
	$$
	\int_{-\infty}^{\infty}{|x_a(t)|dt}<\infty
	$$


[1]: http://chanhi2000.gitbooks.io/elen133/content/lect/ex05.html#1
[2]: http://chanhi2000.gitbooks.io/elen133/content/lect/ex05.html#2
[3]: http://chanhi2000.gitbooks.io/elen133/content/lect/ex05.html#3
[4]: http://chanhi2000.gitbooks.io/elen133/content/lect/ex05.html#4
[5]: http://chanhi2000.gitbooks.io/elen133/content/lect/ex05.html#5
[6]: http://chanhi2000.gitbooks.io/elen133/content/lect/ex05.html#6
