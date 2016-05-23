# lect13

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
- __Basic filter structures: All pass, LPF, band pass, HPF, comb filter, prototype LPF__
- __Digital filter structures and representations; 2nd order building blocks__
- FIR Design, Windowing
- IIR Design, Bilinear transformation
- IIR filter design with MATLAB
- Review of signal acquisition and reconstruction from frequency domain perspective, interpolating filters, zero-padding, A/D and D/A converters, anti-aliasing filter, sample-and-hold, anti-imaging filter
- Multirate DSP, up-sampling and down-sampling
- Implementation considerations—quantization and dynamic range
- Specific Applications (if time)


## OVERVIEW
- More filter types
- Fiter structures


## EXAMPLE OF SYMMETRIC FILTERS
- IDEAL LPF
$$
H\left(e^{j\omega}\right)=\begin{cases}
1,&|\omega|<\omega_0\\
0,&\omega_c<|\omega|<\pi
\end{cases}
$$
- IDTFT:
$$
\begin{align*}
h[n]&=\frac{\sin{\left(\omega_c{n}\right)}}{\pi{n}}\\
&=\frac{\omega_c}{\pi}\cdot\frac{\sin{\left(\omega_c{n}\right)}}{\omega_c{n}}
\end{align*}
$$

## PRACTICAL LPF
- Approximate implementation (practical LPF):
	- Make finite duration
	- Make causal — delay by $$M$$
$$
h_2[n]=\begin{cases}
h[n],&-M\leq{n}\leq{M}\\
0,&|n|>M
\end{cases}
$$
	- __length__: $$2M+1$$
	- __order__: $$2M$$
$$
h_3[n]=h_2[n-M]
$$
	- ...$$h_2[n]$$ is symmetric about $$0$$
	- ...$$h_3[n]$$ is symmetric about $$n=M=\tfrac{N}{2}$$


## EXAMPLES OF ANTI-SYMMETRIC FILTERS
- Have odd symmetry
- *ideal differentiatior*:
$$
\begin{matrix}
H\left(e^{j\omega}\right)=j\omega,&0\leq|\omega|<\pi
\end{matrix}
$$
- IDTFT: ideal differentiatior
$$
h[n]=\begin{cases}
0,&n=0\\
\tfrac{(-1)^n}{n},&|n|>0
\end{cases}
$$
- *ideal Hilbert Transformer*:
$$
H\left(e^{j\omega}\right)=\begin{cases}
j,&-\pi<\omega<0\\
-j&0<\omega<\pi\\
0,&\omega=0
\end{cases}
$$
- IDTFT: ideal Hilbert Transformer
$$
h[n]=\begin{cases}
0,&n\text{ even}\\
\tfrac{2}{\pi{n}},&n\text{ odd}
\end{cases}
$$


## DISCUSSION QUESTION #1
- An order $$N$$ filter with even symmetry is increased to order $$N+1$$ by adding a zero at $$z=-1$$.
- Show that the new filter will have __even symmetry__
- What does it mean to add a zero at $$z=-1$$?
	- multiply by $$\tfrac{z+1}{z}$$ in frequency domain
- What can this look like in the time domain?
	- convolve by $$\delta[n]+\delta[n-1]$$.
- How do we test for even symmetry?
$$
h[n]=h[N-n]
$$
- __new filter__:
$$
\begin{align*}
h_2[n]&=h[n]+h[n-1],&&\text{for }1\leq{n}\leq{N}\\
h_2[0]&=h[0]\\
h_2[N+1]&=h[N]\\
\end{align*}
$$
- __test__:
$$
\begin{align*}
h_2[N+1-n]&=h[N+1-n]+h[N-n]\\
&=h[N-(n-1)]+h[N-n]\\
&=h[n-1]+h[n]\\
&=h_2[n]
\end{align*}
$$
- Thus, still __even__.


## DISCUSSION QUESTION #2
- *ideal differentiatior*:
$$
\begin{matrix}
H\left(e^{j\omega}\right)=j\omega,&0\leq|\omega|<\pi
\end{matrix}
$$
- IDTFT: ideal differentiatior
$$
h[n]=\begin{cases}
0,&n=0\\
\tfrac{(-1)^n}{n},&|n|>0
\end{cases}
$$
- Causal FIR approximation is implemented with a TYPE 3 SYMMETRIC (ODD SYMMETRY) FILTER of order $$N=2M$$ by:
$$
\begin{align*}
h_\text{diff}[n]&=\begin{cases}
h[n-M],&0\leq{n}\leq{N}\\
0,&\text{otherwise}
\end{cases}
\end{align*}
$$
- Find filter coefficient for $$N=2$$ and for $$N=4$$.

| order | coefficients |
| :---: | :----------: |
| 2 | $$1,\:0,\:-1$$ |
| 4 | $$-\tfrac{1}{2},\:1,\:0,\:-1,\:\tfrac{1}{2}$$ |

- For $$N=4$$, and using the limitations of positions of zeros for a TYPE 3 SYMMETRIC FILTER, find the zeros for this filter.
	- making the $$N=4$$ coefficients causal, we get:
	$$
	H(z)=\frac{-\tfrac{1}{2}z^4+z^3-z+\tfrac{1}{2}}{z^4}
	$$
	- What zeros must a TYPE 3 SYMMETRIC FILTER have?
	$$
	\begin{matrix}
	z=1&\text{and}&z=-1
	\end{matrix}
	$$
	- This means $$z^2-1$$ must be part of it, *i.e.* $$(z+1)(z-1)$$
	- Polynomial division yields the rest:
	$$
	\begin{align*}
	-\frac{1}{2}z^2+z-\frac{1}{2}&=-\frac{1}{2}\left(z^2-2z+1\right)\\
	&=-\frac{1}{2}(z-1)^2
	\end{align*}
	$$
	- What should the frequency response look like? Which one is closer?
- Plots for $$N=2$$ and $$N=4$$:
![fig01a]()
![fig01b]()
![fig01c]()


## CONVERTING A LPF TO A HPF
### METHOD 1: sbstract
- IDEAL: no delay,
$$
\begin{align*}
H_{HP}(z)&=1-H_{LP}(z);\\
H_{HP}\left(e^{j\omega}\right)&=1-H_{LP}\left(e^{j\omega}\right);\\
h_{HP}[n]&=\delta[n]-h_{LP}[n]
\end{align*}
$$
- For even, order $$N$$, causal FIR filter:
$$
\begin{align*}
H_{HP}(z)&=z^{-\tfrac{N}{2}}-H_{LP}(z)\\
h_{HP}[n]&=\begin{cases}
-h_{LP}[n],&0\leq{n}\leq{N},\:\:n\neq\tfrac{N}{2}\\
1-h_{LP}[n],&n=\tfrac{N}{2}
\end{cases}
\end{align*}
$$

### METHOD 2: rotate by $$\pi$$
- IDEAL: no delay,
$$
\begin{align*}
H_{HP}(z)&=H_{LP}(-z);\\
H_{HP}\left(e^{j\omega}\right)&=H_{LP}\left(-e^{j\omega}\right)=H_{LP}\left(e^{j(\omega-\pi)}\right);\\
h_{HP}[n]&=(-1)^nh_{LP}[n]
\end{align*}
$$
- For even, order $$N$$, causal FIR filter:
$$
\begin{align*}
H_{HP}(z)&=(-1)^{-\tfrac{N}{2}}H_{LP}(-z)\\
h_{HP}[n]&=(-1)^{-\tfrac{N}{2}}(-1)^{n}h_{LP}[n]
\end{align*}
$$

## SIMPLE FIR LPF
$$
H_o(z)=\frac{1}{2}(1+z^{-1})
$$
- this is a moving average of length $$2$$.
- What is $$h[n]$$?
	$$
	h[n]=\frac{1}{2}\delta[n]+\frac{1}{2}\delta[n-1]
	$$
- What type of FIR filter is this?
	- __symmetry__: even
	- __length__: even
	- __order__: odd
	- TYPE 2 FIR FILTER
$$
\begin{align*}
H_0(z)&=\frac{1}{2}\left(1+e^{-j\omega}\right)\\
&=\frac{1}{2}e^{-j\omega\tfrac{1}{2}}\left(2\cos{\left(\frac{1}{2}\omega\right)}\right)\\
&=e^{-j\omega\tfrac{1}{2}}\cos{\left(\frac{1}{2}\omega\right)}
\end{align*}
$$
- Plot $$\left|H_o\left(e^{j\omega}\right)\right|$$
![fig02]()


## BETTER FIR LPF
- Simple LPF does not provide much attenuation at higher frequencies.
$$
\begin{matrix}
\text{input}\to{H}_0\to{H}_0\to{H}_o\to\text{output}
\end{matrix}
$$
$$
\begin{align*}
H(z)&=\frac{1}{8}(1+z^{-1})^3\\
&=\frac{1}{8}\left(1+3z^{-1}+3z^{-2}+z^{-3}\right)
\end{align*}
$$
	- __symmetry__: even
	- __length__: 4
	- __order__: 3
	- still TYPE 2 FIR FILTER
$$
H\left(e^{j\omega}\right)=e^{-j\omega\tfrac{3}{2}}\cos^3{\left(\frac{1}{2}\omega\right)}
$$
- Plot $$\left|H\left(e^{j\omega}\right)\right|$$
![fig03]()


## LPF FILTER COMPARISON
- Assess filters by looking at “$$3\:\text{dB}$$” point and also looking at shape in passband and stopband.
- Compare previous filter to moving average of same length:
$$
H(z)=\frac{1}{4}\left(1+z^{-1}+z^{-2}+z^{-3}\right)
$$


## BETTER LPF vs. MOVING AVERAGE (same length)
![fig04a]()
![fig04b]()


## SIMPLE HPF
$$
\begin{align*}
H_1(z)&=\frac{1}{2}\left(1-z^{-1}\right)\\
&=\frac{1}{2}\cdot\frac{z-1}{z}\\
H_1\left(e^{j\omega}\right)&=\frac{1}{2}\left(1-e^{-j\omega}\right)\\
&=\frac{1}{2}e^{-j\omega\tfrac{1}{2}} \left(2j\sin{\left(\frac{1}{2}\omega\right)}\right)\\
&=je^{-j\omega\tfrac{1}{2}}\sin{\left(\tfrac{1}{2}\omega\right)}
\end{align*}
$$
- Plot $$\left|H_1\left(e^{j\omega}\right)\right|$$
![fig05]()


## COMB FILTER
- Mulitple passbands and stop bands from prototype filter
$$
G(z)=H(z^{L})
$$
- it has frequency response which is a periodic function of $$\omega$$ with a period of $$\tfrac{2\pi}{L}$$
- __example__:
$$
\begin{align*}
H_0(z)&=\frac{1}{2}(1+z^{-1})\\
G_0(z)&=H_0(z^L)=\frac{1}{2}(1+z^{-L})
\end{align*}
$$
- If $$L=3$$,
$$
\begin{align*}
G_0(z)&=\frac{1}{2}(1+z^{-L})\\
G_0\left(e^{j\omega}\right)&=\frac{1}{2}\left(1+e^{-j3\omega}\right)\\
&=\frac{1}{2}e^{-j\omega\tfrac{3}{2}}\left(2\cos{\left(\frac{3}{2}\omega\right)}\right)\\
&=e^{-j\omega\tfrac{3}{2}}\cos{\left(\frac{3}{2}\omega\right)}
\end{align*}
$$
- Plot $$\left|G_0\left(e^{j\omega}\right)\right|$$ for $$L=3$$.
	- it has 3 reptitions of spectrum for $$H_0\left(e^{j\omega}\right)$$ in $$-\pi$$ to $$\pi$$ range

	- where are the zeros?
	$$
	\begin{align*}
	z^3+1=0;\\
	z^3=-1=e^{-j\pi}e^{j2\pi{k}};\\
	\end{align*}
	$$
	- Think about unit circle— where do the zeros occur? What is the equation that we need to solve?
	$$
	\begin{align*}
	z&=e^{\tfrac{-j\pi}{3}}e^{\tfrac{j2\pi{k}}{3}};\\
	&=e^{-j\pi\tfrac{1}{3}},\:e^{j\pi\tfrac{1}{3}},\:e^{-j\pi}
	\end{align*}
	$$
- What is the difference equation, $$g_0[n]$$?
$$
g_0[n]=\frac{1}{2}\delta[n]+\frac{1}{2}\delta[n-3]
$$
- __NOTE__: this is not the same as a cascade of three $$H_0(z)$$ systems.
- We did this earlier:
$$
\begin{align*}
H(z)&=\frac{1}{8}\left(1+z^{-1}\right)^3\\
&=\frac{1}{8}\left(1+3z^{-1}+3z^{-2}+z^{-3}\right)\\
\end{align*}
$$


## ALL PASS FILTER
- Magnitude of the frequency response is 1, but phase controlled by filter coefficients.
- Consider:
$$
\begin{align*}
H(z)&=\frac{a-z^{-1}}{1-az^{-1}}\\
H\left(e^{j\omega}\right)&=\frac{a-e^{-j\omega}}{1-ae^{-j\omega}}
\end{align*}
$$
- Math...
- If
$$
H\left(e^{j\omega}\right)=\frac{H_N\left(e^{j\omega}\right)}{H_D\left(e^{j\omega}\right)}
$$
- Then,
$$
\begin{align*}
|H_N\left(e^{j\omega}\right)|&=|H_D\left(e^{j\omega}\right)|\\
\theta(\omega)&=\theta_N(\omega)-\theta_D(\omega)\\
\end{align*}
$$
- Phase of numerator:
$$
\theta_N(\omega)=\frac{\sin{\left(\omega\right)}}{a-\cos{\left(\omega\right)}}
$$
- Phase of denominator:
$$
\begin{align*}
\theta_D(\omega)&=\frac{a\sin{\left(\omega\right)}}{1-a\cos{\left(\omega\right)}}\\
&=\frac{\sin{\left(\omega\right)}}{\tfrac{1}{a}-\cos{\left(\omega\right)}}
\end{align*}
$$
- In general,
$$
H(z)=\pm\frac{z^{-M}D_M(z^{-1})}{D_M(z)}
$$
- Why?
	- denominator polynomial:
	$$
	D_M(z)=1+d_1z^{-1}+\cdots+d_{M-1}z^{-(M-1)}+d_Mz^{-M}
	$$
	- numerator polynmial (mirror image):
	$$
	z^{-M}D_M(-z)=d_M+d_{M-1}z^{-1}+\cdots+d_1z^{-(M-1)}+z^{-M}
	$$
- So,
$$
\begin{align*}
H(z)H(z^{-1})&=\frac{z^{-M}D_M(z^{-1})}{D_M(z)}\cdot\frac{z^MD_M(z)}{D_M(z^{-1})}\\
&=1;
\end{align*}
$$


