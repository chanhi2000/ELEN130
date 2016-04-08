# lect04

## COURSE OVERVIEW: PART 1
- ~~Discrete-Time Signals in the Time Domain~~
	- __Operations, Classifications, Sampling__
- Discrete-Time Systems
	- Impulse/Step Responses, LTI Classification, Stability, Response to sinusoidal input
- Discrete-Time Signals in the Frequency Domain
	- Transforms, Applications, Sampling and reconstruction
- Finite-Length Discrete Transforms
	- DFT, FFT, Zero-padding, Fourier Domain filtering, Linear and Circular convolution
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
- Sampling/Alisainsg
- Discrete-Time Systems
- Linearity, Shift-Invariance/Time-Invariance 
- Cascaded Systems


## ALIASING
- There are an infinite number of continuous-time signals which can lead to the same sequence when sampled periodically
- Additional conditions need to be imposed so the sequence $$x[n]$$ can uniquely represent $$x(t)$$.


## SAMPLING VOCABULARY
For continuous time frequency
$$
\begin{align*}
x(t)&=A\cos{(2\pi{f}_0t)}\\
&=A\cos{(\Omega_0t)}
\end{align*}
$$
where
$$
\begin{cases}
f_0&\text{frequency (CT) }\left[\tfrac{\text{cycles}}{sec}\right]\\
\Omega_0&\text{angular frequency (CT) }\left[\tfrac{\text{radians}}{sec}\right]
\end{cases}
$$
- Time variable $$t$$, of $$x_a(t)$$ is related to the discrete time variable $$n$$ of $$x[n]$$, only at discrete-time instants $$t_n$$ where $$t_n=nT$$
- After sampling at rate $$f_T$$ or at rate $$\Omega_T$$, where
$$
\begin{cases}
f_T=\tfrac{1}{T}&\text{sampling frequency/rate }\left[\tfrac{\text{samples}}{sec}\right]\\
\Omega_T&\text{angular sampling frequency (CT) }\left[\tfrac{\text{radians}\cdots\text{samples}}{sec}\right]
\end{cases}
$$

$$
\begin{align*}
x[n]&=A\cos{(2\pi{f}_0nT)}=A\cos{\left(\frac{2\pi{f}_0}{f_T}n\right)}\\
&=A\cos{(\Omega_0nT)}=A\cos{\left(\frac{\Omega_0}{f_T}n\right)}\\
&=A\cos{\left(\frac{2\pi\Omega_0}{\Omega_T}n\right)}\\
&=A\cos{(\omega_0n)}\\
&=A\cos{(2\pi\nu_0n)}\\
\end{align*}
$$
where
$$
\begin{cases}
\nu_0=\frac{{f}_0}{{f}_T}&\text{normalized digital frequency }\left[\tfrac{\text{cycles}}{\text{sec}}\right]\\
\omega_0=\frac{2\pi\Omega_0}{\Omega_T}&\text{normalized digital angular frequency }\left[\tfrac{\text{radians}}{sec}\right]
\end{cases}
$$


## MORE ON ALIASING
- Assertion:
$$
\cos{(\omega_0n)}=\cos{\left((\omega+2\pi{k})n\right)}
$$
- Consider a continuous time signal:  $$x(t)=\cos{(2\pi{f}_0t)}$$ that is sampled with a sampling frequency $$f_T$$.
- This yields:
$$
\begin{align*}
x[n]&=\cos{(2\pi{f}_0nT)}&\begin{cases}\omega_0=\frac{2\pi\Omega_0}{\Omega_T}=\frac{2\pi{f}_0}{f_T}\\f_T=\frac{1}{T}\end{cases}\\
&=\cos{\omega_0n}\\
&=\cos{\left((\omega_0+2\pi{k})n\right)}\\
&=\cos{\left((2\pi{f}_0T+2\pi{k})n\right)}\\
&=\cos{\left(2\pi{f}_0nT+2\pi{k}n\right)}\\
&=\cos{\left(2\pi{f}_0nT+2\pi{k}nTf_T\right)}\\
&=\cos{2\pi{n}T\left(f_0+kf_T\right)}
\end{align*}
$$
- Since $$x[n]=\cos{(2\pi{f}_0nT)}=\cos{\left(2\pi{n}T(f_0+kf_T)\right)}$$, this implies that an original continuous time domain signal with frequency $$f_0+kf_T$$  is indistinguishable from one with a frequency of $$f_0$$ after it has been sampled at sampling frequency: $$f_T$$
- This is the essense of aliasing!

