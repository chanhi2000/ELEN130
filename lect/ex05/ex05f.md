# ex05f

## 6
__EXAMPLE: INVERSE CTFT__

What is the FT of $$X(f)=\tfrac{1}{2}\left(\delta\left(f_0-f\right)+\delta\left(f_0+f\right)\right)$$?

$$
\begin{align*}
X(f)&=\tfrac{1}{2}\left(\delta\left(f_0-f\right)+\delta\left(f_0+f\right)\right)\\
&\left<x_a(t)=\frac{1}{2\pi}\int_{-\infty}^{\infty}{X_a(j\Omega)e^{j\Omega{t}}d\Omega}\right>\\
&=\int_{-\infty}^{\infty}{\frac{1}{2\pi}\int_{-\infty}^{\infty}{X_a(j\Omega)e^{j\Omega{t}}d\Omega}e^{j2\pi{f}t}df}\\
&=\frac{1}{2}\left(e^{j2\pi{f}_0t}+e^{-j2\pi{f}_0t}\right)\\
&=\cos{\left(2\pi{f}_0t\right)}
\end{align*}
$$