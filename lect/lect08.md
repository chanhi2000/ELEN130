# lect08

## COURSE OVERVIEW: PART 1
- ~~Discrete-Time Signals in the Time Domain~~
	- ~~Operations, Classifications, Sampling~~
- ~~Discrete-Time Systems~~
	- ~~Impulse/Step Responses, LTI Classification, Stability, Response to sinusoidal input~~
- ~~Discrete-Time Signals in the Frequency Domain~~
	- ~~Transforms, Applications,~~ __Sampling and reconstruction__
- __Finite-Length Discrete Transforms__
	- __DFT, FFT, Zero-padding, Fourier Domain filtering, Linear and Circular convolution__
- Z-transform
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
- Frequency Response
- DFT
- Circular Convolution
- Overlap/Add and Overlap/Save


## COMPLEX EXPONENTIAL VERSION OF EXAMPLE
- Let $$x[n]=e^{j\omega_0n}$$
- Combine response for $$+\omega_0$$ and $$-\omega_0$$ to get sin and cos.
- Re-doing previous example:
$$
\begin{align*}
y[n]&=x[n]+x[n-1]+x[n-2]\\
&=e^{j\omega_0n}+e^{j\omega_0(n-1)}+e^{j\omega_0(n-2)}\\
&=e^{j\omega_0n}+e^{j\omega_0n}\left(e^{-j\omega_0}\right)+e^{j\omega_0n}\left(e^{-j2\omega_0}\right)\\
&=\underset{\text{input sinusoid}}{e^{-j\omega_0n}}\cdot\underset{\text{Scale factor that depends on }\omega_0\text{ and not }n}{\left(1+e^{-j\omega_0}+e^{-j2\omega_0}\right)}\\
\end{align*}
$$
- Simplify scale factor:
$$
\begin{align*}
\left(1+e^{-j\omega_0}+e^{-j2\omega_0}\right)&=e^{-j\omega_0}\left(e^{-j\omega_0}+1+e^{j\omega_0}\right)\\
&=e^{-j\omega_0}\left(1+2\cos{\left(\omega_0\right)}\right)
\end{align*}
$$
where
$$
\left\{\begin{aligned}
e^{-j\omega_0}:&\text{dealy of one sample}\\
\left(1+2\cos{\left(\omega_0\right)}\right):&\text{Same scaling factor as before}
\end{aligned}\right\}
$$
- Think DTFT pair and what a unit delay looks like in the frequency domain.


## GENERAL LTI SYSTEM WITH SINUSOIDAL INPUT
- Impulse response $$h[n]$$; input $$x[n]=e^{j\omega{n}}$$; convolution to find $$y[n]$$.
$$
\begin{align*}
y[n]&=h[n]\otimes{x}[n]\\
&=\sum_{k=-\infty}^{\infty}{h[k]x[n-k]}\\
&=\sum_{k=-\infty}^{\infty}{h[k]e^{j\omega(n-k)}}\\
&=\sum_{k=-\infty}^{\infty}{h[k]e^{j\omega{n}}e^{-j\omega{k}}}\\
&=e^{j\omega{n}}\sum_{k=-\infty}^{\infty}{h[k]e^{-j\omega{k}}}\\
&=e^{j\omega{n}}\left(H\left(e^{j\omega}\right)\right)\\
&=x[n]H\left(e^{j\omega}\right)
\end{align*}
$$
- So, for the complex exponential input siganl, the LTI system is a complex exponential signal of the  same frequency multiplied by a complex constant $$H\left(e^{j\omega}\right)$$.
- .$$e^{j\omega{n}}$$ is known as an eigenfunction of the system.


## FREQUENCY RESPONSE
$$
H\left(e^{j\omega}\right)
$$
- This is called the frequency response of the LTI DT system.
- This provides a frequency-domain description of the system.
$$
H\left(e^{j\omega}\right)\sum_{n=-\infty}^{\infty}{h[n]e^{-j\omega{n}}}
$$
- This is the DTFT of the impulse response, $$h[n]$$.
$$
H\left(e^{j\omega}\right)=\left|H\left(e^{j\omega}\right)\right|e^{j\theta(\omega)}
$$
- This is in form of __magnitude__ and __phase__.
- In general $$H\left(e^{j\omega}\right)$$ is a complex function of $$\omega$$ with period $$2\pi$$.
- Often, magnitude functionis specified in decibel, $$\text{dB}$$:
$$
G\left(\omega\right)=20\log_{10}{\left|H\left(e^{j\omega}\right)\right|}\:\text{dB}
$$
- This is called the __gain function__.
$$
A\left(\omega\right)=-G\left(\omega\right)
$$
- This is called the __attenuation / loss__ function.


## PLOTTING PHASE
$$
H\left(e^{j\omega}\right)=\left|H\left(e^{j\omega}\right)\right|e^{j\theta(\omega)}
$$
- Solution to previous example:
$$
e^{-j\omega_0}\left(1+2\cos{\left(\omega_0\right)}\right)
$$
- Therefore, Phase is $$-\omega_0$$
- The general form pulls out an $$e^{-j\omega_0}$$ term. Based on the first bullet, this means the phase is :
$$
\theta(\omega)=-\omega
$$
- Thus, phase is a straight line.... with a slope of $$-1$$!
- The slope of the linear phase represents the delay of the system.


## IMPORTANT POINT ABOUT PLOTTING MAG/PHASE
- magnitude should never be negative!
- Thus "negative" section need to be flipped up
- This "$$-1$$" multiplier in the magnitude plot needs to be represented in the phase...
- How do we represent this? IN other words, what phase jump $$e^{j\left<\text{what}\right>}$$ represents $$-1$$?
$$
e^{j\pi}=e^{-j\pi}=-1;
$$
- Ths -$$\pi$$ jumps (in either direction) in the phase plot represent a "$$-1$$" (or a flip) in the magnitude plot.
- Phase can also have $$2\pi$$ jumps that change nothing!


## PROPERTIES OF FREQUENCY RESPONSE
- If $$h[n]$$ is real, magnitude function is even and phase function is odd (functions of $$\omega$$)
- If $$h[n]$$ is real, $$H_\Re\left(e^{j\omega}\right)$$ is even and $$H_\Im\left(e^{j\omega}\right)$$ is odd


## FREQUENCY RESPONSE WITH MATLAB
- `Freqz`
- `Real`
- `Abs`
- `Angle`
- `Unwrap`
- Some of these commands that will be explored in HW5


## STEADY STATE RESPONSE
- Consider steady state response to a step function.
- Similarly, there is a steady state response to a constant sinusoidal signal.
- Let’s use the frequency response to find it...


## STEADY STATE RESPONSE TO SINUSOIDAL INPUT
- Assume we have a real-coefficient, LTI DT system with a frequency response of $$H\left(e^{j\omega}\right)$$ and an input,
$$
\begin{matrix}
x[n]=A\cos{\left(\omega_0n+\phi\right)}&&\forall{n}\\
\text{where}\:\:\:\:-\infty<n<\infty
\end{matrix}
$$
- We can express the input as:
$$
x[n]=\frac{1}{2}Ae^{j\phi}e^{j\omega_0n}+\frac{1}{2}Ae^{-j\phi}e^{-j\omega_0n}
$$
- If we call $$g[n]=Ae^{j\phi}e^{j\omega_0n}$$, then
$$
x[n]=g[n]+g^*[n]
$$
- Recall, the ouput of the system for an input $$e^{j\omega_0n}$$ is simply:
$$
H\left(e^{j\omega)0}\right)e^{j\omega_0n}
$$
- Thus, the output of $$g[n]$$ is given by:
$$
\frac{1}{2}Ae^{j\phi}H\left(e^{j\omega_0}\right)e^{j\omega_0n}
$$
- Likewise, the output of $$g^*[n]$$ is:
$$
\frac{1}{2}Ae^{-j\phi}H\left(e^{-j\omega_0}\right)e^{-j\omega_0n}
$$
- Combining these outputs to create $$y[n]$$ yields:
$$
\begin{align*}
y[n]&=\frac{1}{2}A\left|H\left(e^{j\omega_0}\right)\right|\left(e^{j\theta(\omega_0)}e^{j\phi}e^{j\omega_0n}+e^{-j\theta(\omega_0)}e^{-j\phi}e^{-j\omega_0n}\right)\\
&=A\left|H\left(e^{j\omega_0}\right)\right|\cos{\left(\omega_0n+\theta(\omega_0)+\phi\right)}
\end{align*}
$$
- The output has the same sinusodial waveform! Two main difference:
	- Amplitude is multiplied by the value of the magnitude function at $$\omega=\omega_0$$
	- The output has a phase lag relative to the input by an amount $$\theta(\omega_0)$$


## RESPONSE TO A CAUSAL EXPONENTIAL SEQUENCE
- In practice, it makes sense to assume the input is causal.
- From the input-output relation:
$$
y[n]=\sum_{k=-\infty}^{\infty}{h[k]x[n-k]}
$$
- For an input $$x[n]=e^{j\omega{n}}\mu[n]$$ is given by:
$$
\begin{align*}
y[n]&=\left(\sum_{k=0}^{n}{h[k]e^{j\omega(n-k)}}\right)\mu[n]\\
&=\left(\sum_{k=0}^{n}{h[k]e^{-j\omega{k}}}\right)e^{j\omega{n}}\mu[n]\\
\end{align*}
$$
- Output is
$$
y[n]=\begin{cases}
0,&\text{for }n<0\\
\underset{\text{steady state reponse}}{\left(\sum_{k=0}^{\infty}{h[k]e^{-j\omega{k}}}\right)e^{j\omega{n}}}-\underset{\text{transient response}}{\left(\sum_{k=n+1}^{\infty}{h[k]e^{-j\omega{k}}}\right)e^{j\omega{n}}}
\end{cases}
$$


## COURSE SUMMARY
- So, what can we do so far:
- __Implement__
	- a system in the time domain
- __Analyze__
	- for an LTI system, we can compute $$H\left(e^{j\omega}\right)$$ for $$h[n]$$.
	- For system parameters, we can evaluate amplitude and phase shift for any input frequency
	- Use frequency response to convert $$\text{time}\to\text{frequency}$$
- __Design__
	- Want to go from frequency specifications to time representation... we need a method to do this
	- Use DTFT and CTFT properties to help


## FILTERING
- One purpose of LTI DT system is to pass certain frequencies and to block others — *i.e.* to filter with certain properties
- The key to the filtering process is recognizing that $$x[n]$$ can be written as a weighted sum of an infinite number of exponential sequences (*i.e.* as a linear weighted sum of sinusoids)
- By appropriately choosing the values of $$\left|H\left(e^{j\omega}\right)\right|$$ of the  LTI digital filter — certain frequencies can be attenuated or filteredas desired.
- To understand the mechanism behind the design of frequency-selective filters, consider a real-coefficient LTI discrete-time system characterized by the following magnitude function:
$$
\left|H\left(e^{j\omega}\right)\right|\approx\begin{cases}
1,&|\omega|\leq\omega_c\\
0,&\omega_c<\omega<\pi
\end{cases}
$$
- Now, apply an input:
$$
\begin{matrix}
x[n]=A\cos{\left(\omega_1n\right)}+B\cos{\left(\omega_2n\right)}\\
0<\omega_1<\omega_c<\omega_2<\pi
\end{matrix}
$$
- The filter will attenuate or remove the $$\omega_2$$ term.
- Thus, it acts as a __low-pass filter__.


## USE WHAT WE'VE DONE TO DESIGN FILTER
- Say we want to design an LPF
- We can use the inverse DTFT to determine coefficients — so, we draw our rect and solve the IDTFT — that results in a sinc function evaluated at $$n$$.
- Can we build this?
	- no $$h[n]$$ goes forever
	- and, $$h[n]$$ not causal
- Can approximate with a truncated sinc...defined from $$-M$$ to $$M$$
	- still not causal — need a delay of $$M$$
- Quick aside to define delay and then let’s get into the analysis of using DTFT properties to help with filter design.


## TYEPS OF DELAY
$$
\begin{align*}
\tau_g&=\underset{\text{group delay ot output}}{-\frac{d}{d\omega}\left(\theta(\omega)\right)}\\
\tau_p&=\underset{\text{phase delay}}{-\frac{\theta(\omega)}{\omega}}\\
\end{align*}
$$
- Why do we care about delay?
- Suppose we want to average a signal and subtract the average to remove slow drift:
- Propose:
$$
y[n]=x[n]-\frac{1}{5}\sum_{k=0}^{4}{x[n-k]}
$$


### DELAY EXAMPLE
- Call this:
$$
a[n]=\frac{1}{5}\sum_{k=0}^{4}{x[n-k]}
$$
can be analyzed and yields:
$$
\begin{align*}
A\left(e^{j\omega}\right)&=\frac{1}{5}\left(1+e^{-j\omega}+e^{-j2\omega}+e^{-j3\omega}+e^{-j4\omega}\right)\\
&=\frac{1}{5}e^{-j2\omega}\left(e^{j2\omega}+e^{j\omega}+1+e^{-j\omega}+e^{-j2\omega}\right)\\
&=\frac{1}{5}e^{-j2\omega}\left(1+2\cos{\left(\omega\right)}+2\cos{\left(2\omega\right)}\right)
\end{align*}
$$
- In this case,
$$
\theta(\omega)=-2\omega,
$$
- So, $$\text{group delay}=\text{phase delay}=2$$


## CAN WE REMOVE DELAY?
- Let's try with the moving average:
$$
b[n]=\frac{1}{5}\sum_{k=-2}^{2}{x[n-k]}
$$
- can be analyzed and yields:
$$
\begin{align*}
A\left(e^{j\omega}\right)&=\frac{1}{5}\left(e^{j2\omega}+e^{j\omega}+1+e^{-j\omega}+e^{-j2\omega}\right)\\
&=\frac{1}{5}\left(1+2\cos{\left(\omega\right)}+2\cos{\left(2\omega\right)}\right)
\end{align*}
$$
- NO delay. But, it is also no longer causal — replies on TWO future inputs...


## BACK TO FILTER DESIGN
- Recall the DTFT of a sinc function is a rect function. Specifically:
$$
\begin{matrix}
h_{LP}[n]&\underleftrightarrow{CTFT}&H_{LP}\left(e^{j\omega}\right)
\end{matrix}
$$
where
$$
\begin{align*}
h_{LP}[n]&=\frac{\sin{\left(\omega_cn\right)}}{\pi{n}}\\
H_{LP}\left(e^{j\omega}\right)&=\begin{cases}
1,&0\leq|\omega|\leq\omega_c\\
0,&\omega_c<|\omega|\leq\pi
\end{cases}
\end{align*}
$$
- So, a flat, rectangular, symmetric, real function in the frequency domain corresponds to a "sinc" function in the discrete-time domain.
- Specifically, the sinc function is inifinite... thus, it is not something that can be built.
- Instead we truncate symmetrically:
$$
h_2[n] \begin{cases}
h_{LP}[n],&-M\leq{n}\leq{M}\\
0,&|n|>M
\end{cases}
$$
- And analyze to see how good it is.


## FILTER DESIGN: APPLYING FREQUENCY SHIFT
- Parameters that can be adjusted are: $$\omega_c$$ and $$M$$
- Can also take advantage of DTFT properties to create other filters
- __Example__: Create an LPF and apply a frequency shift.
- Shift in frequency domain maps to a complex exponential in the discrete-time domain.
- So
$$
H_{HP}\left(e^{j\omega}\right)=H_{LP}\left(e^{j(\omega-\pi)}\right)
$$
$$
\begin{matrix}
2\pi\delta(\omega-\pi)&\underleftrightarrow{IDTFT}&e^{j\pi{n}}=\left(e^{j\pi}\right)^n=\left(-1\right)^n
\end{matrix}
$$
- Thus
$$
h_{HP}[n]=h_{LP}[n]\left(-1\right)^n
$$


## ANOTHER WAY TO CREATE A HPF
- We can create a high pass filter by manipulating the coefficients of the low pass filter. Note that the new cutoff frequency of the high pass filter is $$pi-\omega_c$$.
- Another way to create HP from a LP is to substract the frequency domain from $$1$$.
$$
\begin{align*}
H_{HP}(z)&=1-H_{LP}(z);\\
H_{HP}\left(e^{j\omega}\right)&=1-H_{LP}\left(e^{j\omega}\right);\\
h_{HP}[n]&=\delta[n]-h_{LP}[n]\\
\end{align*}
$$
- Cutoff in this case is the same.


## FILTER DESIGN: USING CONVOLUTION PROPERTY
- If you take a LPF and convolve the time-domain representation with itself, you will end up with a longer filter — *i.e.* a “better” LPF.
- You will have a HW problem to help convince yourself of this.


## REVISITING DTFT
- Consider a discrete time signal.
$$
\begin{matrix}
x[n]&\underrightarrow{DTFT}&X\left(e^{j\omega}\right)=\sum_{n=-\infty}^{\infty}{x[n]e^{-j\omega{n}}}
\end{matrix}
$$
- Now, assume that we wish to make $$x_N[n]$$ which is a finite version of $$x[n]$$ defined from $$n=0:N-1$$.
- Or,
$$
x_N[n]=x[n]\times{w}[n]
$$
where
$$
\underset{\text{a rectangular window}}{w[n]=\begin{cases}
1,&0\leq{n}\leq{N}-1\\
0,&\text{otherwise}
\end{cases}}
$$
$$
\begin{matrix}
x_N[n]&\underrightarrow{DTFT}&X_N\left(e^{j\omega}\right)=\sum_{n=0}^{N-1}{x[n]e^{-j\omega{n}}}
\end{matrix}
$$
- Can we recover the original $$x[n]$$ from $$X_N\left(e^{j\omega}\right)$$?
- What do DTFT properties tell us about $$X_N\left(e^{j\omega}\right)$$
- If $$N$$ specific values define $$x[n]$$, do we need a continuous function of $$\omega$$ for our DTFT?
- What is the effect of computing only a finite number of values? Can we represent all of the information in $$X_N\left(e^{j\omega}\right)$$?
- Let's see...
- To compute — sample in $$\omega$$ — we will take $$M$$ samples.
- Recall that our period is $$2\pi$$
- So
$$
\begin{align*}
\Delta\omega&=\frac{2\pi}{M}\\
\omega_k&=k\Delta\omega\\
&=\frac{2\pi{k}}{M}
\end{align*}
$$
- So, we sample $$X_N\left(e^{j\omega}\right)$$ at values $$\omega=\omega_k$$.
- The sampled version of $$X_N\left(e^{j\omega}\right)$$ is now
$$
\begin{align*}
X_{N,M}(k)&=\sum_{n=0}^{N-1}{x[n]e^{-j\omega_kn}}\\
&=\sum_{n=0}^{N-1}{x[n]e^{-j\left(k\frac{2\pi}{M}\right)n}}\\
&=\sum_{n=0}^{N-1}{x[n]e^{-j2\pi\frac{nk}{M}}}\\
\end{align*}
$$
- So, what is the effect of sampling?
	- Sampling in time @ $$\Delta{t}=T$$ resulted in a replication in frequency at intervals of $$F_T=\frac{1}{T}$$.
	- So, sampling in frequency @ $$\Delta\omega=\tfrac{2\pi}{M}$$ will result in a replication in time at intervals of $$\tfrac{M}{2\pi}$$
	- .$$M\geq{N}$$ will prevent overlap in time and sampling in frequency domain will NOT cause information loss.
- This leads us to DFT.


## DFT
- DFT is the DTFT of finite length sequence $$x[n]$$ evaluated at finite number of frequency values.
- __Forward DFT__:
$$
\begin{matrix}
X[k]=\sum_{n=0}^{N-1}{x[n]e^{-j2\pi\tfrac{nk}{N}}}\\
\text{for }0\leq{k}\leq{N}-1
\end{matrix}
$$
- __Inverse DFT__:
$$
\begin{matrix}
x[n]=\frac{1}{N}\sum_{n=0}^{N-1}{X[k]e^{+j2\pi\tfrac{nk}{N}}}\\
\text{for }0\leq{n}\leq{N}-1
\end{matrix}
$$
- Both are periodic of period, $$N$$.
- Simplify with $$W_N=e^{-j\tfrac{2\pi}{N}}$$
- so, $$W_N^{nk}$$ for forward and $$W_N^{-nk}$$ for inverse.

### [EXAMPLE: FAMILIARITY WITH $$W_N$$.][]

### [EXAMPLE: SOLVING A GENERIC DFT][]