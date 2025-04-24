## System Stability Analysis

### Objective
The goal of this project is to develop a MATLAB-based graphical user interface (GUI) tool that allows users to input the numerator and denominator coefficients of a system's transfer function and analyze its stability. The tool displays the Bode Plot, Gain and Phase Margins, Root Locus, System Information, and Step Response for the given system.

### System Stability
A linear time-invariant (LTI) system is stable if its output remains bounded for any bounded input. This is known as Bounded Input, Bounded Output (BIBO) stability.

A transfer function \( H(s) = \frac{Y(s)}{X(s)} = \frac{b_0 s^m + \dots + b_m}{a_0 s^n + \dots + a_n} \) represents the relationship between input and output in the Laplace domain. The **poles** of this function (values of \( s \) that make the denominator zero) determine system stability:
- For **continuous-time systems**, stability requires all poles to lie in the **left half of the complex plane** (i.e., real parts < 0).
- For **discrete-time systems**, stability requires all poles to lie **inside the unit circle** in the complex plane.

A **Bode plot** shows the frequency response of a system (magnitude and phase vs. frequency). While it doesn’t directly show instability, it suggests stability when there are no sharp gain spikes and both the gain and phase margins are positive.

### Mathematical Functions

#### Bode Plot
The Bode plot is a key tool in control systems engineering used to analyze a system's frequency response. It consists of two plots:
- **Magnitude plot**: Gain (in dB) vs. frequency.
- **Phase plot**: Phase shift (in degrees) vs. frequency.

**Input Collection**:
Users enter the numerator and denominator of the transfer function, such as `numerator = [1 1]`, `denominator = [1 2 1]`, representing \( H(s) = \frac{s + 1}{s^2 + 2s + 1} \), and specify a frequency range.

**Frequency Sampling**:
The app uses `logspace` to generate 100 logarithmically spaced frequency points between the user-defined bounds (e.g., `logspace(log10(0.1), log10(1000), 100)`), ensuring smooth coverage across the frequency spectrum.

**Transfer Function Evaluation**:
The transfer function is created using MATLAB’s `tf()` function. At each frequency \( \omega_i = 2\pi f_i \), it evaluates \( H(j\omega_i) \) using `evalfr(H, j*omega_i)`.

**Magnitude and Phase Calculation**:
- Magnitude is calculated as \( |H(j\omega)| = \sqrt{\text{Re}(H)^2 + \text{Im}(H)^2} \) and converted to dB.
- Phase is calculated as \( \angle H(j\omega) = \tan^{-1}(\text{Im}(H)/\text{Re}(H)) \), then converted to degrees.

**Plotting**:
The Bode plot displays magnitude and phase vs. frequency on a dual y-axis plot.

#### Gain and Phase Margin
- **Gain Margin**: The increase in gain (in dB) the system can tolerate before becoming unstable.
- **Phase Margin**: The additional phase lag (in degrees) that can be introduced before the system becomes unstable.

These margins indicate the robustness of the system. Although MATLAB's built-in `margin()` function is used, values are derived from the custom frequency response.

#### Root Locus
The Root Locus plot shows how the poles of a closed-loop transfer function move as the system gain varies.

- **Poles**: Values of \( s \) that make the denominator zero.
- **Zeros**: Values of \( s \) that make the numerator zero.
- **Open-Loop Function**: Root locus starts from the open-loop transfer function \( G(s)H(s) \).
- **Closed-Loop Poles**: Determined by the characteristic equation.
- **Plot**: Visualizes pole movement as gain \( K \) varies from 0 to \( \infty \).
- **Stability**: System is stable if all poles remain in the left half-plane.

#### Step Response
The step response shows how the system reacts to a sudden change in input. It provides time-domain characteristics such as:
- Speed of response
- Overshoot
- Steady-state behavior

This is computed using MATLAB’s `step()` function.

#### System Info
This section displays key system characteristics:
- **Poles**: Determine stability.
- **Zeros**: Shape the frequency response.
- **Rise Time**: Time to go from 10% to 90% of final value.
- **Settling Time**: Time to remain within ±2% of final value.
- **Percent Overshoot**: How much response exceeds final value.
- **Gain Margin** and **Phase Margin**: Indicators of stability margins.

### GUI Layout and Code

#### User Inputs
- **Numerator & Denominator**: Entered using MATLAB-style syntax (e.g., `[1 2 3]` for \( s^2 + 2s + 3 \)).
- **Frequency Range**: Must be numeric; low frequency < high frequency.

#### GUI Components
- **Analysis Button**: Begins the analysis process.
- **Bode Plot Section**: Displays the Bode plot.
- **System Info Box**: Shows calculated poles, zeros, and time-domain parameters.
- **Gain and Phase Analysis**: Graphically displays gain and phase margins.
- **Root Locus Section**: Displays zero-pole movement.
- **Step Response Section**: Visualizes time-domain response.
- **Help Button**: Links to the GitHub repository for documentation and source code.

### Key Functions

- **AnalysisButtonPushed**: Collects input values, checks for errors, and starts system analysis.
- **HELPButtonPushed**: Redirects to the project GitHub page.
- **createTransferFunction**: Builds the transfer function from inputs and validates format.
- **customBode**: Generates custom Bode data.
- **plotCustomBode**, **plotGainPhase**, **plotRootLocus**, **displayStepResponse**: Plot respective components.

### App Properties
- **tf_sys**: Transfer function object created from user inputs.
- **freq**: Array holding the user-defined frequency range.
