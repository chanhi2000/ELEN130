# ex09c

## 3.
__EXAMPLE: OVERLAP-ADD__
$$
\begin{align*}
x[n]&=[1,\:1,\:2,\:2]\\
h[n]&=[1,\:1,\:1]\\
y[n]&=x[n]\otimes{h}[n]
\end{align*}
$$

### 3.
- Let's break $$[1,\:1,\:2,\:2]$$ into two parts.
$$
\begin{align*}
x_1[n]&=[1,\:1]\\
x_2[n]&=[2,\:2]\\
\end{align*}
$$
- Then the results of two convolutions are:
$$
\begin{align*}
y_1[n]&=x_1[n]\otimes{h}[n]\\
&=[1,\:1]\otimes[1,\:1,\:1]\\
&=[1,\:2,\:2,\:1]\\\\
y_2[n]&=x_2[n]\otimes{h}[n]\\
&=[2,\:2]\otimes[1,\:1,\:1]\\
&=[2,\:4,\:4,\:2]\\\\
\end{align*}
$$
- Now, we must take these and overlap and add them
$$
\begin{matrix}
1&2&2&1&&\\
&&2&4&4&2\\\hline
1&2&4&5&4&2
\end{matrix}
$$
- The final answer is
$$
y[n]=[1,\:2,\:4,\:5,\:4,\:2]
$$
