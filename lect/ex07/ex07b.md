# ex07b

## 2.
__EXAMPLE: FREQUENCY RESPONSE__
- Consider an altered moving average filter:
$$
y[n]=\frac{1}{M}\sum_{k=0}{M-1}{x[n-k]}
$$
Find the frequency response.
- For simpicity, we will lose $$\tfrac{1}{M}$$ factor.
- Let's say $$M=3$$, so
$$
y[n]=x[n]+x[n-1]+x[n-2]
$$
- Now, let's assume the input is a generic cosine:
$$
x[n]=\cos{\left(\omega_0n\right)}
$$
- So,
$$
y[n]=\cos{\left(\omega_0n\right)}+\cos{\left(\omega_0(n-1)\right)}+\cos{\left(\omega_0(n-2)\right)}
$$
- Without a loss of generality, we can do a variable substitution: $$m=n-1$$
$$
y[m+1]=\cos{\left(\omega_0(m+1)\right)}+\cos{\left(\omega_0m\right)}+\cos{\left(\omega_0(m-1)\right)}
$$
- Using trig identitiess:
$$
\begin{align*}
y[m+1]&=\left(\cos{\left(\omega_0m\right)}\cos{\left(\omega_0)\right)}\right)-\left(\sin{\left(\omega_0m\right)}\sin{\left(\omega_0\right)}\right)\\
&+\cos{\left(\omega_0m\right)}\\
&+\left(\cos{\left(\omega_0m)\right)}\cos{\left(\omega_0)\right)}\right)+\left(\sin{\left(\omega_0m)\right)}\sin{\left(\omega_0)\right)}\right)\\
&=\cos{\left(\omega_0m)\right)}\left(1+2\cos{\left(\omega_0\right)}\right)\\
y[n]&=\cos{\left(\omega_0(n-1))\right)}\left(1+2\cos{\left(\omega_0\right)}\right)\\
\end{align*}
$$
- So, what we have is an input that is delayed by one sample
- And, more importantly, a scale factor that depends on the frequency and NOT $$n$$!!!!!
- Plot of the gain of the filter is below
![fig02](ex07/ex07-fig02.jpg)
- How does this behave for different values of $$\omega_0$$?
$$
\begin{align*}
\omega_0&=0?,\\
\omega_0&=\frac{\pi}{2}?\\
\omega_0&=\frac{2\pi}{3}?\\
\omega_0&=\pi?\\
\end{align*}
$$
- So, is there an easier way to find frequency selective behavior?
- Let's use complex exponentials to avoid trig
$$
\begin{align*}
e^{j\theta}&=\cos{\left(\theta\right)}+j\sin{\left(\theta\right)}\\
e^{-j\theta}&=\cos{\left(\theta\right)}-j\sin{\left(\theta\right)}\\
e^{j2\theta}&=\cos{\left(2\theta\right)}+j\sin{\left(2\theta\right)}\\
&=\left(e^{j\theta}\right)^2=\left(\cos{\left(\theta\right)}+j\sin{\left(\theta\right)}\right)^2\\
&=\cos^2{\left(\theta\right)}+2j\cos{\left(\theta\right)}\sin{\left(\theta\right)}+j^2\sin^2{\left(\theta\right)}\\
&=\cos^2{\left(\theta\right)}-\sin^2{\left(\theta\right)}+j\left(2\cos{\left(\theta\right)}\sin{\left(\theta\right)}\right)\\
&=\cos{\left(2\theta\right)}+j\sin{\left(2\theta\right)}
\end{align*}
$$