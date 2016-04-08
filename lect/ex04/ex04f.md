# ex04f

## 6
__EXAMPLE: LINEARITY TEST__

Is the accumulator linear?
$$
\begin{align*}
y_1[n]&=\sum_{l=-\infty}^{n}{x_1[l]}\\
y_2[n]&=\sum_{l=-\infty}^{n}{x_2[l]}
\end{align*}
$$
For $$x[n]=\alpha{x}_1[n]+\beta{x}_2[n]$$, the output is:
$$
\begin{align*}
y[n]&=\sum_{l=-\infty}^{n}{\left(\alpha{x}_1[l]+\beta{x}_2[l]\right)}\\
&=\alpha\left(\sum_{l=-\infty}^{n}{x_1[l]}\right)+\beta\left(\sum_{l=-\infty}^{n}{x_2[l]}\right)\\
&=\alpha{y}_1[n]+\alpha{y}_2[n]
\end{align*}
$$

$$
\therefore\:\text{YES, it is linear.}
$$
