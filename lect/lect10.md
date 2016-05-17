# lect10

## COURSE OVERVIEW: PART 1
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


## Z-transform — generalized DTFT
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


### [EXAMPLE: Z-TRANSFORM(1)][]

### [EXAMPLE: Z-TRANSFORM(2)][]


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

### [EXAMPLE: SOLVING TRANSFER FUNCTION(1)][]

### [EXAMPLE: SOLVING TRANSFER FUNCTION(2)][]


## ANOTHER TRICK TO PLOTTING
- Take point on unit circle
- Take product of lengths to __zeros__ as numerator
- Take product of lengths to __poles__ as denominator
- This is the value at the location of the original point that you start with...

### [EXAMPLE: SOLVING TRANSFER FUNCTION(3)][]

### [EXAMPLE: SOLVING TRANSFER FUNCTION(4)][]

## OTHER WAYS TO COMUTE INVERSE Z-TRANSFORM
- partial fraction expansion
- long division of polynomials

### [EXAMPLE: PARTIAL FRACTION EXPANSION][]

### [EXAMPLE: LONG DIVISION OF POLYNOMIALS][]

## STABILITY
- Unit circle must be included in the region of convergence for DTFT to exist.
- Poles CANNOT be included in the region of convergence for stability.
- Thus, if poles are INSIDE the unit circle, causal system is BIBO stable.
- Converse is also true...

### [EXAMPLE: ANALYSIS][]

## MORE BIG PICTURE
![]


