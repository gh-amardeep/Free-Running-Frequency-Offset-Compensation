# 📡 Free-Running Frequency Offset: Observation and Compensation

<p align="center">
  <b>Software-Defined Radio • Carrier Frequency Offset • I/Q Analysis • Frequency Synchronization</b>
</p>

An experimental Software-Defined Radio (SDR) laboratory project that studies the frequency offset between **two independent PlutoSDRs**. The project observes the resulting phase rotation in received complex I/Q samples, estimates the carrier frequency offset using phase-domain analysis, verifies the estimate in the frequency domain, and digitally compensates the offset.

---

## 🚀 Project Overview

When two independent SDRs operate with separate local oscillators, their carrier frequencies are not perfectly identical. This creates a **carrier frequency offset (CFO)** between the transmitter and receiver.

In this experiment, one PlutoSDR transmits a constant complex symbol while another independently running PlutoSDR receives it.

The frequency difference causes the received complex samples to rotate continuously in the IQ plane.

The complete experimental flow is:

```text
       PlutoSDR #1
      Transmitter
           │
           │ Constant IQ Symbol
           ▼
        RF Link
           │
           ▼
       PlutoSDR #2
       Receiver
           │
           ▼
      Complex IQ Samples
           │
           ▼
      Phase / IQ Analysis
           │
           ▼
       CFO Estimation
           │
       ┌───┴────┐
       ▼        ▼
 Phase Method  FFT Method
       │        │
       └───┬────┘
           ▼
     CFO Compensation
           │
           ▼
      Corrected IQ
           │
           ▼
     Verify Correction
```

---

## ✨ Key Features

- 📡 Two independent PlutoSDRs
- 🔄 Free-running oscillator behavior
- 🔢 Complex I/Q signal analysis
- 🌀 Observation of received phasor rotation
- 📐 Phase-based CFO estimation
- 📊 Frequency-domain CFO verification using FFT
- 🔧 Digital frequency-offset compensation
- 🎯 Verification of corrected I/Q samples
- 🐍 Python-based SDR signal processing
- 📓 Jupyter Notebook implementation

---

# 🧪 Experiment Setup

The experiment uses two independently operating PlutoSDRs.

### Transmitter

The first PlutoSDR generates and transmits a constant complex IQ symbol.

### Receiver

The second PlutoSDR receives the transmitted signal and records the resulting complex baseband samples.

Because the transmitter and receiver use independent local oscillators, a frequency difference appears in the received signal.

Conceptually:

```text
Tx Frequency ≠ Rx Frequency
          ↓
    Frequency Offset
          ↓
   Phase Rotation
          ↓
Rotating IQ Phasor
```

---

# 📡 Task 1 — Transmit a Constant Symbol

A constant complex symbol is transmitted from the first PlutoSDR.

For an ideal synchronized system, a constant transmitted symbol would produce a stable point in the IQ plane.

With independent oscillators, however, the received symbol rotates with time.

The experiment therefore uses a constant signal as a simple way to make the frequency offset clearly observable.

---

# 🌀 Task 2 — Observe the Rotating Phasor

The received complex samples can be represented as:

```text
z[n] = I[n] + jQ[n]
```

The received signal is visualized in the complex plane.

Instead of remaining at a single point, the samples form a rotating trajectory.

Conceptually:

```text
              Q
              ↑
          •       •
       •             •
      •       ○       •
       •             •
          •       •
              │
              └────────→ I
```

This rotation is the visible effect of the carrier frequency difference between the independent SDRs.

---

# 📐 Task 3 — Estimate CFO from Phase

The phase of each received complex sample is obtained using:

```python
np.angle(samples)
```

The phase is then unwrapped so that continuous phase evolution can be analyzed.

For a frequency-offset signal, phase changes approximately linearly with time:

```text
Phase
  │
  │          /
  │        /
  │      /
  │    /
  │  /
  └────────────────→ Time
```

The slope of this phase-versus-time relationship provides an estimate of the frequency offset.

The basic relationship is:

```text
Δf = slope / (2π)
```

where:

- `Δf` = frequency offset in Hz
- `slope` = phase change rate in rad/s

---

# 📊 Task 4 — Frequency-Domain Verification

The estimated CFO is independently checked using frequency-domain analysis.

The received complex signal is transformed using the Fast Fourier Transform (FFT):

```python
np.fft.fft()
```

The corresponding frequency axis is then examined to identify the dominant frequency component.

Conceptually:

```text
Time Domain
     │
     ▼
    FFT
     │
     ▼
Frequency Domain
     │
     ▼
Dominant Frequency
     │
     ▼
CFO Estimate
```

Using two independent estimation methods provides a useful cross-check between:

```text
Phase-Domain Estimate
        ↕
FFT-Based Estimate
```

---

# 🔧 Task 5 — Digital CFO Compensation

After estimating the frequency offset, the received signal is digitally corrected.

The compensation process applies the opposite phase rotation to the received samples.

Conceptually:

```text
Received Signal
      │
      ▼
Estimated CFO
      │
      ▼
Opposite Phase Rotation
      │
      ▼
Compensated Signal
```

If the estimated offset is correct, the continuous rotation of the received phasor should be significantly reduced.

---

# 🎯 Task 6 — Verify the Compensation

The compensated samples are analyzed again in the IQ plane.

Before compensation:

```text
Rotating Phasor
      ↓
Circular / Rotating Distribution
```

After compensation:

```text
Reduced Phase Rotation
      ↓
Much More Stable IQ Distribution
```

This provides a visual verification that the estimated frequency offset has been compensated.

---

# 📈 Signal Processing Concept

The experiment demonstrates an important relationship between frequency offset and phase.

A frequency offset produces a phase term that changes with time:

```text
φ(t) = 2πΔf t + φ₀
```

Therefore:

```text
dφ(t)/dt = 2πΔf
```

and consequently:

```text
Δf = (1 / 2π) · dφ(t)/dt
```

This is the fundamental principle used for the phase-based CFO estimation in the experiment.

---

# 🧩 Complete Processing Pipeline

```text
┌───────────────────────┐
│   PlutoSDR #1         │
│   Transmitter         │
└───────────┬───────────┘
            │
            │ Constant Symbol
            ▼
       RF Transmission
            │
            ▼
┌───────────────────────┐
│   PlutoSDR #2         │
│   Receiver             │
└───────────┬───────────┘
            │
            ▼
      Complex I/Q Data
            │
            ▼
     Phase Extraction
            │
            ▼
      Phase Unwrapping
            │
            ▼
      CFO Estimation
            │
       ┌────┴────┐
       ▼         ▼
    Phase       FFT
    Method     Method
       │         │
       └────┬────┘
            ▼
     CFO Compensation
            │
            ▼
     Corrected I/Q Data
            │
            ▼
      Verification
```

---

# 📊 Results

Recommended outputs to include in the repository:

```text
output-images/
├── rotating-phasor.png
├── phase-vs-time.png
├── cfo-fft.png
├── before-compensation.png
└── after-compensation.png
```

### Rotating Phasor

Visualizes the received I/Q samples before CFO compensation.

```markdown
![Rotating Phasor](output-images/rotating-phasor.png)
```

### Phase Evolution

Shows the unwrapped phase as a function of time.

```markdown
![Phase Evolution](output-images/phase-vs-time.png)
```

### Frequency-Domain Verification

Shows the FFT spectrum used to verify the estimated frequency offset.

```markdown
![CFO FFT](output-images/cfo-fft.png)
```

### Before and After Compensation

Comparing the I/Q distributions before and after compensation provides a visual demonstration of the correction.

```markdown
![Before Compensation](output-images/before-compensation.png)

![After Compensation](output-images/after-compensation.png)
```

---

# 🛠️ Technologies Used

### Hardware

- ADALM-Pluto / PlutoSDR × 2

### Programming

- Python
- Jupyter Notebook

### Signal Processing

- NumPy
- SciPy
- FFT
- Complex I/Q processing
- Phase extraction
- Phase unwrapping
- Frequency-offset estimation
- Digital frequency correction

---

# 📁 Project Structure

```text
Free-Running-Frequency-Offset-Compensation/
│
├── README.md
│
├── amar_dcct_lab2.ipynb
│
└── output-images/
    ├── rotating-phasor.png
    ├── phase-vs-time.png
    ├── cfo-fft.png
    ├── before-compensation.png
    └── after-compensation.png
```

---

# ▶️ How to Run

### 1. Hardware Setup

Connect two PlutoSDRs and configure their network/IP addresses according to the experimental setup.

### 2. Open the Notebook

Open:

```text
amar_dcct_lab2.ipynb
```

in Jupyter Notebook or a compatible environment.

### 3. Configure the SDRs

Configure one PlutoSDR as the transmitter and the other as the receiver.

### 4. Run the Experiment

Execute the notebook cells sequentially to:

1. Transmit the constant complex symbol.
2. Receive the signal using the independent PlutoSDR.
3. Visualize the rotating phasor.
4. Extract and unwrap phase.
5. Estimate the carrier frequency offset.
6. Verify the estimate using FFT.
7. Apply digital CFO compensation.
8. Verify the corrected signal.

---

# 🎓 Learning Outcomes

This project provides practical understanding of:

- Software-Defined Radio
- Carrier Frequency Offset
- Independent local oscillators
- Complex baseband signals
- I/Q representation
- Phase rotation
- Phase unwrapping
- FFT-based frequency analysis
- Frequency synchronization
- Digital frequency-offset compensation
- Experimental SDR measurements

---

# 💡 Key Insight

A very small frequency difference between two independent oscillators becomes visible as a continuous rotation of the received complex signal.

The experiment connects three important concepts:

```text
Frequency Offset
       ↓
Phase Rotation
       ↓
CFO Estimation
       ↓
Digital Compensation
```

This provides an intuitive experimental demonstration of why **frequency synchronization is important in digital communication systems**.

---

# 🔬 Possible Future Improvements

The core experiment can later be extended with:

- Automatic CFO tracking
- Time-varying CFO estimation
- More robust synchronization algorithms
- Costas-loop based carrier recovery
- Real-time CFO compensation
- Performance comparison under different SNR conditions
- CFO impact on digitally modulated communication signals

---

## ⭐ Project Highlights

```text
📡 Dual PlutoSDR
🌀 Rotating I/Q Phasor
📐 Phase-Based CFO Estimation
📊 FFT Verification
🔧 Digital CFO Compensation
🎯 Compensation Verification
⚡ DSP
🐍 Python
📓 Jupyter Notebook
```

---

## 👨‍💻 Author

**AmarDeep Dwivedi**

M.Tech
Electrical Engineering — CSPML  
IIT Dharwad

---

## 📜 License

This project is intended for educational and research purposes.

---

<p align="center">
  <b>📡 Observe → Estimate → Compensate → Verify</b>
</p>
