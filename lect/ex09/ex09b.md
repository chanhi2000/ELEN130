# ex09b

## 2.
Given
$$
\begin{align*}
h[n]&=[1,\:2,\:1]\\
x[n]&=[-1,\:5,\:6,\:2,\:7]
\end{align*}
$$
Find the convlution (linear/circular) using DFT.

### 2(a)
__CIRCULAR__

- Length of $$h[n]$$ is $$N=3$$, length of $$x[n]$$ is $$M=5$$.
- Thus length of linear convolution is $$3+5-1=7$$.
- First we have to have the same length for $$H[k]$$ and $$X[k]$$ in order to multiply them in the frequency domain. To get a $$5$$-point DFT of $$h[n]$$, we __zero pad__. So, the new $$h[n]$$ becomes:
$$
h_2[n]=[1,\:2,\:1,\:0,\:0]
$$
- Zero padding computes more DFT values from DTFT. It does NOT change anything about the frequency resolution.
$$
\begin{align*}
X[k]:&\text{DFT of }x[n]\\
H_2[k]:&\text{DFT of }h_2[n]\\
Y[k]&=X[k]H_2[k]\\
y_c[k]:&\text{IDFT of }Y[k] 
\end{align*}
$$
- How does $$y_L[n]$$ relate to $$y_c[n]$$?
$$
\begin{align*}
y_c[0]&=y_L[0]+y_L[5];\\
y_c[1]&=y_L[1]+y_L[6];\\
y_c[2]&=y_L[2];\\
y_c[3]&=y_L[3];\\
y_c[4]&=y_L[4];\\
\end{align*}
$$
- You can take the last $$M-1$$ terms of a linear convolution and add them to the first $$M-1$$ terms of the lienar convolution and get the circular convolution!

### 2(b)
__LINEAR__
- Zero pad BOTH to the length of the linear convolution. $$M+N-1=(3)+(4)-1=7$$.
$$
\begin{align*}
h_3[n]&=[1,\:2,\:1,\:0,\:0,\:0,\:0]\\
x_3[n]&=[-1,\:5,\:6,\:2,\:7,\:0,\:0]\\
\end{align*}
$$
- Then, IDFT of the product of $$X_3[k]$$ and $$H_3[k]$$ (where these are the respective DFTs)
- Verify with MATLAB
