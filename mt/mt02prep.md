# mt02prep

## MIDTERM #2 REVIEW: MATERIAL

### COVERS HW3-HW5

### SAMPLING A BANDPASS SIGNAL

### FREQUENCY RESPONSE
- __Plotting Magnitude and Phase__
- __Simplifying Frequency responses (pulling out trig. Functions)__
- Steady State Response (to sinusoidal input)
- Response to a causal input
- Delay
	- Group Delay
	- Phase Delay
- Filter Design
	- LPF with coefficients of $$\operatorname{sinc}()$$
	- HPF by shifting LPF
	- Better LPF through convolution
- DFT
	- $$W_N$$and hand calculations
	- unit circle represenation
	- Matrix Representation of DFT
	- MATLAB
		- `freqz`
		- `fft`
		- `fftshift`
	- Associating samples to real-world frequency
	- Periodicity of DFT in discrete-time domain and frequency domain
	- Circular convolution
	- Using DFT to calculate linear and circular convolution
	- Cyclic Prefix
	- Overlap-Add and Overlap-Save methodology
	- DFT Symmetries
	- DFT Properties
- Z-transform
	- Solving Z-transforms and determining region of convergences (ROC)
	- How Z-transform relates to the DTFT
	- Finding poles and zeroes, pole/zero plots
	- Using Z-transform table
	- Applying the Z-transform shift property
	- Applying Z-transform to an LCCDE
	- Computing inverse Z-transform
	- Sketching the magnitude of the DTFT from the Z-transform
	- Design example — filtering out a specific frequency
	- Stability
	- Other ways to compute inverse Z-transform
		- Partial Fraction Expansion
		- Long Division of Polynomials
- Basic Filter Structure
	- Determining magnitude and phase
	- Phase importance
	- Linear phase — what it means
	- Impact of symmetry and zero placement on filter design
	- FIR filter types 1, 2, 3, 4
	- Introducing zeros to pre-existing filters
	- Proving even or odd symmetry
	- Practical filters: finite duration AND causal
		- Did this in HW and in Discussion questions
	- Converting an LPF to an HPF
	- Convolving LPFs and HPFs to make better versions of each
	- Comb filters
- More filter design
	- __All-Pass filters__: design, magnitude, phase
	- Impact of filter on a signal (lower frequencies vs. higher frequencies)
- ~~Filter Design Structures and Realization Factors~~
	- ~~Tapped Delay Line (Direct Form)~~
	- ~~Transpose of structures~~
