# lect15

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
- Windowing

## GIBBS PHENOMENON
- Transition from pass-band to stop-band gets narrower — more sharply defined
- Stop-band attenuation does not decrease very fast $$\tfrac{1}{\sin{\left(\tfrac{\omega}{2}\right)}}$$ with frequency, max does not decrease with $$M$$.
- Can we get $$h[n]$$ from $$h_d[n]$$ in another way — *e.g.*, use a smoother window function?
- If we do, what is the cost that we already discussed?
- By using a different window function, we will alter the integral square error to be non-zero within the $$-M$$ to $$M$$ band.
- Perhaps, however, this will do better with respect to pass-band flatness and/or stop-band attenuation – maybe at the expense of transition bandwidth.


## MOVING AVERAGE FILTER
$$
\begin{matrix}
M=1,&\text{length}=3,&\omega_c=\tfrac{pi}{2}
\end{matrix}
$$
![fig01a]()
$$
\begin{matrix}
M=5,&\text{length}=11,&\omega_c=\tfrac{pi}{2}
\end{matrix}
$$
![fig01b]()


## IMPACT OF CONVOLVING WINDOW WITH IDEAL LPF
- When all of main lobe is in passband or all in stop band, the resulting convolution is reasonably smooth, as wc passes through mainlobe we get transition.
![fig02]()


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


