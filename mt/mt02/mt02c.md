# mt02c

## 3.
__DFT and DFT Properties__

### 3(a)
For the point, $$x$$, shown below, calculate the approximate location for $$x^4$$. This will go faster if you use what you know about manipulating points on the unit circle.
![fig01a](mt02/mt02c-fig01a.png)

If we say $$x=e^{-j2\pi\tfrac{23}{32}}$$ (just before 24th point),
$$
x^4&=\left(W_{32}^{23}\right)^4\\
&=W_{32}^{92}\\
&=W_{32}^{92}-\left(W_{32}^{32}\right)^2\\
&=W_{32}^{92-2(32)}\\
&=W_{32}^{28}\\
$$

See the MATLAB result to convince yourself
![fig01b](mt02/mt02c-fig01b.png)

### 3(b)
.$${W_{32}}^p$$ represents the point closest to the product that you determined abvoe in Problem 3(a). What is $$p$$?
$$
p=28;
$$


### 3(c)
Reduce $${W_{16}}^{54}$$ to a complex number.
$$
\begin{align*}
{W_{16}}^{54}
\end{align*}
$$


------
For the problem 3(d)-(e), you are given
$$
x[n]=[1,\:2,\:3,\:4]
$$
and are told that:
$$
\operatorname{DFT}\left(x[n]\right)=\{10,\:-2+2j,\:-2,\:-2-2j\}
$$

### 3(d)
Find
$$
\operatorname{DFT}\left(\operatorname{DFT}\left(\operatorname{DFT}\left(\operatorname{DFT}\left(x[n]\right)\right)\right)\right)
$$


### 3(e)
What is the $$\operatorname{DFT}\left(\{3,\:4,\:1,\:2\right)$$? While you will be given credit for brute forcing this, it is not the point of the problem and you will spend less time on this by applying  the appropriate property.
