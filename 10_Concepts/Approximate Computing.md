## The Philosophy of Approximation in TinyML

Approximate computing differs from traditional optimization (like code refactoring) because it alters the mathematical output of the system. In the context of Tiny Deep Learning (TinyDL), we exploit the inherent resilience of neural networks. Since deep learning models are already probabilistic and deal with noisy real-world data, they do not always require high-precision floating-point math to reach a correct classification. By relaxing the requirement for 100% numerical exactness, we can deploy models on hardware that would otherwise be too weak to run them.

## Precision Scaling and Word-Length Optimization

Precision scaling is the fundamental mechanism of approximate computing where we reduce the number of bits used to represent numerical values. In standard deep learning, we use 32-bit floating point (FP32), but edge hardware often lacks the power or specific units to handle this efficiently. By scaling down to 8-bit, 4-bit, or even 1-bit representations, we drastically reduce the energy required for every multiplication and addition. This allows the system to trade a negligible amount of mathematical precision for a massive gain in execution speed and a reduction in power consumption.

## Task Dropping and Early Termination

Task dropping allows a system to dynamically adjust its workload based on real-time constraints. In "Early Exit" architectures, a neural network is designed with multiple internal classifiers. If an early layer achieves a high enough confidence score, the system "drops" the remaining, more computationally expensive layers. This is a form of temporal approximation: we approximate the final result by stopping the inference process early, ensuring that a decision is made within a strict latency deadline or battery budget.

## Quantization: The Core Approximation Strategy

[[Quantization]] is the most common application of approximate computing in the field. It functions by mapping a large set of continuous input values to a smaller, discrete set of output levels.

- **Post-Training Quantization (PTQ)**: This is applied after the model is fully trained. It is the fastest route to approximation but may result in a higher "accuracy gap."
    
- **Quantization-Aware Training (QAT)**: This technique simulates the rounding errors of low precision during the training phase. By doing so, the model's weights "adapt" to the approximation, significantly recovering any lost accuracy.
    
- **Binary and Ternary Systems**: These are the extreme limits of quantization, using only two (-1, 1) or three (-1, 0, 1) levels. These systems replace expensive multipliers with simple logic gates, representing the pinnacle of hardware-level approximation.

## Pruning: Exploiting Structural Redundancy

While quantization approximates the _value_ of the data, [[Pruning & Sparsity|pruning]] approximates the _structure_ of the network. It identifies and removes weights or entire neurons that contribute minimally to the final output.

- **Unstructured Pruning**: Individual weights are set to zero based on their magnitude, creating a sparse matrix.
    
- **Structured Pruning**: Entire filters or channels are removed, which is often more compatible with standard hardware accelerators. By combining pruning with quantization, we achieve a "double approximation": we reduce the number of parameters (pruning) and then represent those remaining parameters with fewer bits (quantization).