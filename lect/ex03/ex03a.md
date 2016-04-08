# ex03a

## 1
__EXAMPLE: OPERATIONS ON FINITE-LENGTH SEQUENCES__

Consider a length-$$N$$ sequence $$x[n]$$ defined for $$0\leq{n}\leq{N}-1$$ and otherwise undefined

### (a)
What is the length of $$x[-n]$$ and over what range is it defined?
$$
\begin{align*}
\text{Length}:&N\\
\text{Range}:&-(N-1)\leq{n}\leq0
\end{align*}
$$

### (b)
What about a linear time-shift of $$x[n]$$ by $$M$$ (integer)?
$$
\begin{align*}
\text{Length}:&N\\
\text{Range}:&-M\leq{n}\leq{N}-M-1
\end{align*}
$$

###(c)
What about convolution of two length-$$N$$ sequences, each defined from $$0\leq{n}\leq{N}-1$$?
$$
\begin{align*}
\text{Length}:&2N-1\\
\text{Range}:&0\leq{n}\leq2N-2
\end{align*}
$$

> __NOTE__: Manipulations on sequences that change the range and/or length are not ideal for most implementations. Thus, these operations are tweaked as follows.
