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
- Overlap and Add


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
x_N[n]=x[n]\otimes{w}[n]
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


## DFT: VIEWING $$W_N$$ around the unit circle.
- To compute $$X[k]$$ take $$k$$ steps around the unit circle to get factors to multiply.
- If $$N=4$$, there will be $$4$$ steps around the unit circle:
![fig02](lect07/lect07-fig02.png)


## DFT IN MATRIX REPRESENTATION
$$
\begin{bmatrix}
X[0]\\X[1]\\X[2]\\X[3]
\end{bmatrix}=\begin{bmatrix}
W_4^0&W_4^0&W_4^0&W_4^0\\
W_4^0&W_4^1&W_4^2&W_4^3\\
W_4^0&W_4^2&W_4^4&W_4^6\\
W_4^0&W_4^3&W_4^6&W_4^9
\end{bmatrix}\begin{bmatrix}
x[0]\\x[1]\\x[2]\\x[3]
\end{bmatrix}
$$
- __notation__: This is $$D_4$$.
$$
\underline{X}=D_N\underline{x}
$$
- __NOTE__:
$$
\begin{matrix}
\underline{x}=D_N^{-1}\underline{X}\\
\text{where }D_N^{-1}=\frac{1}{N}D_N^*
\end{matrix}
$$
- Frequency vector = DFT mattrix for $$N=4$$ times time vector.

### [EXAMPLE: DFT AROUND UNIT CIRCLE][]


## DFT SUMMARIZED
- DFT is a relationship between a sampled periodic sequence in time and a sampled periodic sequence in frequency.
- DFT is very useful in structured computation and fast algorithms.


## MORE DFT EXAMPLES
$$
\begin{matrix}
x[n]&\underrightarrow{DFT}&X[k]
\end{matrix}
$$
where
$$
\begin{align*}
x[n]&=\delta[n-n_0]\\
X[k]&=\sum_{n=0}^{N-1}{x[n]e^{-j2\pi\tfrac{nk}{N}}}\\
&=e^{-j2\pi\tfrac{n_0k}{N}}
\end{align*}
$$
- Likewise,
$$
\begin{matrix}
X[k]&\underrightarrow{IDFT}&x[n]
\end{matrix}
$$
where
$$
\begin{align*}
X[k]&=\delta[k-k_0]\\
x[n]&=\frac{1}{N}\sum_{n=0}^{N-1}{X[k]e^{+j2\pi\tfrac{nk}{N}}}\\
&=\frac{1}{N}e^{+j2\pi\tfrac{n_0k}{N}}
\end{align*}
$$
- Another one:
$$
\begin{matrix}
w_M[n]=\text{window of }M\text{ samples }\\
M<N
\end{matrix}
$$
so,
$$
x[n]=\sum_{m=0}^{M-1}{\delta[n-m]}
$$
- So, the DFT is:
$$
\begin{align*}
X[k]&=\sum_{n=0}^{N-1}{x[n]e^{-j2\pi\tfrac{nk}{N}}}\\
&=\sum_{n=0}^{M-1}{e^{-j2\pi\tfrac{nk}{N}}}\\
&=\frac{1-e^{-j2\pi\tfrac{Mk}{n}}}{1-e^{-j2\pi\tfrac{k}{n}}}
\end{align*}
$$
- Manipulated — this becomes:
$$
\exp\left({-j\pi\frac{(M-1)k}{N}\frac{\sin{\left(\tfrac{\pi{M}k}{N}\right)}}{ \sin{\left(\tfrac{\pi{k}}{N}\right)}}}\right)
$$
- This is the discrete sinc sampled and period with period, $$N$$.


## REVIEW PERIODICITY
- To review, DFT is $$N$$ evenly spaced samples of $$\omega$$ where
$$
\begin{align*}
\Delta\omega&=\tfrac{2\pi}{N}\\
\omega_k&=k\Delta\omega
\end{align*}
$$
- Both $$x[n]$$ and $$X[k]$$ are periodic with period, $$N$$.
$$
\begin{align*}
x[n+N]&=x[n]\\
X[k+N]&=X[k]
\end{align*}
$$
- What is $$X[-1]$$?


## REAL WORLD FREQUENCY ASSOCIATED WITH SAMPLES
- So, what is the real world frequency associated with a particular $$k_0$$ sample?
$$
\omega_{k_0}=k_0\Delta\omega=\frac{k_02\pi}{N}
$$
- This is the discrete-time frequency.
- Sampled in the time domain at frequency, $$T$$, thus replicated in the $$f$$-domain at $$F_T$$ and we know that it is periodic with period, $$N$$... and, we know that the DTFT was periodic with period $$2\pi$$. So, the $$\omega_{k_0}$$ is a fraction of the sampling frequency...
$$
\frac{\omega_{k_0}}{2\pi}F_T
$$
is the frequency at $$k_0$$.
- Quick aside — earlier I failed to clarify the difference between $$\Omega$$ used in the CTFT and $$w$$ used in the DTFT. $$w$$ is simply a frequency normalized between $$-\pi$$ and $$\pi$$.


## USE OF DFT FOR CONVOLUTION
- So, similarly to how this was done for DTFT, the convolution in time transforms to multiplication in the frequency domain for the DFT.
- However, when doing this with the DFT, care must be taken with the length of the sequences.
- Say we convolve some sequence $$x[n]$$ with some sequence $$h[n]$$.
- __Linear convolution__:
$$
\begin{matrix}
g_L[n]&\underleftrightarrow{DTFT}&G\left(e^{j\omega}\right)
\end{matrix}
$$
where
$$
\begin{align*}
g_L[n]&=\sum_{k=0}^{N-1}{x[k]h[n-k]}\\
G\left(e^{j\omega}\right)&=X\left(e^{j\omega}\right)H\left(e^{j\omega}\right)
\end{align*}
$$
- __Cicular convolution__:
$$
\begin{matrix}
g_c[n]&\underleftrightarrow{DFT}&G[k]
\end{matrix}
$$
where
$$
\begin{align*}
g_c[n]&=\sum_{k=0}^{N-1}{x[k]h[\left<n-k\right>_N]}\\
G[k]&=X[k]H[k]
\end{align*}
$$
- for the DFT, everyhing is periodic with period, $$N$$ (the length of the DFT).


## LINEAR VS. CIRCULAR CONVOLUTION
- Assume $$N=3$$
$$
\begin{align*}
g_L[0]&=x[0]h[0]+x[1]h[-1]+x[2]h[-2]\\
g_L[1]&=x[0]h[1]+x[1]h[0]+x[2]h[-1]\\
g_L[2]&=x[0]h[2]+x[1]h[1]+x[2]h[0]\\
\end{align*}
$$
and
$$
\begin{align*}
g_c[0]&=x[0]h[0]+x[1]h[N-1]+x[2]h[N-2]\\
g_c[1]&=x[0]h[1]+x[1]h[0]+x[2]h[N-1]\\
g_c[2]&=x[0]h[2]+x[1]h[1]+x[2]h[0]\\
\end{align*}
$$

### [EXAMPLE: CIRCULAR CONVOLUTION][]
74-77


## FREQUENCY RESOLUTION
- Zero padding computes more values for DTFT or interpolates the DFT... it does NOT add information or improve frequency resolution.
- Recall, infinite lenght $$x[n]$$ DTFT to $$X\left(e^{j\omega}\right)$$.
- If we take $$N$$ points of $$x[n]$$, we would get $$N$$ points in $$X[k]$$ and the frequency spacing (or frequency resolution) would be $$\Delta\omega=\tfrac{2\pi}{N}$$.
- Now, if we took $$2N$$ points of $$x[n]$$, we would get $$2N$$ points in $$X[k]$$ and the frequency resolution would be $$\Delta\omega=\tfrac{2\pi}{2N}$$


## DFT SYMMETRIES
- An $$N$$-point DFT $$X[k]$$ is said to be a circular conjugate-symmetric sequence if:
$$
\begin{align*}
X[k]&=X^*[\left<-k\right>_N]\\
&=X^*[\left<N-k\right>_N]
\end{align*}
$$
- And, it is said to be a circular conjugate-antisymmetric sequence if:
$$
\begin{align*}
X[k]&=-X^*[\left<-k\right>_N]\\
&=-X^*[\left<N-k\right>_N]
\end{align*}
$$
- Similar to previous sequences, the complex DFT, $$X[k]$$ can be expressed as a sum of a circular conjugate symmetric part $$X_{cs}[k]$$ and a circular conjugate anti-symmetric part $$X_{ca}[k]$$.
$$
\begin{align*}
X[k]&=X_{cs}[k]+X_{ca}&&\text{for }0\leq{k}\leq{N}-1\\
X_{cs}[k]&=\frac{1}{2}\left(X[k]+X^*[\left<-k\right>_N]\right)&&\text{for }0\leq{k}\leq{N}-1\\
X_{ca}[k]&=\frac{1}{2}\left(X[k]-X^*[\left<-k\right>_N]\right)&&\text{for }0\leq{k}\leq{N}-1\\
\end{align*}
$$


## DFT PROPERTIES: SYMMETRY RELATIONS

| Length-$$N$$ Sequence | $$N$$-point DFT |
| :-------------------: | :-------------: |
| $$x[n]$$ | $$X[k]$$ |
| $$x^*[n]$$ | $$X^*[\left<-k\right>_N]$$ |
| $$x^*[\left<-n\right>N]$$ | $$X^*[k]$$ |
| $$\Re{\left\{x[n]\right\}}$$ | $$X_{pcs}[k]$$ |
| $$j\Im{\left\{x[n]\right\}}$$ | $$X_{pca}[k]$$ |
| $$x_{pcs}[n]$$ | $$\Re{\left\{X[k]\right\}}$$ |
| $$x_{pca}[n]$$ | $$j\Im{\left\{X[k]\right\}}$$ |
| $$x[n]$$ | $$X[k]=\Re{\left\{X[k\right\}}+j\Im{\left\{X[k]\right\}}$$ |
| $$x_{pe}[n]$$ | $$\Re{\left\{X[k\right\}}$$ |
| $$x_{po}[n]$$ | $$j\Im{\left\{X[k\right\}}$$ |

where
$$
\begin{align*}
X_{pcs}[k]&=\frac{1}{2}\left(X[\left<k\right>_N]+X^*[\left<-k\right>_N]\right)\\
X_{pca}[k]&=\frac{1}{2}\left(X[\left<k\right>_N]-X^*[\left<-k\right>_N]\right)\\
\end{align*}
$$
- __NOTE__: $$x_{pcs}[n]$$ and $$x_{pca}[n]$$ are the __periodic conjugate-symmetric__ and __periodic conjugate-antisymmetric__ parts of $$x[n]$$, respectively.
- Likewise, $$X_{pcs}[k]$$ and $$X_{pca}[k]$$ are the __periodic conjugate-symmetric__ and __periodic conjugate-antisymmetric__ parts of $$X[k]$$, respectively.


## SYMMETRY RELATIONS
$$
\begin{align*}
X[k]&=X^*[\left<-k\right>_N]\\
\Re{\left\{X[k]\right\}}&=\Re{\left\{X[\left<-k\right>_N]\right\}}\\
\Im{\left\{X[k]\right\}}&=-\Im{\left\{X[\left<-k\right>_N]\right\}}\\
\left|X[k]\right|&=\left|X[\left<-k\right>_N]\right|\\
\arg{\left(X[k]\right)}&=\arg{X[\left<-k\right>_N]}
\end{align*}
$$
- __NOTE__: $$x_{pe}[n]$$ and $$x_{po}[n]$$ are the periodic even and periodic odd parts of $$x[n]$$. respectively.


## MORE DFT PROPERTIES:

| Theorems | Length-$$N$$ Sequence | $$N$$-point DFT |
| :------: | :-------------------: | :-------------: |
| Linearity | $$\alpha{g}[n]+\beta{h}[n]$$ | $$\alpha{G}[k]+\beta{H}[k]$$ |
| Circular time-shifting | $$g[\left<n-n_0\right>_N]$$ | $${W_{N}}^{kn_0}G[k]$$ |
| Circular time-shifting | $${W_{N}}^{-k_0n}g[n]$$ | $$G[\left<k-k_0\right>_N]$$ |
| Duality | $$G[n]$$ | $$Ng[\left<-k\right>_N]$$ |
| $$N$$-point circular convolution | $$\sum_{m=0}^{N-1}{g[m]h[\left<n-m\right>_N]}$$ | $$G[k]H[k]$$ |
| Modulation | $$g[n]h[n]$$ | $$\tfrac{1}{N}\sum_{m=0}^{N-1}{G[m]H[\left<k-m\right>_N]}$$ |

- Parseval's relation:
$$
\sum_{n=0}^{N-1}{|x[n]|^2}=\frac{1}{N} \sum_{k=0}^{N-1}{|X[k]|^2}
$$


## CYCLIC PREFIX
- Another way to calculate the circular convolution.
	1. Assume $$x[n]$$ has length $$N$$ and $$h[n]$$ has length $$M$$ with $$N>M$$.
	2. Take the last $$M-1$$ terms of $$x[n]$$ and append them to the front of $$x[n]$$.
	3. Take the linear convolution.
	4. Take the center $$N$$ samples of the convolution — this is the circular convolution.
- __NOTE__: The process of appending the last $$M-1$$ terms to the front of $$x[n]$$s called applying the cyclic prefix.


## OVERLAP-ADD
- Assume we have a long sequence, $$x[n]$$, that we wish to convolve with $$h[n]$$ (length $$M$$, which, by comparison, is short).
- Now, assume we segment $$x[n]$$ into length-$$N$$ sections that we number, such that:
$$
\begin{align*}
x_m[n]&=\begin{cases}
x[n+mN],&0\leq{n}\leq{N}-1\\
0,&\text{otherwise}
\end{cases}\\
&=x[n]=\sum_{m=0}^{\infty}{x_m[n-mN]}
\end{align*}
$$
- Say
$$
y_m[n]=h[n]\otimes{x}_m[n]
$$
This linear convolution should be in length $$M+N-1$$. Thus
$$
\begin{align*}
y_m[n]&=h[n]\otimes{x}_m[n]\\
&=\sum_{m=0}^{\infty}{y_m[n-mN]}
\end{align*}
$$
- The short linear convolutions can be performed using the DFT methods described earlier (with appropriate zero-padding).
- One last thing to recognize is that there is overlap between the end of one of the short convolutions and the start of the next convolution. These overlaps need to be added.
- Specifically, $$M-1$$ samples overlap at each junction.
- A HW problem will ask that you break this down and perform this.

![EXAMPLE: OVERLAP-ADD][]


## OVERLAP-SAVE
- Using Overlap-Add — if using the DFT to compute the convolutions
	- two $$(N+M-1)$$-point DFTs
	- one $$(N+M-1)$$-point IDFTs
- It is possible to do this entire convolution by performing circular convolutions of shorter length — thus, shorter DFTs.
- To do so, need to segment $$x[n]$$ into overlapping blocks, keep the terms of the circular convolution of $$h[n]$$ with the $$x_m[n]$$ that corresponds to the terms obtained by the linear convolution of $$h[n]$$ with the $$x_m[n]$$ that corresponds to the terms obtained by the linear convolution and throw away the other parts... Let's see what this looks like.
- Assume we have a length-$$4$$ sequence, $$x[n]$$, and a length-$$3$$ sequence, $$h[n]$$.
- The result of the linear convolution of $$x[n]$$ with $$h[n]$$ is dentoed $$y_L[n]$$.
- The 6 samples are:
$$
\begin{align*}
y_L[0]&=h[0]x[0]\\
y_L[1]&=h[0]x[1]+h[1]x[0]\\
y_L[2]&=h[0]x[2]+h[1]x[1]+h[2]x[0]\\
y_L[3]&=h[0]x[3]+h[1]x[2]+h[2]x[1]\\
y_L[4]&=h[1]x[3]+h[2]x[2]\\
y_L[0]&=h[2]x[3]\\
\end{align*}
$$
- If we append a single $$0$$ to the end of $$h[n]$$ and compute the $$4$$-point circular convolution (easily obtained by doing two $$4$$-point DFTs, multiplying and a $$4$$-point IDFT)
$$
\begin{align*}
y_c[0]&=h[0]x[0]+h[1]x[3]+h[2]x[2]\\
y_c[1]&=h[0]x[1]+h[1]x[0]+h[2]x[3]\\
y_c[2]&=h[0]x[2]+h[1]x[1]+h[2]x[0]\\
y_c[3]&=h[0]x[3]+h[1]x[2]+h[2]x[1]\\
\end{align*}
$$
$$
y_c[2]=y_c[3]
$$
So, we can save those, throw saway the other two and move on.
- More Generally:
	- .$$N$$-point circular convolution of length-$$M$$ sequence (zeroes added) with length-$$N$$ sequence, $$N>M$$.
	- First $$M-1$$ samples of the circular convolution are incorrect and are rejected.
	- Remaining $$N-M+1$$ samples correspond to correct samples of linear convolution.
- OK — now for a long sequence, how do we break it up?
- A collection of smaller length, overlapping sequences — say, for example, of length-$$4$$.
$$
x_m[n]=x[n+2m]\\
\text{for }\begin{matrix}
0\leq{n}\leq3,\\
0\leq{m}\leq\infty,\\
\end{matrix}
$$
- Then, form
$$
w_m[n]=h[n]\underset{4\text{-point convolution}}{\otimes}$$x_m[n]
$$
- For the above example:
$$
\begin{align*}
w_0[2]&=y[2];\\
w_0[2]&=y[3];
\end{align*}
$$
- Then:
$$
\begin{align*}
w_1[2]&=y[4];\\
w_1[2]&=y[5];
\end{align*}
$$
- This pattern continues, but is still specific to length-$$4$$ sub-sequences.
- To create $$y[0]$$ and $$y[1]$$, need to form $$x_{-1}[n]$$ where the first two terms are $$0$$ and the last two are $$x[0]$$ and $$x[1]$$ respectively. Again, you keep $$w_{-1}[2]$$ and $$w_{-1}[3]$$.
- General, general case:
- .$$h[n]$$ is length $$N$$
$$
x_m[n]=x[n+(N-M+1)m]\\\\
\text{for }0\leq{n}\leq3
$$
- Then, calculate all of the $$N$$-point circular convolution so:
$$
x_m[n]=h[n]\underset{N\text{-circularly convolution}}{\otimes}x_m[n]
$$
- Reject the first $$M-1$$ samples and keep the rest $$(N-M+1)$$
- Then,
$$
y_m[n]=y_L[n+(N-M+1)m]
$$


## BIG PICTURE
- What can we do?
	- quickly compute DTFT and IDTFT using, DFT, IDFT through MATLAB's `fft` and `ifft` or `freqz`.
- What do we want to be able to do?
	- Have another way to look at the frequency response to quickly determine stability
	- Have a way to design a filter with feedback
	- These will be done with z-transform


