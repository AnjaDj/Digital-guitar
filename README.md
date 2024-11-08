# Karplus-Strong Algorithm

The **Karplus-Strong** algorithm, also known as the "digital guitar" algorithm, enables the simple synthesis of fairly realistic plucked string instrument tones. The basic block diagram of the Karplus-Strong synthesizer is shown in Figure 1. The system consists of a delay line of length `N` and feedback containing a filter with the transfer function `Hd(z)`. In the basic algorithm, the feedback filter is a simple first-order filter:

$$
H_d(z) = 1 + z^{-1}
$$

![Figure 1. Karplus-Strong block diagram](https://github.com/user-attachments/assets/bc14831e-fb40-4095-881e-a2451da99bad)<br>
**Figure 1:** Karplus-Strong block diagram <br> <br>
The system is excited by a white noise sample of length `N`. It can be noted that this is a modification of the wavetable synthesis. Wavetable synthesis in the feedback path does not use a filter, causing the same signal sample to repeat periodically at the output. On the other hand, in the case of Karplus-Strong synthesis, the signal sample is modified at each "period" using the filter in the feedback path, producing a quasi-periodic signal that sounds more realistic.

The total delay introduced by the system is `N + 1/2` samples, and the fundamental frequency of the resulting tone is:

$$
F_0 = \frac{F_s}{N + \frac{1}{2}}
$$

Using equation (2), it is possible to calculate the required length of the delay line for the desired fundamental frequency of the tone and the sampling frequency. However, due to the constraint that the delay line length must be an integer, the frequencies of the generated tones may deviate from the desired values.

The lack of averaging in the feedback path results in no precise control over the tone duration. The original algorithm generates higher frequencies that last shorter than lower frequencies. While this behavior is expected for plucked string instruments, the range between the durations of deep and high tones is too large, causing high tones to last very briefly and low tones to last unnaturally long.

To address this, the averaging in the feedback path can be replaced with a filter whose transfer function is:

$$
H_d(z) = \rho \left( (1 - S) + S z^{-1} \right)
$$

The tone duration can now be controlled using the parameters `0 < ρ ≤ 1` and `0 < S < 1`. The value of `ρ` can be calculated using the formula:

$$
\rho = \frac{0.001}{F_0 t_{60}}
$$

where `F_0` is the fundamental frequency of the tone, and `t_{60}` is the time for the tone's intensity to decay by 60 dB. When `S = 1/2`, the generated tone will have the shortest duration, and as the value of `S` approaches 0 or 1, the tone duration increases.

The effect of the plucking position on the string can be modeled by filtering the input white noise with a comb filter whose transfer function is:

$$
H_b(z) = 1 - z^{-\lfloor \beta N \rfloor}
$$

where the position on the string is determined by the value of `β ∈ ⟨0, 1⟩`. <br>

![image](https://github.com/user-attachments/assets/2281de03-378e-4ca7-b689-329d6fb3f099)<br>
**Figure 2:** Karplus-Strong synthesizer with a comb filter at the input. <br> <br>

### Nonlinear Effects and Overdrive

A common nonlinear guitar effect in rock music is overdrive, achieved using a symmetric saturation amplifier. The static characteristic of this nonlinear effect is given by the equation:

$$
f(x) = 
\begin{cases}
-\frac{2}{3}, & x \leq -1 \\
x - \frac{x^3}{3}, & -1 < x < 1 \\
\frac{2}{3}, & x \geq 1
\end{cases}
$$

The final block diagram of the synthesizer is shown in Figure 3. <br>

![image](https://github.com/user-attachments/assets/0b239a12-bae5-44fa-98c6-fbb279d70b80)<br>
**Figure 3:** Karplus-Strong synthesizer with tone duration control, string plucking position, and an overdrive nonlinear effect at the output. <br> <br>

### Tasks

1. Determine the transfer function for the system shown in Figure 1.
2. Plot the amplitude and phase response of the system for synthesizing the tone A in the fourth octave, with a frequency of 440 Hz and a sampling frequency of 48 kHz. Label the frequency axis in Hertz. Based on the spectral shape of the excitation signal (white noise), comment on why this system is suitable for tone synthesis.
3. Write a Python function that, using the Karplus-Strong algorithm, generates a tone of a given frequency (in Hz) and duration (in seconds) for a given sampling frequency (in Hz).
4. Test the function from the previous task by generating the tone A in the fourth octave. Use a sampling frequency of 48 kHz. Reproduce the tone. The duration of the tone should be 1 second. Plot the waveform of the generated signal (as a continuous signal) with the time axis labeled in seconds. Plot the amplitude spectrum of the generated signal with the frequency axis labeled in Hertz.
5. Write Python code that, using the function from Task 3, generates the C-major scale. The frequencies of the notes and their formula can be found at [Wikipedia](https://en.wikipedia.org/wiki/Piano_key_frequencies). Reproduce the generated signal and save it as a WAV file. What are the actual frequencies of the notes generated by the implemented synthesizer?
6. Write a Python function that implements the Karplus-Strong synthesizer with a filter in the feedback branch, allowing control over the tone duration. In addition to the desired tone frequency, its duration, and the sampling frequency, the input arguments of this function should also include parameters `t60` and `S` from (3) and (4).
7. Demonstrate the effect of the parameters `t60` and `S` on the duration of the tones in the C-major scale.
8. The plucking position on the strings can be controlled by using a signal that is not white noise but one obtained by filtering the white noise with a comb FIR filter with the transfer function (5). Modify the function from Task 6 so that the input signal is first filtered with the comb FIR filter. The parameter `β` should be an additional input argument of this function, along with the existing ones.
9. Demonstrate the effect of the parameter `β ∈ {0.01, 0.1, 0.5, 0.9}` on the tones of the C-major scale.
10. Modify the function from Task 8 to include the implementation of the overdrive effect with gain `g`, which can be specified as an input argument.
11. Demonstrate the overdrive effect on the tones of the C-major scale with a gain of `g = 1000` before the nonlinearity (6).
12. Write a function that, using some of the functions from Tasks 6, 8, or 10, generates a chord consisting of notes with given frequencies. A chord on the guitar can be seen as a series of notes played simultaneously, with a slight delay between the start of each note.
13. Using the synthesizer implemented in one of Tasks 6, 8, or 10, play a complete composition. Possible examples include compositions from [Arcade Tones](http://arcadetones.emuunlim.com/), but you can also choose your own.

<br> <br>

## First steps
[1. Installing conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html) <br>
[2. Creating environments](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html) <br>
[3.Install jupyter notebook](https://jupyter.org/install) <br>
[4. Install numpy](https://numpy.org/install/) <br>


## Start the system
1. conda activate "enviroment_name"
2. jupyter notebook


## Note
Zadaca1.ipynb is the main file and all the other files are just resources for Zadaca1.ipynb <br>
The easiest way to check this implementation is to simple open the Zadaca1.ipynb file from GitHub page as double click.
