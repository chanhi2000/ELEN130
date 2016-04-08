# ex05b

## 2
__EXAMPLE: SOLVING THE LCCDE__

### COMPLEMENTARY SOLUTION
Given
$$
y[n]+y[n-1]-6y[n-2]=x[n]
$$
We are interested in knowing the closed solution for input $$x[n]=8\mu[n]$$ with initial conditions: $$y[-1]=y[-2]=1$$. Find the complementary solution, $$y_c[n]$$ by assuming it has the form $$\lambda^n$$.
$$
\begin{align*}
\sum_{k=0}^{N}{\left(d_ky[n-k]\right)}&=\sum_{k=0}^{N}{\left(d_k\lambda^{n-k}\right)}\\
&=\lambda^{n-N}\left(d_0\lambda^{N}+d_1\lambda^{N-1}+\cdots+d_{N-1}\lambda+d_N\right)=0
\end{align*}
$$
So 
$$
\begin{matrix}
\lambda^n+\lambda^{n-1}-6\lambda^{n-2}=\lambda^{n-2}\left(\lambda^2+\lambda-6\right)
\left\{\begin{matrix}
\lambda_1=-3;\\
\lambda_2=2;
\end{matrix}\right\}
\end{matrix}
$$
Thus
$$
y_c[n]=\alpha_1(-3)^n+\alpha_2(2)^n
$$

### PARTICULAR SOLUTION
For particular solution, assume $$y_p[n]=\beta$$, substituting yields
$$
\beta+\beta-6\beta=8\mu[n]
$$
So
$$
\begin{matrix}
\beta=-2\mu[n]&\text{for }n\geq0
\end{matrix}
$$
Then $$\beta=-2$$.

Final form gives you
$$
y[n]=\alpha_1(-3)^n+\alpha_2(2)^n-2
$$
Using initial conditions, we solve for $$\alpha_1$$ and $$\alpha_2$$ and get:
$$
\begin{matrix}
y[n]=-1.8(-3)^n+4.8(2)^n-2&\text{for }n\geq0
\end{matrix}
$$

### IMPULSE RESPONSE
The impulse response can be computed from the complementary solution:
$$
y_c[n]=\alpha_1\lambda_1^{n}+\alpha_2\lambda_2^{n}+\alpha_3\lambda_3^{n}+\cdots+\alpha_N\lambda_N^{n}
$$
by determining the constants that satisfy the $$0$$ initial conditions!

So
$$
\begin{align*}
h[n]&=y_c[n]=\alpha_1(-3)^n+\alpha_2(2)^n\\
h[0]&=\alpha_1(-3)^0+\alpha_2(2)^0-=\alpha_1+\alpha_2\\
h[1]&=\alpha_1(-3)^1+\alpha_2(2)^1-=-3\alpha_1+2\alpha_2\\
\end{align*}
$$

From the original equation $$y[n]+y[n-1]-6y[n-2]=x[n]$$, we can determine $$h[0]$$ and $$h[1]$$ to solve for $$\alpha_1$$ and $$\alpha_2$$. In this case,
$$
\begin{matrix}
h[0]=1\\
h[1]+h[0]=0
\end{matrix}
$$
Solving yields:
$$
h[n]=0.6(-3)^n+0.4(2)^n
$$