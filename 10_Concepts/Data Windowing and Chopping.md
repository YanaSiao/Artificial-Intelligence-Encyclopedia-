## Overview of the Technique

In Edge AI applications handling continuous data streams (e.g., audio from a microphone, 3-axis acceleration from an IMU, or ECG signals), a neural network cannot process an infinite stream at once. The continuous signal must be segmented into finite, discrete time blocks called **windows** or **frames**.

To capture transitions and ensure events are not split awkwardly at a boundary, a **sliding window** approach is utilized. This introduces an overlap between consecutive frames.

## Key Parameters & Variables

Let:

- $f_s$: **Sampling Frequency** (Hz) — The number of data samples acquired per second.
    
- $T_w$: **Window Length / Duration** (seconds) — The physical time duration spans by a single block.
    
- $N_w$: **Window Size** (samples) — Number of data points inside one window.
    
    $$N_w = T_w \times f_s$$
    
- $T_s$: **Stride / Step Size** (seconds) — The amount of time the window advances forward for the next frame.
    
- $N_s$: **Stride Size** (samples) — The number of new samples advanced between consecutive frames.
    
    $$N_s = T_s \times f_s$$
    
- $O_L$: **Overlap Percentage** (%) — How much of the previous window is kept.
    
    $$O_L = \left(1 - \frac{T_s}{T_w}\right) \times 100\% = \left(1 - \frac{N_s}{N_w}\right) \times 100\%$$
    

## System-Level Trade-offs & Impact Analysis

### A. Latency

- **Initial Latency:** To compute the very first prediction, the device must wait for the buffer to fill up. This introduces an initial delay equal to the full window duration:
    
    $$\text{Latency}_{\text{initial}} = T_w$$
    
- **Inference Frame Rate / Step Latency:** After the first window is filled, a new frame is generated every $T_s$ seconds. Therefore, the architectural system latency or update rate is governed entirely by the stride:
    
    $$\text{Inference Frequency} = \frac{1}{T_s} \text{ Hz}$$
    
- _Impact:_ Decreasing the stride ($T_s$) increases the frame rate (more frequent predictions), giving the user a more responsive system. However, it does **not** change the fact that the classification is based on a historical window of $T_w$ seconds.

### B. Memory Demand (SRAM)

- **Buffer Size:** At a bare minimum, the device must allocate a ring-buffer (circular buffer) in SRAM to store the current frame of size $N_w$.
    
    $$\text{Memory Demand (Bytes)} = N_w \times \text{Bytes per Sample}$$
    
- _Impact:_ Longer window durations ($T_w$) or higher sampling frequencies ($f_s$) directly inflate the SRAM footprint. On a microcontroller with tight RAM limits, huge window sizes can cause memory allocation failures before the neural network activations are even factored in.

### C. Amount of Information & Feature Extraction Quality

- **Large Window ($T_w \uparrow$):** Captures long-term temporal dependencies and provides better frequency resolution if an FFT (Fast Fourier Transform) is performed later. However, transient, short-duration anomalies might get "washed out" or diluted by surrounding normal data.
    
- **Small Window ($T_w \downarrow$):** Excellent for tracking rapid changes and sharp transitions, but loses macro-level trends and severely limits low-frequency spectral resolution.

### D. Overlap, Missed Data, and Frame Alignment

- **No Overlap ($O_L = 0\%$, $T_s = T_w$):** Windows are adjacent. If a critical micro-event (like a keyword utterance or structural crack sound) occurs exactly across the split boundary, the information is cleaved in half. Neither window will contain enough contextual information to trigger a correct prediction, leading to effectively **missed data / misclassifications**.
    
- **High Overlap ($O_L \ge 50\%$):** Guarantees that any transient physical event will appear completely intact in at least one (if not multiple) sliding frames.

## Summary

|**Change Parameter**|**Architectural Latency**|**RAM Footprint**|**Computational Load (Inferences/sec)**|**Risk of Splitting Events**|
|---|---|---|---|---|
|**Increase Window Duration ($T_w \uparrow$)**|Increases (Initial)|🔺 Higher|Unchanged (if $T_s$ constant)|🔻 Lower|
|**Decrease Stride / Step ($T_s \downarrow$)**|Decreases (Updates faster)|Unchanged|🔺 Higher (More inferences/sec)|🔻 Lower|
|**Increase Sampling Rate ($f_s \uparrow$)**|Unchanged|🔺 Higher|Unchanged||