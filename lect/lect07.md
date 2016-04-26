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
