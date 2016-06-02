# lect16

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
- ~~Digital filter structures and representations; 2nd order building blocks~~
- ~~FIR Design, Windowing~~
- __IIR Design, Bilinear transformation__
- __IIR filter design with MATLAB__
- Review of signal acquisition and reconstruction from frequency domain perspective, interpolating filters, zero-padding, A/D and D/A converters, anti-aliasing filter, sample-and-hold, anti-imaging filter
- Multirate DSP, up-sampling and down-sampling
- Implementation considerations—quantization and dynamic range
- Specific Applications (if time)

## OVERVIEW
- Bilinear Transformation
- IIR Filters in MATLAB
- FIR Filter (Parks-McClellan)


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
![fig03]()
- What is the frequency domain of this?
	- Time-domain of this window is the convolution of two $$
	[1,\:1,\:1,\:1,\:1,\:1,\:1,\:\cdots]$$ array.
	- So freq-domain is
	$$
	W_T\left(e^{j\omega}\right)=\left(\frac{\tfrac{\sin{\left(M+1\right)\omega}}{2}}{\sin{\left(\tfrac{\omega}{2}\right)}}\right)^2
	$$
	- __benefits__: falls of as $$\tfrac{1}{\omega^2}$$ instead of $$\tfrac{1}{\omega}$$.
	- __downside__: "main lobe" is twice as wide
	$$
	\begin{matrix}
	\sin{\left(2M+1\right)\omega}}{2}&\text{vs.}&\sin{\left(2M+1\right)\omega}}{2}
	\end{matrix}
	$$


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

| kind | Mian Lobe Width | Transition Band | Relative Side Lobe Level | Stop-Band Attenuation |
| :--- | :-------------: | :-------------: | :----------------------: | :-------------------: |
| Rectangular | $$4\frac{\pi}{2M+1}$$ | $$0.92\frac{\pi}{M}$$ | $$-13.3\:\text{dB}$$ | $$-21\:\text{dB}$$ |
| Hanning | $$8\frac{\pi}{2M+1}$$ | $$3.11\frac{\pi}{M}$$ | $$-31.5\:\text{dB}$$ | $$-44\:\text{dB}$$ |
| Hamming | $$8\frac{\pi}{2M+1}$$ | $$3.37\frac{\pi}{M}$$ | $$-42.7\:\text{dB}$$ | $$-53\:\text{dB}$$ |
| Blackman | $$12\frac{\pi}{2M+1}$$ | $$5.56\frac{\pi}{M}$$ | $$-58.1\:\text{dB}$$ | $$-74\:\text{dB}$$ |


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


## TYPE I CHEBYSHEV FILTERS AND MATLAB EXAMPLE
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


## TYPE II CHEBYSHEV FILTERS AND MATLAB EXAMPLE
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
a_1&=\frac{2\alpha{K}}{1+K};\\
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
B = firpm(N, Fo, Ao, W)
```
<`W` weights the error in each band allowing you to prioritize>

The resulting filter will approximately meet the specifications given by the input parameters `F`, `A`, and `DEV`.
- `F` is a vector of cutoff frequencies in Hz, in ascending order between 0 and half the sampling frequency `Fs`. If you do not specify `Fs`, it defaults to 2.
- `A` is a vector specifying the desired function's amplitude on the bands defined by `F`. The length of `F` is twice the length of `A`, minus 2 (it must therefore be even). The first frequency band always starts at zero, and the last always ends at `Fs`/2. It is not necessary to add these elements to the `F` vector.
- `DEV` is a vector of maximum deviations or ripples (in linear units) allowable for each band. `DEV` must have the same length as `A`.


## `firpmord` — THINGS TO NOTE
- `F` are actual frequencies and `Fs` is the sampling frequency.
- length of `F` is `2*length(A)-2`

