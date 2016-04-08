# ex04b

## 2
### (a)
- Start with a continuous-time signal $$x(t)=\cos{\left(2\pi(5)t\right)}$$ and sample it at $$f_T=20\:\text{Hz}$$.
- This has a continuous-time frequency of $$5\:\text{Hz}$$ and is being sampled at a sampling rate of $$20\:\text{Hz}$$.

### (b)
- Start with a continuous-time signal $$x(t)=\cos{\left(2\pi(10)t\right)}$$ and sample it at $$f_T=40\:\text{Hz}$$.
- This has a continuous-time frequency of $$10\:\text{Hz}$$ and is being sampled at a sampling ate of $$40\:\text{Hz}$$.

### (c)
- Start with a continuous-time signal $$x(t)=\cos{\left(2\pi(25)t\right)}$$ and sample it at $$f_T=20\:\text{Hz}$$.
- This has a continuous-time frequency of $$25\:\text{Hz}$$ and is being sampled at a sampling ate of $$20\:\text{Hz}$$.


> - ALL THREE of these examples result in the EXACT SAME SAMPLES!!!
> - Without context (the reconstructor knowing what the sampling rate was) it is impossible to recreate the intended continuous time signal
> - How can a reconstructor get back the original continuous-time signal (the goal of any good reconstructor)?
> - It needs context – it needs to know the sampling rate
> - And, it needs to assume that the sampling rate was chosen intelligently (TRUST!!!)
> - __What does intelligently mean?__: If the sampling rate was chosen to exceed twice the highest frequency of our signal, then, on the other side, the reconstructor can assume that all frequencies will be less than half of the sampling rate.

