# mt01c

## SECTION 3:
Discrete-Time Systems

### 3(a)
__Linear and Shift-Invariance__: For the signal $$y[n]=nx[n+2]+3$$

#### 3a(i)
Determine if the system is linear and justify your answer.
- Verify, if this is true.
$$
\begin{matrix}
x[n]&\to&y[n]\\
\alpha{x}_1[n]+\beta{x}_2[n]&\to&\alpha{y}_1[n]+\beta{y}_2[n]\\
\end{matrix}
$$
- Given, $$y[n]=nx[n+2]+3$$
$$
\begin{align*}
y[n]&=\alpha{y}_1[n]+\beta{y}_2[n]\\
&=\alpha\left(nx_{1}[n+2]+3\right)+\beta\left(nx_{2}[n+2]+3\right)\\
&=n\left(\alpha{x}_1[n+2]\right)+3\alpha+n\left(\beta{x}_2[n+2]\right)+3\beta\\
&=n\left(\alpha{x}_1[n+2]+\beta{x}_2[n+2]\right)+3\left(\alpha+\beta\right)\\
&=n\left(x[n]\right)+3\left(\alpha+\beta\right);
\end{align*}
$$
- This system is __NOT__ linear.

#### 3a(ii)
Determine if the system is shift-invariant and justify your answer.

- Verify, if this is true.
$$
\begin{matrix}
x[n]&\to&y[n]\\
x_1[n-n_0]&\to&y_1[n-n_0]\\
\end{matrix}
$$
- Given, $$y[n]=nx[n+2]+3$$
$$
\begin{align*}
y[n]&=y[n-n_0]\\
&=(n-n_0)x[n-n_0+2]+3\\
&=nx[n-n_0+2]-n_0x[n-n_0+2]+3\\
\end{align*}
$$
- This system is __NOT__ shift-invariant.

### 3(b)
__Interconnected system__: YOu are told an LTI system has an impulse reponse
$$
h[n]=(h_1[n]\otimes{h}_2[n])-(h_3[n]\otimes{h}_4[n])
$$
You have access to system blocks labeled with the individual impulse responses, $$h_1[n]$$, $$h_2[n]$$, $$h_3[n]$$, and $$h_4[n]$$.  Draw a complete system to realize the impulse response as described.

![fig01](mt01/mt01c-fig01.png)

### 3(c)
__Block Diagram Analysis__: Write the difference equation that describeds the block diagram below.

![fig02](mt01/mt01c-fig02.png)

$$
y[n]=y[n-1]+2x[n-3]-x[n-2]+2x[n];
$$


### 3(d)
__Impulse Reponse__: For the block diagram in __3(c)__, write a mathematical expression (in terms of deltas and/or step functions) for the impulse response. This expression should NOT be recrusive — in other words, it should not include an $$h[n]$$ term. Assume $$\begin{matrix}y[n]=0&\text{for }n<0\end{matrix}$$.
$$
\begin{align*}
y[n]&=\begin{cases}
y[n-1]+2x[n-3]-x[n-2]+2x[n]&\text{for }n\geq0\\
0&\text{for }n<0
\end{cases}\\\\
y[-1]&=0;\\
y[0]&=y[-1]+2x[-3]-x[-2]+2x[0]\\
&=2x[n-3]-x[n-2]+2x[n]\\
h[0]&=2\delta[-3]-\delta[-2]+2\delta[0]\\
&=2\\\\
y[1]&=y[0]+2x[-2]-x[-1]+2x[1]\\
&=2+(2x[-2]-x[-1]+2x[1])\\
h[1]&=2+(2\delta[-2]-\delta[-1]+2\delta[1])\\
&=2\\\\
y[2]&=y[1]+2x[-1]-x[0]+2x[2]\\
&=2+(2x[-1]-x[0]+2x[2])\\
h[1]&=2+(2\delta[-1]-\delta[0]+2\delta[2])\\
&=1\\\\
y[3]&=y[2]+2x[0]-x[1]+2x[3]\\
&=1+(2x[0]-x[1]+2x[3])\\
h[1]&=1+(2\delta[0]-\delta[1]+2\delta[3])\\
&=3\\\\
y[4]&=y[3]+2x[1]-x[2]+2x[4]\\
&=3+(2x[1]-x[2]+2x[4])\\
h[1]&=3+(2\delta[1]-\delta[2]+2\delta[4])\\
&=3\\\\
y[5]&=\cdots
\end{align*}
$$
Therefore,
$$
h[n]&=2\delta[n]+2\delta[n-1]+\delta[n-2]+3\mu[n-3]\\\\
&=2\mu[n]-mu[n-2]+2\mu[n-3];
$$


### 3(e)
__Response to an input__: Assume you've determined the closed solution for the impulse response of an LTI system to be $$h[n]=\left(\tfrac{1}{2}\right)^n\mu[n-2]$$. Determine a mathematical expression for the output, $$y[n]$$, for the input $$x[n]=\mu[n-1]$$. Simplify as much as possible. (This could prove more tedious than most of the other problems. If you are having trouble simplifying, set it up and explain what is happening as best you can. Understanding the set up and what is being asked is more important than being able to simplify to a final answer.)

$$
\begin{align*}
h[n]&=\left(\frac{1}{2}\right)^n\mu[n-2]\\
x[n]&=\mu[n-1]\\\\
y[n]&=x[n]\otimes{h}[n]\\
&=(\mu[n-1])\otimes\left(\left(\frac{1}{2}\right)^n\mu[n-2]\right)\\
&=\sum_{k=-\infty}^{\infty}{\mu[k-1]\left(\frac{1}{2}\right)^n\mu[n-(k-2)]}
\end{align*}
$$

### 3(f)
__Response to a new input__: An LTI system takes an input, $$x[n]$$, yields an output $$y[n]$$. In terms of $$y[n]$$, what is the output to $$x_1[n]=x[n]-x[n-4]$$