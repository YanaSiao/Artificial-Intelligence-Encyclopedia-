---
status: developing
related: "[[Deep Learning MOC]]"
Topic:
---
## Theory & Core Concepts

### The Perceptron (Linear Unit)
- **Mechanism:** A basic unit that computes a weighted sum of inputs plus a bias and applies a threshold activation function.
 ![[Pasted image 20260212162355.png]]
- **Decision Boundary:** The perceptron is a linear classifier. Its decision boundary is a **hyperplane** defined by the equation $w^T x + w_0 = 0$.
- **Mathematical Representation:** $h(x) = \text{Sign}(w^T x + w_0)$.
- **Capabilities:** It can perfectly solve problems that are **linearly separable** (e.g., logical AND, logical OR).

### Convergence and Limitations
- **Convergence Theorem:** The perceptron learning algorithm is guaranteed to converge to a solution in a finite number of steps **if and only if** the training data is linearly separable.
- **The XOR Problem:** A single-layer perceptron cannot solve the XOR problem. This is because there is no single straight line (hyperplane) that can separate the "True" outputs from the "False" outputs in the XOR truth table.
- **Learning Rule:** Weights are updated based on the error: $\Delta w_i = \eta \cdot x_i \cdot (t - y)$, where $t$ is the target and $y$ is the prediction.

### Multi-Layer Perceptron (MLP)
- **Architecture:** By stacking multiple layers of neurons, we create a **Feed Forward Neural Network (FFNN)**.
- **Hidden Layers:** These layers allow the network to create non-linear decision boundaries by projecting the data into higher-dimensional spaces where it might become linearly separable.
- **Differentiability:** Unlike the original perceptron's step function, modern networks use differentiable activation functions (like Sigmoid, Tanh, or ReLU). This is necessary to use gradient-based optimization.

## PyTorch Implementation 
- In PyTorch, a perceptron layer is implemented using `nn.Linear(input_dim, output_dim)`.
- The activation function (e.g., `nn.ReLU()`, `nn.Sigmoid()`) must be applied separately to introduce non-linearity.

## Questions
> [!question] 
> **Question:** 
> **Hint:** 