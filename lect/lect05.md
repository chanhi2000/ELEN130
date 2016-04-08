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
- Sampling/Alisainsg
- Discrete-Time Systems
- LInearity


## LINEAR TIME-INVARIANT (LTI) SYSTEM
- __LTI system__: A system satisfying both the linearity property and the time-invariance property
- Mathematically easy to analyze, characterize and design
- Highly useful signal processing algorithms have been developed utilizing this class of systems over the last several decades


## CASUAL SYSTEM
Recall from [lecture 1]() : $$x[n]=0$$ for $$n<N_1$$—this is called a __causal__ signal
- A causal system, the  $$n_0$$-th output sample, $$y[n_0]$$ depends only on the input samples $$x[n]$$ for $$n\leq{n}_0$$.
- In a causal system changes in the output samples do NOT precede changes in the input
samples.
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


## PAIR AND SHARE: IMPULSE RESPONSE EXAMPLE
- What is the impulse response of this system
$$
y[n]=x[n]+0.5x[n-1]
$$
- What is the impulse response of this system
$$
y[n]=x[n]+0.5y[n-1]
$$
- What is the impulse response of the accumulator?


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
![fig05a](lect04/lect04-fig05a.png)

### Parallel connection
- Two LTI discrete-time systems in parallel
- These combine in the following way:
![fig05a](lect04/lect04-fig05a.png)


## FINITE-DIMENSIONAL LTI DISCRETE-TIME SYSTEMS
- A very important subclass of LTI discrete-time systems are characterized by a linear constant coefficient difference equation of the form
$$
\sum_{k=0}^{N}{d_ky[n-k]}=\sum_{k=0}^{M}{p_kx[n-k]}
$$
- Here, $$x[n]$$ and $$y[n]$$ are the input and output of the system, respectively.
- .$$\{d_k\}$$ and $$\{p_k\}$$ are constants characterizing the system
- The __order__ of the system is given by $$\max{(N,\:M)}$$—which is the order of the difference equation.
-
