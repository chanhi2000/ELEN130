# ex05e

## 5
__EXAMPLE: CTFT__
What is the FT of $$\cos{\left(2\pi{f}_0t\right)}$$?

$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}{\cos{(2\pi{f}_0t)}e^{-j2\pi{f}t}dt}\\
&=\int_{-\infty}^{\infty}{\frac{1}{2}\left(e^{j2\pi{f}_0t}+e^{-j2\pi{f}_0t}\right)e^{-j2\pi{f}t}dt}\\
&=\int_{-\infty}^{\infty}{\frac{1}{2}\left(e^{j2\pi\left({f}_0-f\right)t}+e^{-j2\pi\left({f}_0+f\right)t}\right)dt}\\
&\left\<\delta(f)=\int_{-\infty}^{\infty}{e^{j2\pi{f}t}dt}\right>\\
&=\frac{1}{2}\left(\delta\left(f_0-f\right)+\delta\left(f_0+f\right)\right)
\end{align*}
$$
- This is two deltas @ $$f=\pm{f}_0$$ with $$\tfrac{1}{2}$$ the original amplitude

