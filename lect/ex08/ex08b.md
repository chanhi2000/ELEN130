# ex08b

## 2.
__EXAMPLE: SOLVING A GENERIC DFT__

Let's assume $$N=4$$.
$$
\begin{align*}
X[k]&=\sum_{n=0}^{3}{x[n]e^{-j2\pi\tfrac{nk}{4}}}\\
&=\sum_{n=0}^{3}{W_4^{nk}}
\end{align*}
$$
What are $$X[0]$$ and $$X[1]$$ in terms of $$x[n]$$?
$$
\begin{align*}
X[0]&=x[0]+x[1]+x[2]+x[3]\\
X[1]&=x[0]+x[1]W_4+x[2]{W_4}^2+x[3]{W_4}^3\\
&=x[0]+x[1]\left(e^{-j2\pi\tfrac{(1)}{4}}\right)+x[2]\left(e^{-j2\pi\tfrac{(2)}{4}}\right)+x[3]\left(e^{-j2\pi\tfrac{(3)}{4}}\right)\\
&=x[0]+x[1]e^{-j\pi\left(\tfrac{1}{2}\right)}+x[2]e^{-j\pi}+x[3]e^{-j\pi\left(\tfrac{3}{2}\right)}\\
&=x[0]+x[1](-j))+x[2](-1))+x[3](j))\\
\end{align*}
$$
