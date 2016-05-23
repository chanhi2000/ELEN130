# lect10

## COURSE OVERVIEW: 
- ~~Discrete-Time Signals in the Time Domain~~
	- ~~Operations, Classifications, Sampling~~
- ~~Discrete-Time Systems~~
	- ~~Impulse/Step Responses, LTI Classification, Stability, Response to sinusoidal input~~
- ~~Discrete-Time Signals in the Frequency Domain~~
	- ~~Transforms, Applications, Sampling and reconstruction~~
- ~~Finite-Length Discrete Transforms~~
	- ~~DFT, FFT, Zero-padding, Fourier Domain filtering~~, __Linear and Circular convolution__
- __Z-transform__
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
- Overlap/Add and Overlap/Save
- Z-transform


## EXAMPLE TO MOTIVATE NEED FOR Z-TRANSFORM
- Recall this difference equation
$$
y[n]=x[n]+ay[n-1]
$$
- Which yielded the impulse response:
$$
h[n]=a^{n}\mu[n]
$$
- Evaluating this impulse response, we can attempt to create the Frequency Response
$$
\begin{align*}
H\left(e^{j\omega}\right)&=\sum_{n=-\infty}^{\infty}{h[n]e^{-j\omega{n}}}\\
&=\lim_{N\to\infty}{\sum_{n=0}^{N-1}{a^ne^{-j\omega{n}}}}\\
&=\lim_{N\to\infty}{\frac{1-\left(ae^{-j\omega}\right)^N}{1-ae^{-j\omega}}}
\end{align*}
$$
- If $$|a|<1$$, then this reduces to $$\frac{1}{1-ae^{-j\omega}}$$
-  If we had started with
$$
H\left(e^{j\omega}\right)=\frac{1}{1-ae^{-j\omega}}
$$
- we would take the inverse and get
$$
h[n]=a^n\mu[n]
$$
- And would implement as:
$$
\begin{align*}
y[n]&=\sum_{k=-\infty}^{\infty}{h[k]x[n-k]}\\
&=\sum_{k=0}^{\infty}{a^kx[n-k]}
\end{align*}
$$
- Then, of course, if we were to implement it, we'd use only a finite number of coefficients resulting in two downsides:
	- only approximates the desried response
	- takes a lot longer than the original difference equation
- We need another approach


## Z-TRANSFORM — GENERALIZED DTFT
$$
H\left(e^{j\omega}\right)=\sum_{n=-\infty}^{\infty}{h[n]e^{-j\omega{n}}}
$$
- This may or may not converge.
- The Z-transform:
$$
H(z)=\sum_{n=-\infty}^{\infty}{h[n]z^{-n}}
$$
- This can be viewed as a simple substitution. If $$z=e^{j\omega}$$, then we get the DTFT
- Say, instead, if $$z=re^{j\omega}$$, then the Z-transform is:
$$
H(re^{j\omega})=\sum_{n=-\infty}^{\infty}{h[n]r^{-n}e^{-j\omega{n}}}
$$
- This is the DTFT of $$h[n]r^{-n}$$
- It is easy to define a region of convergence — i.e. values of $$r$$ that will result in convergence.
- To summarize, Z-transform is defined as
$$
H(z)=\sum_{n=-\infty}^{\infty}{h[n]z^{-n}}
$$
and when $$z=e^{j\omega}$$, we get the DTFT as we force $$z$$ to the unit circle.

### EXAMPLE: Z-TRANSFORM(1)
$$
\begin{align*}
h[n]&=a^{n}\mu[n]\\
H(z)&=\sum_{n=-\infty}^{\infty}{h[n]z^{-n}}\\
&=\sum_{n=-\infty}^{\infty}{a^{n}\mu[n]z^{-n}}\\
&=\sum_{n=0}^{\infty}{a^nz^{-n}}\\
&=\lim_{L\to\infty}{\sum_{n=0}^{L-1}{(az^{-1})^n}}\\
&=\lim_{L\to\infty}{\frac{1-(az^{-1})^L}{1-az^{-1}}}\\
&=\frac{1}{1-az^{-1}},&&\text{for }|a|<|z|
\end{align*}
$$
- Plotting this, we have a Region Of Convergence (ROC) outside a circle with radius “$$a$$”
- In general, a causal sequence (right-sided sequence starting at/after n=0) will have a ROC outside a circle.
- ALWAYS NEED TO SPECIFY A ROC WITH A Z-TRANSFORM!


### EXAMPLE: Z-TRANSFORM(2)
$$
\begin{align*}
h[n]&=-a^n\mu[-n-1]\\
H(z)&=\sum_{n=-\infty}^{\infty}{h[n]z^{-n}}\\
&=\sum_{n=-\infty}^{\infty}{-a^{n}\mu[-n-1]z^{-n}}\\
&=\sum_{n=-\infty}^{-1}{-a^nz^{-n}},&&\left<(-n-1)\geq0\right>\\
&=\sum_{m=1}^{\infty}{-a^{-m}z^{m}}\\
&=\sum_{m=1}^{\infty}{-(a^{-1}z)^m}\\
&=\lim_{L\to\infty}{\sum_{m=1}^{L-1}{(a^{-1}z)^m}}\\
&=\lim_{L\to\infty}{\frac{(a^{-1}z)-(a^{-1}z)^L}{1-(a^{-1}z)}}\\
&=\frac{(a^{-1}z)}{1-(a^{-1}z)}\\
&=\frac{1}{1-az^{-1}}\\,&&\text{for }|a|>|z|
\end{align*}
$$
- This is the same functional form as the previous causal sequence, but, the region of convergence (ROC) is opposite. The ROC is inside the circle of radius “$$a$$”
- In general, left-sided sequences will have a ROC inside a circle.


## ROC
- If ROC includes the unit circle, the DTFT exists.
- For the first example, this happens if $$|a|<1$$; for the second example, it happens if $$|a|>1$$.


## INVERSE Z-TRANSFORM
$$
\begin{align*}
H\left(e^{j\omega}\right)&=\sum_{n=-\infty}^{\infty}{h[n]e^{-j\omega{n}}}\\
h[n]&=\frac{1}{2\pi{j}}\oint{H(z)z^{n-1}dz}
\end{align*}
$$
- Three (generally) more attractie options for finding $$h[n]$$
	1. Find a forward transform that matches
	2. Partial Fraction Expansion
	3. Long Division of Polynomial
- \#2 and \#3 are expected to be review... but we will cover if we have time
- __Side Note__:
$$
\begin{align*}
z&=re^{j\omega}\\
\frac{dz}{dw}&=rje^{j\omega}\\
dz&=j\left(re^{j\omega}\right)dw\\
&=jzdw
\end{align*}
$$


## Z-TRANSFORM vs. DTFT
- DTFT can only do FIR design
- Z-transform can do FIR or IIR design!
- Z-transform is very powerful for representing systems defined by linear constant coefficient difference equations (LCCDEs)


## SOME Z-TRANSFORMS
$$
\begin{align*}
\delta[n]&\overset{\text{z}}{\longrightarrow}1&&\\\\
\delta[n-n_0]&\overset{\text{z}}{\longrightarrow}z^{-n_0}&&\text{ROC: }|z|>0\text{ for }n_0>0\\\\
a^{n}\mu[n]&\overset{\text{z}}{\longrightarrow}\frac{z}{z-a}&&\text{ROC: }|z|>|a|
\end{align*}
$$


## Z-TRANSFORM SHIFT PROPERTY
Assume
$$
x[n]\overset{\text{z}}{\longrightarrow}X(z)
$$
then
$$
x[n-n_0]\overset{\text{z}}{\longrightarrow}\left(z^{-n_0}\right)X(z)
$$
So
$$
\begin{align*}
x[n-1]&\overset{\text{z}}{\longrightarrow}\left(z^{-1}\right)X(z)\\\\
x[n-2]&\overset{\text{z}}{\longrightarrow}\left(z^{-2}\right)X(z)\\\\
y[n-1]&\overset{\text{z}}{\longrightarrow}\left(z^{-1}\right)Y(z)
\end{align*}
$$


## USING Z-TRANSFORM ON A LCCDE
- Say we have the following difference equation:
$$
y[n]=b_0x[n]+b_1x[n-1]+a_1y[n-1]
$$
has the following Z-transform:
$$
Y(z)=b_0X(z)+b_1\left(z^{-1}\right)X(z)+a_1\left(z^{-1}\right)Y(z)
$$
- As with other transforms, we have
$$
Y(z)=X(z)H(z)
$$
(this can be verified by looking at convolution)
- So, the transfer function is:
$$
H(z)=\frac{Y(z)}{X(z)}
$$
- Thus, we have to solve the expression above for this relationship


## SOLVING FOR THE TRANSFER FUNCTION
$$
Y(z)=b_0X(z)+b_1\left(z^{-1}\right)X(z)+a_1\left(z^{-1}\right)Y(z)
$$
- Grouping results in:
$$
\left(1-a_1 \left(z^{-1}\right)\right)Y(z)=\left(b_0+b_1\left(z^{-1}\right)\right)X(z)
$$
- So,
$$
H(z)=\frac{\left(b_0+b_1\left(z^{-1}\right)\right)}{\left(1-a_1\left(z^{-1}\right)\right)}
$$
- In general, we end up with:
$$
H(z)=\frac{\sum_{k=0}^{M-1}{b_k\left(z^{-k}\right)}}{1+\sum_{l=1}^{N-1}{a_l\left(z^{-1}\right)}}
$$
- This is a complex-valued function of a complex variable, $$z$$.
	- __Zeroes__ of $$H(z)$$: roots of the numerator
	- __Poles__ of $$H(z)$$: roots of the denominator


### EXAMPLE: OUR ORIGINAL EXAMPLE WITH NEW APPROACH
$$
\begin{align*}
y[n]&=x[n]+ay[n-1]\\
Y(z)&=X(z)+aY(z)z^{-1}\\
Y(z)\left(1-a_1z^{-1}\right)&=X(z)\\
H(z)&=\frac{Y(z)}{X(z)}=\frac{1}{\left(1-a_1z^{-1}\right)}\\
&=\frac{z}{z-a}
\end{align*}
$$
- __zeros__: $$0$$
- __poles__: $$a$$
- For a causal system, ROC is,
$$
|z|>|a|
$$
If $$|a|<1$$, this includes the unit circle (*i.e. stable system*)
- This condition would also mean that the DTFT exists
- Can we sketch $$\left|H\left(e^{j\omega}\right)\right|$$ from the poles and zeros?
- Are there any zeros on the unit circle?
- Where is $$\tfrac{z}{z-a}$$ max or min?


### SKETCHING DTFT FROM Z-TRANSFORM
$$
H(z)=\frac{z}{z-a}
$$
- @ $$z=1$$, (*i.e.* @ $$\omega=0$$),
$$
|z|=1;
$$
- min: $$|z-a|$$ if $$0<a<1$$
- @ $$z=-1$$, (*i.e.* @ $$\omega=\pi$$),
$$
|z|=1;
$$
- max: $$|z-a|$$ if $$0<a<1$$
- Let’s sketch the magnitude of the frequency response


### RELEVANT PLOTS FOR PREVIOUS EXAMPLE
$$
\begin{align*}
H(z)&=\frac{z}{z-a},&&a=0.25
\end{align*}
$$

![fig01a]()
![fig01b]()

### ANOTHER EXAMPLE
$$
y[n]=\underset{\text{3-point moving average}}{\frac{1}{3}\left(x[n]+x[n-1]+x[n-2]\right)}
$$
So
$$
\begin{align*}
Y(z)&=\frac{1}{3}X(z)\left(1+z^{-1}+z^{-2}\right)\\
H(z)&=\frac{1}{3}\left(1+z^{-1}+z^{-2}\right)\\
&=\frac{1}{3}\left(\frac{1-z^{-3}}{1-z^{-1}}\right)\\
&=\frac{1}{3}\left(\frac{z^{3}-1}{z^{2}(z-1)}\right)\\
\end{align*}
$$
- So, what are __poles__ and __zeros__ of this?
	- __poles__: $$z=0,\:0,\:1$$
	- __zeros__: $$z^{3}=1$$
- To determine zeros, recall that 
$$
\begin{matrix}
1=(1)e^{j2\pi{k}},&\forall{k}\in\mathbb{I}
\end{matrix}
$$


### FINDING POLES AND ZEROS
- So, cube root of both sides reveals that 
$$
1=(1)e^{j2\pi\tfrac{k}{3}}
$$
there are three unique solutions here for $$k=0,\:1,\:2$$. ($$k=3$$ starts the cycle over and reveals the same quantity.)
- Look at the unit circle for the zeros and poles. Note that the zero and pole at 1 cancel each other out.
- So, the remaining zeros occur at $$e^{j2\pi\tfrac{1}{3}}$$ and $$e^{j2\pi\tfrac{2}{3}}$$ (this second one is at $$
e^{-j2\pi\tfrac{1}{3}}$$)
- Like last time, can we roughly sketch the DTFT (frequency response) based on poles and zeros?

### RELEVANT PLOTS FOR PREVIOUS EXAMPLE
$$
H(z)=\frac{1}{3}\left(1+z^{-1}+z^{-2}\right)
$$

![fig02a]()
![fig02b]()

### PLOTTING BASED ON ZEROS AND POLES AGAIN
- Alternative way to find zeros and poles...
$$
\begin{align*}
H(z)&=\frac{1}{3}\left(1+z^{-1}+z^{-2}\right)\\
&=\frac{1}{3}\frac{\left(z^2+z+1\right)}{z^2}
\end{align*}
$$
	- __poles__: $$z=0,\:0$$
	- __zeros__: $$z=\frac{1}{2}\pm{j}\frac{\sqrt{3}}{2}$$
- __Look at unit circle__: convince yourself, it's the same as the previous listed zeros: $$e^{j2\pi\tfrac{1}{3}}$$ and $$e^{j2\pi\tfrac{2}{3}}$$.


## ANOTHER TRICK TO PLOTTING
- Take point on unit circle
- Take product of lengths to __zeros__ as numerator
- Take product of lengths to __poles__ as denominator
- This is the value at the location of the original point that you start with...

### ANOTHER EXAMPLE
- What about the 5 point moving average? Use the same method.
$$
y[z]=\frac{1}{5}\left(x[n]+x[n-1]+x[n-2]+x[n-3]+x[n-4]\right)
$$
So,
$$
Y(z)=\frac{1}{5}X(z)\left(1+z^{-1}+z^{-2}+z^{-3}+z^{-4}\right)\\
$$
Thus,
$$
\begin{align*}
Y(z)&=\frac{1}{5}\sum_{n=0}^{4}{z^{-n}}\\
&=\frac{1}{5}\left(\frac{1-z^{-5}}{1-z^{-1}}\right)\\
&=\frac{1}{5}\left(\frac{z^5-1}{z^4(z-1)}\right),&&\text{ROC:}\:\:|z|>0\\
\end{align*}
$$
	- __poles__: $$\begin{matrix}
	z=e^{j2\pi\tfrac{k}{5}},&\text{for }k=0,\:1,\:2,\:3,\:4
	\end{matrix}$$
	- __zeros__: $$z=0,\:0,\:0,\:0,1$$
- Sketch...


### RELEVANT PLOTS FOR PREVIOUS EXAMPLE
$$
Y(z)=\frac{1}{5}X(z)\left(1+z^{-1}+z^{-2}+z^{-3}+z^{-4}\right)
$$

![fig03a]()
![fig03b]()

### DESIGN EXAMPLE
- Design a filter to cancel $$3000\:\text{Hz}$$ in an input sampled at $$8000\:\text{Hz}$$.
$$
\begin{align*}
F_T&=8000,\\
f_0&=3000,\\
\omega_0&=2\pi\frac{f_0}{F_T}\\
&=2\pi\frac{(3000)}{(8000)}\\
&=\frac{3\pi}{4}
\end{align*}
$$
- To achieve this, we could put two zeros at $$\pm\omega_0$$, *i.e.* $$z=e^{\pm{j}\omega_0}$$
- Thus
$$
\begin{align*}
H(z)&=\frac{(z-z_1)(z-z_2)}{z^2}\\
&=\frac{(z-e^{-j\pi\tfrac{3}{4}})(z-e^{+j\pi\tfrac{3}{4}})}{z^2}\\
&=\frac{z^2-2\cos{\left(\tfrac{3\pi}{4}\right)}+1}{z^2}\\
&=1-\left(2\cos{\left(\frac{3\pi}{4}\right)}\right)z^{-1}+z^{-2}\\
&=1+\sqrt{2}z^{-1}+z^{-2}
\end{align*}
$$
- From here, we can create a difference equation:
$$
h[n]=\delta[n]+\sqrt{2}\delta[n-1]+\delta[n-2]
$$
or
$$
y[n]=x[n]+\sqrt{2}x[n-1]+x[n-2]
$$

### RESULTS OF DESIGN EXAMPLE
$$
y[n]=x[n]+\sqrt{2}x[n-1]+x[n-2]
$$

![fig04a]()
![fig04b]()


### IS THIS DESIGN OKAY?
- It’s not great, but no other constraints are given. We zeroed out $$3\:\text{kHz}$$. So, it’s acceptable.
- For the rest of the quarter, we will look at ways to design filters with overall frequency specification using pole and zero placement.
- __Notch filter__:
$$
H(z)=\frac{\left(z-e^{-j\omega_0}\right)\left(z-e^{+j\omega_0}\right)}{\left(z-\rho{e}^{-j\omega_0}\right)\left(z-\rho{e}^{+j\omega_0}\right)}
$$
	- Complex conjugate zeros and poles.
	- Coefficients of the polynomial are __real__.
	- Difference equation has __real__ coefficients.
-  Remember that $$|H(z)|$$ is the product of the magnitude of the distanc0e from a point on the unit circle to the zeros devided by the product of the magnitude of the distance from the point to the poles.  For most of the unit cirle, this is about $$1$$... But, at $$\pm\omega_0$$, this is closer to $$0$$.