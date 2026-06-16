**Precision scaling** is the process of reducing the number of bits (the _word-length_) used to represent numerical values within a model. In standard Deep Learning, we typically operate in **FP32 (32-bit Floating Point)**. In the context of Edge AI, we scale this down to **INT8 (8-bit Integer)**, **INT4**, or even lower. By reducing the bit-width, we directly decrease the **energy consumption** required to move data from memory to the processor—which is often the most expensive part of the inference process—and allow for much higher **computational throughput** on specialized hardware.

## Linear, Logarithmic, and Data-Driven Quantization

Quantization methods determine how floating-point values are mapped to a discrete set of levels.

- **Linear (Uniform) Quantization** uses equally spaced levels. It is the most hardware-friendly approach because it allows for simple integer arithmetic using a scaling factor and a zero-point ($x_q = \text{round}(x/S + Z)$).
    
- **Logarithmic Quantization** maps values to powers of two. This is highly efficient for certain hardware architectures because it replaces complex multiplications with **bit-shift operations**.
    
- **Data-Driven (Non-linear) Quantization** uses techniques like _k-means clustering_ to find the optimal levels based on the actual distribution of the weights. While this minimizes the **accuracy drop** (quantization error), it is much more difficult to implement efficiently in standard hardware compared to linear mapping.

## Quantizing Weights vs. Activations

To fully optimize a system, we must address two different types of data. **Quantizing Weights** primarily reduces the **model size**, meaning the model occupies significantly less **Non-Volatile Memory (Flash)**. This is usually done "offline" before deployment. **Quantizing Activations** (the outputs of each layer) is more challenging because their values change with every input. However, this is critical for reducing **RAM usage**, which is the primary bottleneck in edge devices. If only weights are quantized, the system might still require high-precision RAM for intermediate calculations, limiting the overall efficiency gains.

## Layer-Wise and Mixed-Precision Quantization

Quantization does **not** have to be the same in each layer. In fact, **Mixed-Precision Quantization** is a powerful strategy where sensitive layers (typically the first and last layers of a network) are kept at higher precision (e.g., 8-bit or 16-bit) while the "hidden" intermediate layers are pushed to lower precision (e.g., 4-bit). This approach allows the system to achieve the best possible **Accuracy vs. Resource** trade-off by allocating the "bit budget" only where it is most needed to preserve the model's performance.

## Impact on Accuracy, Performance, and Energy

The transition to lower precision always introduces **quantization noise**, which can lead to a decrease in **Accuracy**. However, the benefits are multi-fold:

1. **Performance**: Lower-bit operations (like INT8) allow the processor to perform more operations per second (high throughput).
    
2. **Memory Footprint**: A 4x reduction in bit-width (from 32 to 8) results in a 4x reduction in the memory required to store the model.
	
3. **Energy Consumption**: Reducing the word-length minimizes the energy spent on "data movement" between the memory and the ALU.
#### **The Hardware Energy Scaling Rules**

- **1 FP32 Multiply** $\rightarrow$ **1 INT8 Multiply** = Saves **18.5x** energy.
    
- **1 FP32 Add** $\rightarrow$ **1 INT8 Add** = Saves **30x** energy.

#### **The Formula**

If a network layer requires $X$ multiplications and $Y$ additions:

$$\text{Baseline Energy (FP32)} = (X \cdot E_{\text{FP32\_Mult}}) + (Y \cdot E_{\text{FP32\_Add}})$$

$$\text{Quantized Energy (INT8)} = \left( \frac{X \cdot E_{\text{FP32\_Mult}}}{18.5} \right) + \left( \frac{Y \cdot E_{\text{FP32\_Add}}}{30} \right)$$

## Extreme Quantization: Binary and Ternary

At the limit of approximate computing, we find **Extreme Quantization**.

- **Binary Neural Networks (BNNs)** use only **1-bit** representations, where weights and activations are either -1 or +1. This transforms standard convolutions into simple **XNOR** and **Popcount** logic operations, which are orders of magnitude faster and more energy-efficient than traditional multipliers.
    
- **Ternary Neural Networks** use **1.5-bits** (typically represented as 2-bits in hardware) to allow for three levels: -1, 0, and +1. The inclusion of **zero** is crucial; it allows the model to "mask" or "prune" unimportant connections, which often leads to significantly higher accuracy than purely binary models while remaining extremely lightweight.

## The Mathematics of Linear Quantization Implementation

The implementation of quantization relies on mapping a high-precision range of values (floating-point) to a discrete, low-precision range (integers). This is essentially a **Linear Mapping** problem. In practice, we define a **Scaling Factor ($S$)** and a **Zero-point ($Z$)** to transform a real value ($r$) into a quantized value ($q$). The fundamental formula is:

$$q = \text{clamp}(\lfloor \frac{r}{S} + Z \rceil, q_{min}, q_{max})$$

where $\lfloor \cdot \rceil$ denotes rounding to the nearest integer, and the **clamp** function ensures the result does not exceed the bit-depth limits (e.g., -128 to 127 for INT8).

## Symmetric vs. Asymmetric Quantization

**Symmetric Quantization** is the simpler approach where the floating-point range is assumed to be symmetric around zero (e.g., $[-x, x]$).

- **Calculation Step-by-Step**:
    
    1. Find the maximum absolute value in the data: $\alpha = \max(|r|)$.
    2. Calculate the scale: $S = \frac{\alpha}{127}$ (for signed 8-bit).
    3. The zero-point ($Z$) is always **0**.    
    4. Quantize: $q = \text{round}(r/S)$.
    
- **When to use**: Best for **Weights**, which are typically centered around zero. It simplifies hardware because you don't need to add a zero-point offset during every multiplication.

**Asymmetric Quantization** maps the actual min/max of the data to the full integer range.

- **Calculation Step-by-Step**:
    
    1. Find $\min(r)$ and $\max(r)$.
    2. Calculate scale: $S = \frac{\max(r) - \min(r)}{q_{max} - q_{min}}$.
    3. Calculate zero-point: $Z = \text{round}(q_{min} - \frac{\min(r)}{S})$.
    4. Quantize: $q = \text{round}(r/S + Z)$.
    
- **When to use**: Essential for **Activations** (like ReLU outputs), which are always non-negative $[0, \infty)$. Using symmetric quantization here would waste half of the available integer range.

## Quantization Errors: Clipping and Rounding

Two types of errors occur during this process, both of which degrade model accuracy.

- **Rounding Errors**: These occur because we map a continuous value to the _nearest_ integer. This is "noise" that is usually small but accumulates across layers.
    
- **Clipping (Saturation) Errors**: These occur when the input value $r$ is outside the range defined by $[\min, \max]$. Any value beyond this limit is "clipped" to the maximum possible integer. This error is much more destructive than rounding because it destroys the relative magnitude of outliers.
    
- **Implementation Strategy**: To balance these, we often use **Calibration**. Instead of using the absolute $\min/\max$, we might use the 99.9th percentile to allow some clipping in exchange for better rounding precision for the majority of the data.

## The De-quantization Step

**De-quantization** is the process of reconstructing an approximation of the original floating-point value from the quantized integer. This is necessary when passing data to a layer that requires higher precision or when interpreting the final output.

- **Formula**: $\hat{r} = S \times (q - Z)$.
    Note that $\hat{r}$ is an _approximation_; the original precision lost during the rounding step cannot be recovered.

## Layer Importance and Sensitivity

The effect of quantization is **not uniform** across the network. Some layers are significantly more sensitive to precision loss than others.

- **The Bottleneck Layers**: The **First Layer** (which processes raw sensor data) and the **Last Layer** (which produces the final probability scores) are the most critical. Quantizing these to very low bits (like 4-bit) often causes a total collapse in accuracy.
    
- **Depthwise Convolutions**: In architectures like [[MobileNet 16k (2017)|MobileNet]], these layers have fewer parameters and are often more sensitive to quantization than standard "dense" convolutions.
    
- **Impact**: Consequently, we often use **Mixed Precision**, keeping the sensitive outer layers at INT8 and aggressive intermediate layers at INT4.

## PTQ vs. QAT: Strategies and Trade-offs

**Post-Training Quantization (PTQ)** is performed on a model that has already been trained in FP32.

- **Pros**: Extremely fast, requires no retraining, and needs very little data (calibration set).
- **Cons**: High accuracy drop for low-bit (e.g., <8-bit) quantization.
- **Bottlenecks**: It struggles with "outliers" in activations that weren't seen during calibration.

**Quantization-Aware Training (QAT)** simulates quantization during the training process itself using "Fake Quantization" nodes.

- **Pros**: The model "learns" to be robust to rounding and clipping, allowing for 4-bit or even 1-bit weights with minimal accuracy loss.
- **Cons**: Computationally expensive and requires the full training dataset and pipeline.

**Can we use both?** Yes. A common workflow is to start with **PTQ** to see if the accuracy is acceptable. If the drop is too large, the PTQ-quantized model is used as the starting point (initialization) for **QAT** to fine-tune the weights for low-precision performance.