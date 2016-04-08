# ex04g

## 7
__EXAMPLE: SHIFT-INVARIANCE TEST__

Is an up-sampler shift-invariant?

Consider an up-sampler:
$$
x_u[n]=
\begin{cases}
x\left[\frac{n}{L}\right],&n=0,\:\pm{L},\:\pm2L,\cdots\\
0,&\text{otherwise}
\end{cases}
$$

For an input, $$x_1[n]=x[n-n_0]$$, the output is given by
$$
\begin{align*}
x_{1,u}[n]&=
\begin{cases}
x_1\left[\frac{n}{L}\right],&n=0,\:\pm{L},\:\pm2L,\cdots\\
0,&\text{otherwise}
\end{cases}\\
&=
\begin{cases}
x_1\left[\frac{n-Ln_0}{L}\right],&n=0,\:\pm{L},\:\pm2L,\cdots\\
0,&\text{otherwise}
\end{cases}
\end{align*}
$$

This does  not match the shifted up-sampling – thus, up-sampler is a __time-varying system__!


