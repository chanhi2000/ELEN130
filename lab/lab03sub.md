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


## LAB

### STEP 1: 
Create and debug a function to generate a DTMF signal as described in the preLab

#### 1(a) 
Write your m-file to implement the function `my_dtmf`. Save it in a file called `my_dtmf.m` in your MATLAB working directory. Save it in your MATLAB current folder.

#### `my_dtmf.m`
```matlab
function dial_sig = my_dtmf(tone_time, quiet_time, fs, dial_vals)
% INPUTS:
% - tone_time is the tone duration in seconds
% - quiet_time is quiet time duration between tones in seconds
% - fs is the sampling frequency in Hz
% - dial_vals is a vector of integers from 1 to 12 representing the
% - button numbers of the sequence of numbers to be dialed
% Note that the dialed "0" is button number 11!!!!!!
%
% OUTPUT:
% - dial_sig is the vector of sampled values of the DTMF output signal 
% for the number sequence

t_tone_new = 0:tone_time*fs-1;
num = length(dial_vals);
quiet_sig = zeros(1, fs*quiet_time);
f_tone = [
    697, 1209; 697, 1336; 697, 1477;... 
    770, 1209; 770, 1336; 770, 1477;...
    852, 1209; 852, 1336; 852, 1477;...
    941, 1209; 941, 1336; 941, 1477;
    ];

dial_sig = []; %initializes the output to an empty vector

for ii=1:num
    lo = f_tone(dial_vals(ii),1);
    hi = f_tone(dial_vals(ii),2);
    new_sig = cos( 2 * pi * lo / fs * t_tone_new)...
        + cos( 2 * pi * hi / fs * t_tone_new );
    
    % normalize the output around 1
    new_sig = new_sig./abs(max(new_sig(:)));

    dial_sig = [dial_sig, new_sig, quiet_sig];
end
```

#### 1(b)
Call your function with a sampling frequency of $$8000$$, a tone time of $$0.5\:\text{sec}$$, a quiet time of $$0.1\:\text{sec}$$, and a dial value vector with a single number. Debug the `m`-file until there are no errors.

When the function runs without detected errors:
- Use sound to listen to your signal. Does it sound right? Compare it to a telephone when the same button is pressed. Be sure that you divided the output signal by the maximum value so that the output values will be between -1 and +1. Otherwise the nonlinear clipping will create additional frequencies in the sound.
- Use the plot instruction to plot your signal. Describe the display of the complete signal.

Repeat your test with a dial value vector containing two different numbers.

Demonstrate this to your laboratory assistant.

```matlab
fs = 8000;
t_tone = 0.5;
t_quiet = 0.1;

% ----- create signals -----
dial_val1 = [1 4 11 8 5 5 4 4 11 11 11];    % SCU: 1-408-554-4000
test_sig1 = my_dtmf(t_tone, t_quiet, fs, dial_val1);

dial_val2 = [1 4 11 8 6 9 1 8 7 6 1]; % my number 1-408-691-8761
test_sig2 = my_dtmf(t_tone, t_quiet, fs, dial_val2);

% ----- play the sound -----
sound(test_sig1, fs)
pause(6.5);
 
sound(test_sig2, fs);


% ----- plot the signals -----
figure();
plot(test_sig1);

figure();
plot(test_sig2);
```

#### 1(c)
Use the zoom feature of the figure display to show regions that are about $$200\:\text{samples}$$ wide. (Or use `axis` to do this.) Describe the zoomed-in display. (An example for comparison is shown below.)

```matlab
% fs = 8000;
% t_tone = 0.5;
% t_quiet = 0.1;

% ----- test 3 -----
dial_val3 = [3 5 7];
test_sig3 = my_dtmf(t_tone, t_quiet, fs, dial_val3);

% zoom in
figure();

subplot(3,1,1);
plot(test_sig3);
axis([0 14400 -1 1]);
title('first 3 number dials: 3, 5, 7');

subplot(3,1,2);
plot(test_sig3);
axis([0 500 -1 1]);
title('dials: 3');

subplot(3,1,3);
plot(test_sig3);
axis([5000 5500 -1 1]);
title('dials: 5');
```

##### Q1(c)
Does it look like the sum of two sinusoids that have no harmonic relationship? If one were a harmonic of the other, how would the display be different?

##### A1(c)

-----

### STEP 2:
Create an `m`-file to test and explore the DTMF signal generated by your function.

#### 2(a)
Create a script `m`-file named `lab03test01` to make some test signals with your function. For all signals, use a sampling frequency of `$$8000\:\text{Hz}$$. These signals will be used in Laboratory 4 with filters that will detect the dialed sequence.
- Make signal my_phone with a tone time of 0.5 seconds, a quiet time of 0.1 seconds and a dial value vector set to a phone number you use.
- Make sig5, for the '5' button using the same parameters
- Make the following four additional test signals:

| Signal name | `dial_vals` | Tone time (sec.) | Quiet time (sec.) |
| :---------- | :---------- | :--------------: | :---------------: |
| `testSig1` | `dial_vals = [3 5 7 11]` | 0.25 | 0.05 |
| `testSig2` | `dial_vals = [3 5 7 11]` | 0.10 | 0.02 |
| `testSig3` | `dial_vals = [3 5 7 11]` | 0.50 | 0.10 |
| `testSig4` | `dial_vals = 1:12` | 0.25 | 0.05 |

- Use sound to play each of these signals and compare the first three. Use the sound instruction in the command window and wait until the sound is finished before typing the next sound instruction. __Do not put a sequence of sound instructions in an `m`-file script__.


#### `lab03test01.m`
```matlab
%% intialize
clear, clc, clf, cla, close all;

%% PART 2
% This is the matlab file to test signals with our function.

fs = 8000;

% ----- create signals -----
dial_val1 = [1 4 11 8 6 9 1 8 7 6 1]; % my number 1-408-691-8761
t_tone = 0.5;
t_quiet = 0.1;
my_phone = my_dtmf(t_tone, t_quiet, fs, dial_val1);

dial_val2 = [5];
sig5 = my_dtmf(t_tone, t_quiet, fs, dial_val2);

dial_val3 = [3 5 7 11];
t_tone1 = 0.25;
t_quiet1 = 0.05;
testSig1 = my_dtmf(t_tone1, t_quiet1, fs, dial_val3);

t_tone2 = 0.10;
t_quiet2 = 0.02;
testSig2 = my_dtmf(t_tone2, t_quiet2, fs, dial_val3);

t_tone3 = 0.50;
t_quiet3 = 0.10;
testSig3 = my_dtmf(t_tone3, t_quiet3, fs, dial_val3);

dial_val4 = 1:12;
t_tone4 = 0.25;
t_quiet4 = 0.05;
testSig4 = my_dtmf(t_tone4, t_quiet4, fs, dial_val4);


% ---- play the sound -----
sound(my_phone, fs);
pause(7);

sound(sig5);
pause(1);

sound(testSig1);
pause(3);

sound(testSig2);
pause(3);

sound(testSig3);
pause(4);

sound(testSig4);
pause(4);
```

#### 2(b) 
What is the total duration of each of the six signals in samples? in time?
Demonstrate the sounds to your laboratory assistant.

-----

### STEP 3:
Explore the effect of the sampling frequency.

#### 3(a)
Create a script `m`-file named `lab03test02` to make some additional test signals with your function. Except for the sampling frequency, use the same parameters used for `testSig1` above.
- Make `testSig1_16000` using a sampling frequency of 16000.
- Make `testSig1_4000` using a sampling frequency of 4000.
- Make `testSig1_2000` using a sampling frequency of 2000.

#### `lab03test02.m`
```matlab
%% intialize
clear, clc, clf, cla, close all;

%% PART 3
% This is the matlab file to test signals with our function.

fs1 = 16000;
fs2 = 4000;
fs3 = 2000;

% ---- create signals ----- 
dial_val = [3 5 7 11];
t_tone = 0.25;
t_quiet = 0.05;
testSig1_16000 = my_dtmf(t_tone, t_quiet, fs1, dial_val);
testSig1_4000 = my_dtmf(t_tone, t_quiet, fs2, dial_val);
testSig1_2000 = my_dtmf(t_tone, t_quiet, fs3, dial_val);


% ---- play the sound -----

% sound(testSig1_16000);
% pause(3);

% sound(testSig1_4000);
% pause(3);

sound(testSig1_2000);
pause(3);


% ----- see the plot -----
figure();
subplot(3,1,1)
plot(testSig1_16000);
subplot(3,1,2)
plot(testSig1_4000);
subplot(3,1,3)
plot(testSig1_2000);


% ----- length of the plot -----
len1 = length(testSig1_16000)
len2 = length(testSig1_4000)
len3 = length(testSig1_2000)
```


#### 3(b)
Use sound to listen to these three new signals and compare each to `testSig1`. In each case for the sound instruction use the sampling frequency that was used to create the signal.
- Is the time duration of each signal the same?
- Do all the signals sound the same? Explain any differences you hear.
- What is the length of each signal in samples?

Demonstrate the sounds to your laboratory assistant.

