# lect07

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
- Bandpass signal Sampling


## EXAMPLE: SAMPLING AND RECONSTRUCTION


## SAMPLING BANDPASS SIGNALS
What was special about the previous two cases?


## SAMPLING THEOREM FOR BANDPASS SIGNALS
- Many signals are narrowband
	- Despite being at large frequencies, they take up relatively small BW
- Undersampling and taking advantage of the resulting aliasing can yield benefits.
$$
\begin{align*}
B_{IF}&=f_H-f_L\\
F_T&=2*B_{IF}&&\text{if }\frac{f_H}{B_{IF}}=n
\end{align*}
$$
- If $$n$$ is an even integer for the above, spectral inversion occurs
- if $$\tfrac{f_H}{B_{IF}}$$ is *not* an integer, then math gets slightly more complicated
- Two inequalities must be preserved:
$$
\begin{matrix}
-(n-1)\frac{F_T}{2}F_T\leq{f}_L\\
f_H\leq(n)\frac{F_T}{2}
\end{matrix}
$$
- __New Sampling Theorem__: A bandpass signal occupying $$\left[f_L,\:f_H\right]$$ and $$B_{IF}=f_H-f_L$$ can be exactly reconstructed from its samples through bandpass filtering if the sampling frequency, $$F_T$$, satisfies
$$
\frac{2f_H}{n}\leq{F}_T\leq\frac{2f_L}{(n-1)}
$$
where $$n_\text{max}=\left\lfloor\left(\frac{f_H}{B_{IF}}\right)\right\rfloor$$
- Thus, the minimum satisfactory $$F_T=\frac{2f_H}{n_\text{max}}$$
- If $$n_\text{max}$$ is an even integer for the above, spectral inversion occurs


## FREQUENCY RESPONSE
- Fourier Transform tells us that most signals can be represented as a linear combination of sinusoidal signals of different angular frequencies.
- Knowing the response of an LTI system to a single sinusoidal signal means that we can determine its response to more complicated signals through superposition


## EXAMPLE: FREQUENCY RESPONSE
- Consider an altered moving average filter:
$$
y[n]=\frac{1}{M}\sum_{k=0}{M-1}{x[n-k]}
$$
- For simpicity, we will lose $$\tfrac{1}{M}$$ factor.
- Let's say $$M=3$$, so
$$
y[n]=x[n]+x[n-1]+x[n-2]
$$
- Now, let's assume the input is a generic cosine:
$$
x[n]=\cos{\left(\omega_0n\right)}
$$
- So,
$$
y[n]=\cos{\left(\omega_0n\right)}+\cos{\left(\omega_0(n-1)\right)}+\cos{\left(\omega_0(n-2)\right)}
$$
- Without a loss of generality, we can do a variable substitution: $$m=n-1$$
$$
y[m+1]=\cos{\left(\omega_0(m+1)\right)}+\cos{\left(\omega_0m\right)}+\cos{\left(\omega_0(m-1)\right)}
$$
- Using trig identitiess:
$$
\begin{align*}
y[m+1]&=\left(\cos{\left(\omega_0m\right)}\cos{\left(\omega_0)\right)}\right)-\left(\sin{\left(\omega_0m\right)}\sin{\left(\omega_0\right)}\right)\\
&+\cos{\left(\omega_0m\right)}\\
&+\left(\cos{\left(\omega_0m)\right)}\cos{\left(\omega_0)\right)}\right)+\left(\sin{\left(\omega_0m)\right)}\sin{\left(\omega_0)\right)}\right)\\
&=\cos{\left(\omega_0m)\right)}\left(1+2\cos{\left(\omega_0\right)}\right)\\
y[n]&=\cos{\left(\omega_0(n-1))\right)}\left(1+2\cos{\left(\omega_0\right)}\right)\\
\end{align*}
$$
- So, what we have is an input that is delayed by one sample
- And, more importantly, a scale factor that depends on the frequency and NOT $$n$$!!!!!
- Plot of the gain of the filter is below
![fig01](ex07/ex07-fig01.png)
- How does this behave for different values of $$\omega_0$$?
$$
\begin{align*}
\omega_0&=0?,\\
\omega_0&=\frac{\pi}{2}?\\
\omega_0&=\frac{2\pi}{3}?\\
\omega_0&=\pi?\\
\end{align*}
$$
- So, is there an easier way to find frequency selective behavior?
- Let's use complex exponentials to avoid trig
$$
\begin{align*}
e^{j\theta}&=\cos{\left(\theta\right)}+j\sin{\left(\theta\right)}\\
e^{-j\theta}&=\cos{\left(\theta\right)}-j\sin{\left(\theta\right)}\\
e^{j2\theta}&=\cos{\left(2\theta\right)}+j\sin{\left(2\theta\right)}\\
&=\left(e^{j\theta}\right)^2=\left(\cos{\left(\theta\right)}+j\sin{\left(\theta\right)}\right)^2\\
&=\cos^2{\left(\theta\right)}+2j\cos{\left(\theta\right)}\sin{\left(\theta\right)}+j^2\sin^2{\left(\theta\right)}\\
&=\cos^2{\left(\theta\right)}-\sin^2{\left(\theta\right)}+j\left(2\cos{\left(\theta\right)}\sin{\left(\theta\right)}\right)\\
&=\cos{\left(2\theta\right)}+j\sin{\left(2\theta\right)}
\end{align*}
$$


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
- So, for the complex exponential input siganl, the LTI system is a complex exponential signal of the  same frequency multiplied by a complex constant $$H\left(e^{j\omega}\right).
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
\left(\sum_{k=0}^{\infty}{h[k]e^{-j\omega{k}}}\right)e^{j\omega{n}}-\left(\sum_{k=n+1}^{\infty}{h[k]e^{-j\omega{k}}}\right)e^{j\omega{n}}
\end{cases}
$$
- steady state response MINUS transient response.


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
