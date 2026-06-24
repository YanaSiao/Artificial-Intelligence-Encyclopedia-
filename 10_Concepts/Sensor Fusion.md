## Sensor Fusion via Concatenation

Sensor fusion combines data from multiple heterogeneous sensors (e.g., merging a 3-axis accelerometer with a temperature sensor or a microphone) to improve prediction accuracy and robustness beyond what a single sensing modality could achieve.

### Feature-Level Fusion (Early Fusion)

In Edge AI pipelines, **Concatenation** is typically performed at the feature level after each raw signal has been windowed, preprocessed, and reduced to a set of statistical features.

Let:

- $\mathbf{x}_{\text{sensorA}} = [f_1, f_2, \dots, f_m]$ be a feature vector of length $m$ extracted from Sensor A.
    
- $\mathbf{x}_{\text{sensorB}} = [g_1, g_2, \dots, g_n]$ be a feature vector of length $n$ extracted from Sensor B.
    

The fused feature vector $\mathbf{x}_{\text{fused}}$ is constructed by sequentially appending (concatenating) the vectors together:

$$\mathbf{x}_{\text{fused}} = [\mathbf{x}_{\text{sensorA}} \,\|\, \mathbf{x}_{\text{sensorB}}] = [f_1, f_2, \dots, f_m, g_1, g_2, \dots, g_n]$$

The resulting tensor has a total dimensionality of $m + n$. This single, unified vector is then fed straight into the input layer of the Machine Learning or Deep Learning model.

### Why use Concatenation on the Edge?

- **Simplicity:** It requires zero complex mathematical alignment at runtime; it is a straightforward memory copy operation (`memcpy`) in C/C++.
    
- **Preserves Relationships:** It allows downstream neural network layers (like fully connected layers) to naturally learn the cross-dependencies and correlations between different sensor types.
    

## 2. Feature Rescaling: Normalization vs. Standardization

When fusing features via concatenation, the features often originate from completely different physical units (e.g., Sensor A measures acceleration in $m/s^2$ with values between $-10$ and $+10$, while Sensor B measures atmospheric pressure in Pascals around $101,325$).

> [!CAUTION]
> 
> **The Scale Imbalance Trap**
> 
> If you feed an unscaled concatenated vector into a neural network, the features with the largest absolute numerical values (e.g., Pascal units) will completely dominate the loss function gradients. The network will effectively "ignore" the smaller-scale features (like acceleration), rendering the sensor fusion useless. To fix this, you must apply feature scaling _after_ extraction but _before_ the model.

### A. Normalization (Min-Max Scaling)

Normalization shifts and rescales the data so that all feature values fall strictly within a bounded range, typically $[0, 1]$ or $[-1, 1]$.

#### Formula:

$$x_{\text{norm}} = \frac{x - x_{\text{min}}}{x_{\text{max}} - x_{\text{min}}}$$

- **When to use:** When you know the absolute operational bounds of the sensor (e.g., an 8-bit ADC outputting values between $0$ and $255$) and the distribution of the data does not follow a normal (Gaussian) curve.
    
- **Edge Advantage:** Extremely efficient for microcontrollers. It only requires one subtraction and one division per element using static pre-calculated constants ($x_{\text{min}}$ and $x_{\text{max}}$ derived during training).
    

### B. Standardization (Z-Score Normalization)

Standardization transforms the features so that they have a mean ($\mu$) of $0$ and a standard deviation ($\sigma$) of $1$. The data is not bounded to a specific range.

#### Formula:

$$x_{\text{std}} = \frac{x - \mu}{\sigma}$$

- **When to use:** Highly recommended for neural networks and optimization algorithms (like gradient descent). It handles outliers much better than Min-Max scaling, because outliers won't compress the rest of the valid sensor data into a tiny fractional range.
    
- **Edge Advantage:** The mean ($\mu$) and standard deviation ($\sigma$) for each feature are calculated offline during training. At runtime, the microcontroller simply performs a Multiply-Accumulate style operation: $x_{\text{std}} = (x - \mu) \cdot \frac{1}{\sigma}$.
    

## Summary Matrix for Sensor Fusion Preprocessing

|**Method**|**Mathematical Goal**|**Handling of Outliers**|**MCU Compute Cost**|
|---|---|---|---|
|**Normalization (Min-Max)**|Forces values into a strict $[0, 1]$ or $[-1, 1]$ range.|**Poor.** Extreme outliers compress normal data into a narrow band.|Very Low (1 subtract, 1 divide/multiply)|
|**Standardization (Z-Score)**|Centers data around $\mu=0$ with variance $\sigma^2=1$.|**Good.** Preserves the relative variance of outliers without breaking the scale.|Very Low (1 subtract, 1 multiply)|