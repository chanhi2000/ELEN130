# mt02d

## 4.
__Overlap-add, Overlap-save, and Linear/Circular Convolution__

You are tolde that an input sequence $$x[n]=[0,\:1,\:2,\:3,\:4,\:5,\:6,\:7,\:8,\:9,\:10,\:11,\:12,\:13,\:\cdots]$$ (note, this starts at $$n=0$$) will be going into a system that has an impulse response of $$h[n]=[1,\:0,\:-1]$$.

For question 4(a)-(b), we will use the __overlap-add__ method with $$N=6$$.

### 4(a)
Using the definition provided in lecture, what is the sub-sequence $$x_1[n]$$?
Given
$$
\begin{matrix}
x_m[n]=x[n+mN],&0\leq{n}\leq{N}-1
\end{matrix}
$$
we can find $$x_1[n]$$ by 
$$
\begin{align*}
x_1[n]&=x[n+(1)(6)],&&0\leq{n}\leq{6}-1\\
&=x[n+6],&&0\leq{n}\leq5\\
&=[6,\:7,\:8,\:9,\:10,\:11]\\
\end{align*}
$$

### 4(b)
Assuming that the DFT method of determining convolution is used, what length DFTs and invers DFTs are used to determine $$y_1[n]$$?

Overlap-add uses linear convolution.
For example, if we convolve
$$
\begin{align*}
x_1[n]&=[6,\:7,\:8,\:9,\:10,\:11]\\
h[n]&=[1,\:0,\:-1]
\end{align*}
$$
The length of $$y_1[n]$$ should be
$$
\begin{align*}
\operatorname{length}(y_1)&=\operatorname{length}(x_1)+\operatorname{length}(h)-1\\
&=(6)+(3)-1\\
&=8;
\end{align*}
$$


For questions 4(c)-(e), we will use the __overlap-save__ method with $$N=6$$.

### 4(c)
Using the definition provided in lecture, what is the sub-sequence $$x_1[n]$$?
Given
$$
\begin{matrix}
x_m[n]=x[n+m(N-M+1)],&0\leq{n}\leq{N}-1
\end{matrix}
$$
we can find $$x_1[n]$$ by 
$$
\begin{align*}
x_1[n]&=x[n+(1)(6-3+1)],&&0\leq{n}\leq{6}-1\\
&=x[n+4m],&&0\leq{n}\leq5\\
&=[4,\:5,\:6,\:7,\:8,\:9]\\
\end{align*}
$$

### 4(d)
How many terms of $$y_1[n]$$ will be kept in the final output $$y[n]$$?

When $$y_1[n]$$ is computed
$$
y_1[n]=[\underset{M-1\:\text{ gone }}{\underline{A,\:B}},\:C,\:D,\:E,\:\cdots]
$$
So, compute how much terms are kept
$$
\begin{align*}
\therefore\:6-(M-1)&=6-(3-1)\\
&=4;
\end{align*}
$$


### 4(e)
Even though it may be rejected, find the first term of $$y_1[n]$$, *i.e.* $$y_1[0]$$.

Use Cyclic Prefix Method to quickly compute the circular convolution

$$
\begin{align*}
x_1[n]&=[4,\:5,\:6,\:7,\:\underset{\text{take }M-1}{\underline{8,\:9}}]\\
x_1^\prime[n]&=[\underset{\text{append}}{\underline{8,\:9}},\:4,\:5,\:6,\:7,\:8,\:9]\\
h&=[1,\:0,\:-1]\\\\
y_1[n]&=[8,\:9,\:\underset{\text{take }N\text{ center}}{\underline{-4,\:-4,\:2,\:2,\:2,\:2}},\:-8,\:-9]\\
\therefore\:y_1[0]&=-4
\end{align*}
$$
