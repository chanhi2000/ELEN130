# ex03c

## 3
__EXAMPLE: CIRCULAR TIME-REVERSAL__
$$
x[n]=\{1,\:2,\:3,\:4,\:5\}
$$
and we are interested in the __circular time__-__reversal__, $$\{y[n]\}=\{x[\left<-n\right>_{N}]\}$$ what does the new sequence look like?
- start one-by-one... 
$$
\begin{align*}
y[0]&=x\left[\left<0\right>_5\right]\\
&=x[0]=1
\end{align*}

$$
- next, 
$$
y[1]&=x\left[\left<-1\right>_5\right]\\
&=x[4]=5
$$

- Convince yourself the rest of the final sequence looks like this:
$$
\{1,\:5,\:4,\:3,\:2\}
$$

> __IMPORTANT NOTE__: MATLAB STARTS COUNTING AT $$1$$ – DO NOT LET THIS CONFUSE YOU!!! The modulo definition assumes a range $$0\leq{n}\leq{N}-1$$ and would be defined differently if the range was $$1\leq{n}\leq{N}$$.

