# lab03sub

## OBJECTIVES:
In this lab, learning about the way old push-button telephones worked is used as a tool to gain familiarity with generating useful sequences, filtering for sequences and learn more about the importance of sampling frequencies.


## PREALB:

### 1.
![fig01](lab03sub/lab03sub-fig01.png)
For each of the seven frequencies used for DTMF dialing signals, compute the following
- The number of samples per cycle if the sampling rate is $$8000\:\text{Hz}$$
- The number of cycles in the minimum $$50\:\text{ms}$$ interval

#### 1(a) ANSWER
The number of samples per cycle if the sampling rate is $$8000\:\text{Hz}$$
$$
\begin{align*}
f_0&\:\left[\tfrac{\text{cycle}}{\text{sec}}\right]\\
f_T&=8000\:\left[\tfrac{\text{sample}}{\text{sec}}\right]\\
\nu_0&=\frac{f_0}{f_T}\:\left[\tfrac{\text{cycle}}{\text{sample}}\right]
\end{align*}
$$

| $$f_0$$ | $$\nu_0$$ | $$\frac{1}{\nu_0}$$ |
| :------ | :-------- | :------------------ |
| 697 Hz | 0.087125 | 11.477761836 |
| 770 Hz | 0.09625 | 10.38961039 |
| 852 Hz | 0.1065 | 9.3896713615 |
| 941 Hz | 0.117625 | 8.5015940489 |
| 1209 Hz | 0.151125 | 6.6170388751 |
| 1336 Hz | 0.167 | 5.9880239521 |
| 1477 Hz | 0.184625 | 5.4163845633 |


#### 1(b) ANSWER
The number of cycles in the minimum $$50\:\text{ms}$$ interval
$$
\require{cancel}
\begin{align*}
t_\text{min}&=0.05\:\text{sec}\\
\#\text{CYCLE}&=(f_0)(t_\text{min})(\nu_0)\:\left[\tfrac{\cancel{\text{sample}}}{\cancel{\text{sec}}}\right]\left[\cancel{\text{sec}}\right]\left[\tfrac{\text{cycle}}{\cancel{\text{sample}}}\right]
\end{align*}
$$

| $$f_0$$ | $$\#\text{CYCLE}$$ |
| :------ | :-------- |
| 697 Hz | 3.03630625 |
| 770 Hz | 3.705625 |
| 852 Hz | 4.5369 |
| 941 Hz | 5.53425625 |
| 1209 Hz | 9.13550625 |
| 1336 Hz | 11.1556 |
| 1477 Hz | 13.63455625 |


### 2.
Review the signal generating `m`-files you used in previous laboratories. Draw a flow chart for a function that will create a signal vector for a telephone dialing signal. The function will have four inputs and one output as defined below. (You will write and test your function in the lab.)
```matlab
function dial_sig = my_dtmf(tone_time, quiet_time, fs, dial_vals)
% INPUTS:
% tone_time is the tone duration in seconds
% quiet_time is quiet time duration between tones in seconds fs is the sampling frequency in Hz
% dial_vals is a vector of integers from 1 to 12 representing the
% button numbers of the sequence of numbers to be dialed Note that the dialed "0" is button number 11!!!!!!
%
% OUTPUT:
% dial_sig is the vector of sampled values of the DTMF output signal for the number sequence
```

#### 2. ANSWER
![fig02](lab03sub/lab03sub-fig02.png)


### 3.
Determine the output vector created by the following MATLAB instructions where concatenation is used to build up a vector by computing components and appending them.
```matlab
out_vec = [ ];
for k = 1:4
	out_vec = [out_vec k*ones(1,5) ];
end
out_vec
```

__output__:
```
out_vec =

	Columns 1 through 6

	1	1	1	1	1	2

	Columns 7 through 12

	2	2	2	2	3	3

	Columns 13 through 18

	3	3	3	4	4	4

	Columns 19 through 20

	4	4
```

#### 3. ANSWER
It will create the vector of $$1\times20$$ matrix that has five sets of 1, 2, 3, 4, and 5 concatenated respectively.


### 4.
Determine the length in samples of the vector created by the following MATLAB instruction using your function. Determine the time duration of the sound created. (__NOTE__: You will create the function in the lab period, but you can answer this question based on the function description.)
```matlab
sample_freq = 8000;
dt = 0.2;
qt = 0.1;
dial_scu = [ 1 4 11 8 5 5 4 4 11 11 11 ] ; % note that dialed 0 is button 11
sig_1 = my_dtmf( dt, qt, sample_freq, dial_scu);
sound( sig_1, sample_freq);
```

#### 4. ANSWER
__length in samples of the vector__ and __time duration of the sound__
$$
\require{cancel}
\begin{align*}
\#\text{DIALS}&=11;\\
f_T&=8000\:\left[\tfrac{\text{sample}}{\text{sec}}\right];\\
t_\text{tone}&=0.2\:\text{sec}\\
t_\text{quiet}&=0.1\:\text{sec}\\
L_\text{sample}&=\#\text{DIALS}\cdot{f}_T\cdot\left(t_\text{tone}+t_\text{quiet}\right)\\
&=(11)(8000)(0.2+0.1)\:\left[\tfrac{\text{sample}}{\cancel{\text{sec}}}\right]\left[\cancel{\text{sec}}\right]\\
&=26,400;\\
t_\text{duration}&=\frac{L_\text{sample}}{f_T}\\
&=\frac{(26,400)}{(8000)}\:\frac{\left[\cancel{\text{sample}}\right]}{\left[\tfrac{\cancel{\text{sample}}}{\text{sec}}\right]}\\
&=3.3\:\text{sec};
\end{align*}
$$






