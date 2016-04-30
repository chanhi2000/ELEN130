# lect09

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
- `fftshift()`
- DFT
- Circular Convolution
- Overlap/Add and Overlap/Save


## `fftshift`


## DFT: VIEWING $$W_N$$ around the unit circle.
- To compute $$X[k]$$ take $$k$$ steps around the unit circle to get factors to multiply.
- If $$N=4$$, there will be $$4$$ steps around the unit circle:
![fig01](lect09/lect09-fig01.png)


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
$$
X[-1]=X[N-1]
$$

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

## CIRCULAR CONVOLUTION
- .$$N$$ point convolution is often represented as an $$N$$ with a circle around it.
- Circular Convolution is commutative.
- Tabular method:

| $$n$$ | $$g[n]$$ | $$h[n]$$ | $$g[n]\underset{4}{\otimes}h[n]$$ |
| :---: | :------: | :------: | :-------------------------------- |
| $$0$$ | $$g[0]$$ | $$h[0]$$ | $$g[0]h[0]$$ |
| $$1$$ | $$g[1]$$ | $$h[1]$$ | $$g[1]h[0]+g[0]h[1]$$ |
| $$2$$ | $$g[2]$$ | $$h[2]$$ | $$g[2]h[0]+g[1]h[1]+g[0]h[2]$$ |
| $$3$$ | $$g[3]$$ | $$h[3]$$ | $$g[3]h[0]+g[2]h[1]+g[1]h[2]+g[0]h[3]$$ |
| $$\left<4\right>_4$$ | | | $$g[3]h[1]+g[2]h[2]+g[1]h[3]$$ |
| $$\left<5\right>_4$$ | | | $$g[3]h[2]+g[2]h[3]$$ |
| $$\left<6\right>_4$$ | | | $$g[3]h[3]$$ |

$$
\begin{align*}
X[0]&=g[0]h[0]+\left(g[3]h[1]+g[2]h[2]+g[1]h[3]\right)\\
X[1]&=g[1]h[0]+g[0]h[1]+\left(g[3]h[2]+g[2]h[3]\right)\\
X[2]&=g[2]h[0]+g[1]h[1]+g[0]h[2]+\left(g[3]h[3]\right)\\
X[3]&=g[3]h[0]+g[2]h[1]+g[1]h[2]+g[0]h[3]\\
\end{align*}
$$


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
| Duality | $$G[n]$$ | $$N\left(g[\left<-k\right>_N]\right)$$ |
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


