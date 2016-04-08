# ex03e

## 5
__EXAMPLE: PERIODICITY__

Is $$x[n]=\sin{(\sqrt{3}n+\varphi)}$$ a periodic sequence?

__NO!__ $$x(t)=\sin{\left(\sqrt{3}t+\varphi\right)}$$ IS periodic! However, because $$\sqrt{3}$$ is not a multiple of $$\tfrac{2\pi}{\omega_0}$$, the sequence of samples in $$x[n]=\sin{\left(\sqrt{3}n+\varphi\right)}$$ will never repeat.
      
> __General rule__: 
- If $$\tfrac{2\pi}{\omega_0}$$ is a noninteger rational number, then the period will be a multiple of $\tfrac{2\pi}{\omega_0}$$
- otherwise, the sequence is __aperiodic__!