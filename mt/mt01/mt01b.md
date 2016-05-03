# mt01b

## SECTION 2:
__Finite Length Operation, Energy/Power and Fundamental Period__

### 2(a)
__Circular time reversal and shift__: You are given the following sequence for $$ \begin{matrix}n=0:7&\left\{0,\:1,\:2,\:3,\:4,\:5,\:6,\:7\right\}\end{matrix}$$.

Recall, the circular time reversal is represented as this:
$$
\{y[n]\}=\{x[\left<-n\right>_8]\}
$$
and, the circular shift is represented as this:
$$
\{x_c[n]\}=\{x[\left<n-n_0\right>_8]\}
$$
We wish to combine these two operations into one and we define a circular shifted and refersed signal as:
$$
\{y_c[n]\}=\{x[\left<n-n_0\right>_8]\}
$$
For $$n_0=-13$$, list the $$8$$ values that are now in the slots $$n=0:7$$.
$$
\begin{align*}
\{y_c[n]\}&=\{x[\left<-n-13\right>_8]\}\\
&=\{x[\left<-(n+13)\right>_8]\}\\
\end{align*}
$$

| $$n$$ | $$\left<-(n+13)\right>_8$$ | $$y_c[n]$$ |
| :---: | :------------------------: | :--------: |
| $$0$$ | $$-13+8(2)$$ | $$x[3]=3$$ |
| $$1$$ | $$-14+8(2)$$ | $$x[2]=2$$ |
| $$2$$ | $$-15+8(2)$$ | $$x[1]=1$$ |
| $$3$$ | $$-16+8(2)$$ | $$x[0]=0$$ |
| $$4$$ | $$-17+8(3)$$ | $$x[7]=7$$ |
| $$5$$ | $$-18+8(3)$$ | $$x[6]=6$$ |
| $$6$$ | $$-19+8(3)$$ | $$x[5]=5$$ |
| $$7$$ | $$-20+8(3)$$ | $$x[4]=4$$ |

$$
\therefore\:\begin{matrix}
\{y_c[n]\}=\{3,\:2,\:1,\:0,\:7,\:6,\:5,\:4\}
\end{matrix}
$$

### 2(b)
__Energy/Power__

#### 2b(i)
Sketch the signal $$p[n]=\sum_{k=0}^{\infty}{\delta[n-2k]}$$. Label appropriately enough so it is clear that you understand what this signal looks like.
$$
p[n]=\sum_{k=0}^{\infty}{\delta[n-2k]};
$$
![fig02](mt01/mt01-fig02.png)


#### 2b(ii)
Is $$p[n]$$, as described in __2b(i)__ , an energy signal or a power signal? If it is an energy signal, calculate the energy. If it is a powre signal, calculate the power.
$$
\begin{align*}
P_p&=\lim_{k\to\infty}{\frac{1}{2k+1}\sum_{n=-k}^{k}{\left|p[n]\right|^2}}\\
&=\lim_{k\to\infty}{\frac{1}{2k+1}\sum_{n=0}^{k}{\left|p[n]\right|^2}}\\
&=\lim_{k\to\infty}{\frac{1}{2k+1}\left(\frac{k}{2}+1\right)}\\
&\overset{L'H}{=}\lim_{k\to\infty}{\frac{\tfrac{k}{2}}{2k}}\\
&=\frac{1}{4};
\end{align*}
$$


### 2(c)
__Fundamental Period__: What is the fundamental period of the signal:
$$
x[n]=2\cos{\left(1.4\pi{n}\right)}-\sin{\left(1.2\pi{n}\right)}+\cos{\left(0.5\pi{n}\right)}
$$ 

List the frequencies
$$
\begin{matrix}
\omega_{0,a}=1.4\pi&\omega_{0,b}=1.2\pi&\omega_{0,c}=0.5\pi
\end{matrix}
$$

Find the period for each.
- .$$N_a$$
$$
\begin{align*}
\omega_{0,a}N_a&=2\pi{r}_a\\
(1.4\pi)N_a&=2\pi{r}_a\\
N_a&=r_a\left(\frac{10}{7}\right)&&(r_a=7)\\
&=10;
\end{align*}
$$
- .$$N_b$$
$$
\begin{align*}
\omega_{0,b}N_b&=2\pi{r}_b\\
(1.2\pi)N_a&=2\pi{r}_b\\
N_a&=r_b\left(\frac{5}{3}\right)&&(r_b=3)\\
&=5;
\end{align*}
$$
- .$$N_c$$
$$
\begin{align*}
\omega_{0,c}N_c&=2\pi{r}_c\\
(0.5\pi)N_a&=2\pi{r}_c\\
N_a&=r_b\left(4\right)&&(r_b=1)\\
&=4;
\end{align*}
$$
Now find the fundamental period
$$
\begin{align*}
N^\prime&=\frac{\left(N_b,\:N_c\right)}{\gcd{\left(N_b,\:N_c\right)}}\\
&=\frac{(5)(4)}{(1)}\\
&=20;\\\\
\therefore\:N&=\frac{\left(N^\prime,\:N_a\right)}{\gcd{\left(N^\prime,\:N_a\right)}}\\
&=\frac{(10)(20)}{(10)}\\
&=20;
\end{align*}
$$


