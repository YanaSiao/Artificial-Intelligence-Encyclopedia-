The typical Edge AI processing pipeline translates physical phenomena captured by sensors into an actionable prediction. Because running a neural network on continuous, raw, high-frequency sensor data is computationally prohibitive on constrained edge devices, data must pass through a strict preprocessing pipeline first.

Raw Signal ──> Data Windowing & Chopping ──> DSP & Noise Reduction ──> Feature Extraction ──> ML Inference


## The Pipeline Stages

### 1. [[Data Windowing and Chopping]]

- **Concept:** Continuous physical signals (like audio, accelerometer data, or ECG) must be broken into finite, manageable pieces before processing.
    
- **Key Techniques:** * Fixed-size windowing (e.g., 1-second chunks).
    - Sliding windows with overlapping parameters (e.g., 50% overlap) to prevent losing critical events that occur exactly at a window boundary.


### 2. [[Signal Preprocessing and Noise Filtering]]`

- **Concept:** Raw chunks are typically noisy and contain unwanted artifacts. Digital Signal Processing (DSP) is used to clean the signal and isolate the relevant physical phenomena.
    
- **Key Techniques:**    
    - **Filtering:** Low-pass, high-pass, or band-pass filters to cut off out-of-band noise.
        
    - **Normalization:** Scaling the amplitudes to prevent sensor drift or clipping from skewing downstream math.

### 3. [[Feature Extraction from pre-processed data]]

- **Concept:** Instead of feeding raw time-domain samples straight into a resource-heavy Machine Learning model, the data dimensionality is drastically reduced by extracting hand-crafted features. This saves significant memory and compute energy.
    
- **Key Techniques:**
    - **Time-domain features:** Mean, variance, zero-crossing rate, root-mean-square (RMS).
        
    - **Frequency-domain features:** Fast Fourier Transform (FFT) coefficients, spectral energy.
        
    - **Edge AI Standards:** Mel-Frequency Cepstral Coefficients (MFCCs) for audio/speech apps, Spectrograms for vibration analysis.