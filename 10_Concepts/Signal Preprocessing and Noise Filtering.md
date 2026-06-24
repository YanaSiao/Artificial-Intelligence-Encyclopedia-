## Filtering (Noise Reduction)

Raw physical sensor signals capture unintended environmental noise alongside the actual phenomena. For instance, high-frequency thermal noise can skew accelerometer patterns, or power-line interference (50/60 Hz) can degrade ECG or audio recordings. Filtering cleanses the data so the neural network learns real structural features instead of transient noise patterns.

### Techniques

Digital filters process discrete time-series data using past inputs and past outputs.

- **Moving Average / Windowed Mean Filter:**
    
    - _Mechanism:_ Replaces each data point with the average of its neighbors within a window of size $M$.
        
    - _Edge Advantage:_ Extremely fast and cheap computationally.
        
    - _Downside:_ Blurs sharp transitions and acts purely as a rudimentary low-pass filter.
        
- **Finite Impulse Response (FIR) Filters:**
    
    - _Mechanism:_ The output is a weighted sum of the current and past input samples only:
        
        $$y[n] = \sum_{k=0}^{N} b_k \cdot x[n-k]$$
        
    - _Edge Advantage:_ Always stable, linear phase response (preserves signal timing/shape).
        
    - _Downside:_ Requires a high filter order $N$ (many Multiply-Accumulate operations) to achieve a sharp frequency cutoff, increasing latency and memory overhead.
        
- **Infinite Impulse Response (IIR) Filters:**
    
    - _Mechanism:_ Uses feedback by combining past inputs _and_ past outputs:
        
        $$y[n] = \sum_{k=0}^{N} b_k \cdot x[n-k] - \sum_{m=1}^{M} a_m \cdot y[n-m]$$
        
    - _Edge Advantage:_ Achieves sharp cutoffs with very low orders ($N, M$ are small), requiring significantly less RAM and CPU cycles than equivalent FIR filters.
        
    - _Downside:_ Can become unstable if poorly designed, and introduces phase distortion.

## Resampling (Downsampling and Upsampling)

- **Downsampling:** Reducing the sampling rate matches the maximum frequency required by the application. Running a model at 100 Hz when the underlying movement only occurs below 10 Hz wastes massive memory and compute cycles.
    
- **Upsampling:** Standardizing sample rates across heterogeneous sensor hardware. If model version A expects 50 Hz data, but a new sensor provides 25 Hz data, upsampling resolves the input tensor mismatch.

### Techniques

- **Downsampling (Decimation):**
    
    - _Mechanism:_ Retaining every $M$-th sample while dropping the rest.
        
    - _Critical Rule (Anti-Aliasing):_ You **must** apply a low-pass filter _before_ discarding samples. According to the Nyquist theorem, if you downsample to a new frequency $f_{\text{new}}$, any frequency component above $f_{\text{new}}/2$ will alias into lower frequencies and corrupt the data.
        
- **Upsampling (Interpolation):**
    
    - _Mechanism:_ Inserting $L-1$ zeros between original samples, followed by a low-pass filter (interpolation filter) to smooth out the artificial steps.
        
    - _Edge Alternative:_ Instead of full zero-stuffed filtering, edge microcontrollers often use simple **Linear Interpolation** or **Cubic Splines** to compute the value of intermediate points directly, saving buffer space.


## Reconstruction of Missing Data (Imputation)

Sensors communicating over SPI/I2C buses, BLE, or noisy analog lines can drop packets, experience hardware brownouts, or timeout. If an entry in a 1D tensor buffer is blank or NaN (Not a Number), a convolutional or dense layer cannot execute mathematically. Missing data must be cleanly repaired on the fly.

### Techniques

- **Zero Fill / Constant Imputation:**
    
    - _Mechanism:_ Missing entries are set to zero or the global channel mean.
        
    - _Edge Evaluation:_ Computational cost is effectively zero. However, it introduces sudden, non-physical step jumps that can severely throw off neural network filters.
        
- **Sample-and-Hold (Last Observation Carried Forward):**
    
    - _Mechanism:_ Duplicates the last valid received sensor value until a new one arrives.
        
    - _Edge Evaluation:_ Safe and low overhead, though it introduces artificial flatlines if a large block of data is missing.
        
- **Linear Interpolation:**
    
    - _Mechanism:_ Estimates the missing point $x_t$ using the last known sample $x_a$ and next known sample $x_b$:
        
        $$x_t = x_a + (t - a) \cdot \frac{x_b - x_a}{b - a}$$
        
    - _Edge Evaluation:_ The sweet spot for embedded systems. It requires minimal math and ensures smooth continuity across data drops.
        
- **K-Nearest Neighbors (KNN) or Autoregressive (AR) Imputation:**
    
    - _Mechanism:_ Uses statistical prediction to guess missing data based on temporal context.
        
    - _Edge Evaluation:_ Too heavy for resource-constrained microcontrollers; generally avoided unless working on more powerful Linux edge gateways.
        

## Summary

|**Preprocessing Step**|**Best Practice Technique for MCUs**|**Key Hardware Metric Impact**|
|---|---|---|
|**Noise Filtering**|Low-order **IIR Filter** (e.g., Butterworth)|Minimizes MAC operations vs. FIR, preserving CPU cycles.|
|**Rate Change**|**Decimation** with built-in hardware low-pass.|Shrinks input buffer size, lowering peak SRAM.|
|**Missing Data**|**Linear Interpolation**|Low latency, keeps signal mathematically continuous for tensors.|