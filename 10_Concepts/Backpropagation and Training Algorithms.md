## The Learning Rate ($\eta$)

### Theory & Core Concepts

The learning rate is perhaps the most important hyperparameter in deep learning. It determines the size of the steps the optimizer takes toward the minimum of the loss function.

#### The Role of the Learning Rate

In the gradient descent update rule:

$$w_{k+1} = w_k - \eta \nabla E(w)$$

The learning rate $\eta$ scales the gradient.

- If **too high**: The updates are too large, causing the optimizer to "overshoot" the minimum. This leads to oscillations or even divergence (loss becomes NaN).
    
- If **too low**: The updates are tiny. Training becomes extremely slow, and the model is more likely to get permanently stuck in poor local minima or plateaus.

---
### Beyond Fixed Learning Rates

Static learning rates are rarely optimal. Modern training uses strategies to change $\eta$ dynamically.

#### 1. Learning Rate Schedules (Decay)

The idea is to start with a high learning rate to move quickly toward the solution, and then decrease it to "settle" into the global minimum.

- **Step Decay:** Reduce $\eta$ by a factor every $X$ epochs.
    
- **Exponential Decay:** $\eta = \eta_0 \cdot e^{-kt}$.
    
- **1/t Decay:** $\eta = \frac{\eta_0}{1 + kt}$.

#### 2. Adaptive Learning Rate Algorithms

These algorithms adjust the learning rate for _each individual parameter_ based on the history of gradients.

- **Stochastic Gradient Descent (SGD):** Updates weights after every sample. While noisy, the randomness helps escape local minima.
    
- **Momentum:** Adds a fraction $\alpha$ of the previous update to the current one. This "velocity" helps dampen oscillations in steep valleys.
    
    $$v_{k+1} = \alpha v_k - \eta \nabla E(w_k)$$
    
    $$w_{k+1} = w_k + v_{k+1}$$
    ![[Pasted image 20260212192948.png]]
- **Adagrad:** Great for sparse data; it gives larger updates to infrequent features and smaller updates to frequent ones by accumulating the sum of squared gradients.
    
- **RMSProp:** Resolves Adagrad's radical diminishing of learning rates by using a moving average of squared gradients.
    
- **Adam (Adaptive Moment Estimation):** The current industry standard. It tracks both the **mean** (1st moment, like Momentum) and the **variance** (2nd moment, like RMSProp) of the gradients.

---

### Fine-tuning and the Learning Rate

When performing **Transfer Learning** (using a pre-trained model), the learning rate must be significantly smaller (often $10^{-1}$ or $10^{-2}$ of the original rate). This is to ensure that the "knowledge" stored in the pre-trained weights is not destroyed by massive updates, but rather gently adapted to the new task.

## The Chain Rule in Backpropagation

The Chain Rule is the mathematical backbone of backpropagation. It allows us to calculate the gradient of the loss function with respect to any parameter (weight or bias) in a deep architecture by breaking down the complex derivative into a product of local derivatives.

### Mathematical Foundation

If we have a composition of functions, for example, $z = f(y)$ and $y = g(x)$, the derivative of $z$ with respect to $x$ is calculated as:

$$\frac{dz}{dx} = \frac{dz}{dy} \cdot \frac{dy}{dx}$$

In a Neural Network, for a specific weight $w_{ij}$ in layer $l$, the gradient of the loss $L$ is computed by following the "path" from the output back to that weight:

$$\frac{\partial L}{\partial w_{ij}^{(l)}} = \frac{\partial L}{\partial a_{i}^{(l)}} \cdot \frac{\partial a_{i}^{(l)}}{\partial z_{i}^{(l)}} \cdot \frac{\partial z_{i}^{(l)}}{\partial w_{ij}^{(l)}}$$

Where:

- $z_{i}^{(l)}$ is the weighted sum (pre-activation).
    
- $a_{i}^{(l)}$ is the activation output.
    
- $\frac{\partial L}{\partial a_{i}^{(l)}}$ represents the "error" signal passed back from the subsequent layer.
![[Pasted image 20260212193031.png]]
### Computational Efficiency

Backpropagation implements the chain rule using **Dynamic Programming**. Instead of calculating the derivative from scratch for every weight, the algorithm:

1. Performs a **Forward Pass** to compute and store intermediate activations.
    
2. Performs a **Backward Pass** to compute gradients, starting from the loss and reusing the "error" signals calculated in later layers to update earlier layers.
    
![[Pasted image 20260212193517.png]]
![[Pasted image 20260212193535.png]]
### Pros and Cons

|**Feature**|**Description**|
|---|---|
|**Pro: Computational Efficiency**|Avoids redundant calculations by caching intermediate derivatives; complexity is linear with respect to the number of parameters.|
|**Pro: Scalability**|Mathematically allows for the training of networks with hundreds or thousands of layers.|
|**Pro: Generality**|Applicable to any architecture as long as the operations are differentiable (CNNs, RNNs, Transformers).|
|**Con: Vanishing Gradients**|Since it is a product of derivatives, if the local gradients are small (e.g., in Sigmoid saturation), the signal "vanishes" before reaching early layers.|
|**Con: Exploding Gradients**|Conversely, if local gradients are $> 1$, the product can grow exponentially, leading to numerical instability.|
|**Con: Memory Requirement**|Requires storing all intermediate activations from the forward pass until the backward pass is complete, which can be memory-intensive for very large models.|

### Maximum Likelihood Estimation (MLE)

Maximum Likelihood Estimation is a statistical method used to estimate the parameters of a model (such as the weights $\theta$ of a neural network) by maximizing a **likelihood function**. In Deep Learning, we view the network as a probabilistic model that defines a distribution $p(y | x; \theta)$. The goal is to find the $\theta$ that makes the observed data most probable.

#### Mathematical Formulation

Given a dataset of $m$ independent and identically distributed (i.i.d.) samples, the **Likelihood Function** $L(\theta)$ is the joint probability of the data:

$$L(\theta) = \prod_{i=1}^{m} p(y^{(i)} | x^{(i)}; \theta)$$

Because products are numerically unstable (leading to underflow) and difficult to differentiate, we maximize the **Log-Likelihood** instead. Since $\log$ is a monotonic function, the maximum remains the same:

$$\ell(\theta) = \sum_{i=1}^{m} \log p(y^{(i)} | x^{(i)}; \theta)$$

In practice, optimization algorithms (like SGD) are designed to _minimize_ a function, so we minimize the **Negative Log-Likelihood (NLL)**:

$$J(\theta) = -\sum_{i=1}^{m} \log p(y^{(i)} | x^{(i)}; \theta)$$

#### Relationship to Standard Loss Functions

Many common loss functions are simply the NLL under different assumptions about the distribution of the noise/targets:

|**Assumption**|**Distribution**|**Equivalent Loss Function**|
|---|---|---|
|**Gaussian Noise**|$y = f(x; \theta) + \epsilon, \epsilon \sim \mathcal{N}(0, \sigma^2)$|**Mean Squared Error (MSE)**|
|**Binary Classes**|$y \in \{0, 1\}$, Bernoulli distribution|**Binary Cross-Entropy (BCE)**|
|**Multi-class**|$y \in \{1, \dots, K\}$, Categorical distribution|**Categorical Cross-Entropy**|

#### Comparison: MLE vs. MAP

**Maximum A Posteriori (MAP)** is a Bayesian extension of MLE. While MLE only looks at the data, MAP incorporates a **prior distribution** $p(\theta)$, representing our beliefs about the parameters before seeing any data.

- **Formula:** $\theta_{MAP} = \text{argmax}_\theta [\log p(y|x; \theta) + \log p(\theta)]$
    
- **Deep Learning Link:** Adding a Gaussian prior ($\log p(\theta) \propto -\lambda \|\theta\|^2$) is mathematically equivalent to adding **L2 Regularization (Weight Decay)** to the MLE objective.
    

#### Use Cases

- **Supervised Learning:** Deriving loss functions for regression (MSE) and classification (Cross-Entropy).
- **Unsupervised Learning:** Training Generative Models (like Variational Autoencoders or GANs) and density estimation.
- **Probabilistic Models:** Used in Gaussian Mixture Models (GMMs) and Hidden Markov Models.

#### Pros and Cons

| **Feature**                     | **Description**                                                                                                                             |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Pro: Statistical Efficiency** | As the number of samples $m \to \infty$, the MLE is the "best" possible estimator (minimum variance).                                       |
| **Pro: Principled**             | Provides a clear mathematical justification for why we use specific loss functions.                                                         |
| **Con: Overfitting**            | With small datasets, MLE can perfectly fit the noise (overfit) because it lacks a prior constraint.                                         |
| **Con: Non-convexity**          | In Deep Learning, the log-likelihood is usually non-convex, meaning gradient descent might find a local maximum rather than the global MLE. |

## Training Stability
### Weight initialization 
Weight initialization is important because it directly influences the way a network can learn. An appropriate weight initialization will lead to **efficient training**, while an inefficient one may cause overfitting or underfitting. Some choices for weight initialization can be:

- **All 0s**, not good because all the weights learn in the same way and thus they’re incapable of extracting any feature;
- **High values**, not good because big values may lead to overfitting or cause vanishing gradients because they’re closer to saturation zones in the derivatives of some functions;
- **Small values** according to a gaussian distribution **centered in 0** and with unitary variance, good for small nets but may lead to vanishing gradient in deeper ones;
- Techniques such as **Xavier and Glorot**, which are good because they try to standardize the weight initialization in a way that their variance is kept under control, so that they’ll not grow too much but still be able to learn effectively, by using a distribution with certain characteristics (variance proportional to the number of inputs) from which selecting the values;
- **Transfer learning** or fine tuning, since the nets that are transferred are assumed to be reliable;
- **Autoencoders**, taking the encoder after training, can reduce overfitting;

#### Xavier (Glorot) Initialization

- **Best for:** Symmetric activation functions like **Sigmoid** or **Tanh**.

- **Problem:** If weights start too small, the signal vanishes. If too large, the signal explodes or saturates activations.
    
- **Solution:** It initializes weights from a distribution with zero mean and a specific variance that keeps the signal variance the same across layers.
    
- **Formula:** $Var(W) = \frac{2}{n_{in} + n_{out}}$
    
- **Use Case:** Primarily used for symmetric activation functions like **Tanh** or **Sigmoid**. (Note: For ReLU, **He Initialization** is preferred).

#### He (Kaiming) Initialization

- **Best for:** **ReLU** and its variants (Leaky ReLU, ELU).
    
- **Mechanism:** Because ReLU "shuts off" half of the input space (the negative side), the variance needs to be doubled compared to Xavier to compensate for the lost signal.
    
- **Formula:** $Var(W) = \frac{2}{n_{in}}$
    
- **Logic:** This accounts for the fact that approximately 50% of the neurons are inactive at any given time.

#### Why proper initialization is important

If the initialization is "wrong," training can fail before it even starts. There are two primary risks associated with poor initialization:

1. **Vanishing Gradients:** If weights are initialized to very small values, the signal (and the gradient) shrinks as it passes through each layer. By the time the gradient reaches the early layers of a deep network, it may be so close to zero that the weights never update, preventing the network from learning.
    
2. **Exploding Gradients:** If weights are initialized to very large values, the signal grows exponentially as it passes through the layers. This leads to massive gradient updates that cause the loss to oscillate wildly or reach numerical overflow (NaN).
    
3. **Symmetry Breaking:** If all weights are initialized to the same value (e.g., all zeros), every neuron in a hidden layer will perform the same calculation and receive the same gradient update. This means the neurons will remain identical throughout training, effectively reducing a "deep" network to a single-neuron model.
More details: [[Vanishing and Exploding Gradients]]
### Batch Normalization
Batch Normalization is a technique to standardize the inputs to any network layer. It ensures that the distribution of activations remains stable as the model trains, a concept known as reducing **Internal Covariate Shift**.

#### How it Works

For a given mini-batch, BN performs the following operations on the activations $x$:

1. **Calculate Mean and Variance:** Compute the average and dispersion of the current batch.
    
2. **Normalize:** Subtract the mean and divide by the standard deviation. This centers the data at 0 with a variance of 1.    $$\hat{x}^{(i)} = \frac{x^{(i)} - \mu_B}{\sqrt{\sigma_B^2 + \epsilon}}$$
    _(Where $\epsilon$ is a tiny constant for numerical stability)._
3. **Scale and Shift:** Apply two **learnable parameters**, $\gamma$ (gamma) and $\beta$ (beta).
    
    $$y^{(i)} = \gamma \hat{x}^{(i)} + \beta$$
    
    This allows the network to "undo" the normalization if it decides that a different mean or variance is more optimal for learning.
    ![[Pasted image 20260213204405.png]]

#### Why We Use It

- **Faster Convergence:** By keeping activations in a stable range, you can use significantly **higher learning rates** without the risk of divergence.
    
- **Reduces Sensitivity to Initialization:** Because the layers are re-normalized, the exact starting scale of the weights becomes less critical.
    
- **Regularization Effect:** The noise introduced by calculating mean/variance on small batches acts as a slight regularizer, often reducing the need for Dropout.
    
- **Prevents Saturation:** It keeps inputs to Sigmoid/Tanh functions in their linear (non-saturating) regions, effectively killing the vanishing gradient problem.

#### Pros and Cons

| **Feature**                    | **Description**                                                                                                           |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| **Pro: Stability**             | Prevents gradients from vanishing or exploding by controlling activation scales.                                          |
| **Pro: Speed**                 | Networks often train in half the time compared to those without BN.                                                       |
| **Con: Batch Size Dependency** | BN works poorly with very small batch sizes (e.g., 2 or 4) because the mean/variance estimates become too noisy.          |
| **Con: Inference Complexity**  | During testing, you must use a "running average" of the mean/variance calculated during training, which adds bookkeeping. |

## Activation Functions

### Theory & Core Concepts

Activation functions are mathematical identities applied to the output of a neuron. Without them, a multi-layer neural network would simply be a composition of linear functions, which is itself a linear function ($W_2(W_1x) = W_{new}x$), rendering deep architectures useless.

#### 1. Sigmoid

- **Formula:** $\sigma(x) = \frac{1}{1 + e^{-x}}$
    
- **Range:** $(0, 1)$
    
- **Characteristics:** Historically popular as it mimics a probability or a "firing rate."
    
- **Pros:** Smooth gradient; output is normalized.
    
- **Cons:** 
	- **Vanishing Gradient:** For very high or low inputs, the gradient is near zero, effectively stopping learning.
    
    - **Not Zero-Centered:** Outputs are always positive, which can make optimization harder (zig-zagging gradients).
        
    - **Computationally Expensive:** Requires exponential calculation.

#### 2. Hyperbolic Tangent (Tanh)

- **Formula:** $\tanh(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}$
    
- **Range:** $(-1, 1)$
    
- **Pros:** Zero-centered, meaning the mean of the activations is closer to zero, which helps the model converge faster than Sigmoid.
    
- **Cons:** Still suffers from the **Vanishing Gradient** problem when the neuron saturates.
    
![[Pasted image 20260213163415.png]]
#### 3. Rectified Linear Unit (ReLU)

- **Formula:** $f(x) = \max(0, x)$
    
- **Range:** $[0, \infty)$
    
- **Pros:** 
	- **Sparsity:** Only some neurons are active at a time.
    
    - **Efficiency:** Extremely fast to compute (simple thresholding).
        
    - **Reduced Vanishing Gradient:** Gradient is 1 for all $x > 0$.
        
- **Cons:** 
	- **Dying ReLU Problem:** If a large gradient update pushes weights such that the input $x$ is always negative, the gradient becomes 0 forever. The neuron "dies" and never updates again.

#### 4. Leaky ReLU

- **Formula:** $f(x) = \max(\alpha x, x)$ where $\alpha$ is a small constant (e.g., 0.01).
    
- **Pros:** Solves the **Dying ReLU** problem by ensuring that even for negative inputs, there is a small, non-zero gradient.

#### 5. Exponential Linear Unit (ELU)

- **Formula:** $f(x) = x$ if $x > 0$, and $\alpha(e^x - 1)$ if $x \le 0$.
    
- **Pros:** 
	- Like Leaky ReLU, it avoids the dying neuron problem.
    
    - It is smoother than ReLU/Leaky ReLU at $x=0$, which can lead to faster convergence.
        
    - Brings the mean activations closer to zero.
        
- **Cons:** More computationally expensive than ReLU due to the exponential term.

![[Pasted image 20260213163502.png]]

---

### Comparison Summary Table

| **Function**   | **Range**     | **Zero-Centered** | **Main Issue**         |
| -------------- | ------------- | ----------------- | ---------------------- |
| **Sigmoid**    | (0, 1)        | No                | Vanishing Gradient     |
| **Tanh**       | (-1, 1)       | Yes               | Vanishing Gradient     |
| **ReLU**       | [0, inf)      | No                | Dying ReLU             |
| **Leaky ReLU** | (-inf, inf)   | No                | Needs $\alpha$ tuning  |
| **ELU**        | (-alpha, inf) | Yes               | Computationally slower |
![[Pasted image 20260213163906.png]]
## Questions

> [!question] 
> **Question:** The learning rate is a critical hyperparameter of gradient descent because:
> a. It might lead to unstable training if set too high.
> b. If set low, it guarantees the training is stable and will always converge.
> c. If you set it high the gradient descent moves faster toward the minimum of the loss function.
> d. It must be modified necessarily monotonically during training, so the starting value is critical if not exactly selected as it cannot be reversed.
> e. It can reduce the effectiveness of neural networks based on ReLU activation functions if set too low.
> f. It allows, if set low, to finetune pre-trained models keeping their "knowledge"
> g. If it is modified during training, starting low and increasing, it might lead to better minima in the loss function at the end of the training.
> 
> **Hint:** > - Consider the "overshooting" effect in narrow valleys of the loss landscape.
> - Think about the "Dying ReLU" problem—does it occur because an update is too small or because an update is large enough to push the neuron into a permanent inactive state?
> - For fine-tuning, evaluate if you want to perform a massive "reset" of the weights or a gentle "adaptation."
> - Reflect on whether "stable training" (slow progress) is the same thing as a "guarantee of convergence" to a global minimum.

> [!question] 
> **Question:** Consider the following 4 activation functions: Sigmoid, Tanh, Linear, ReLU. Discuss for each of them when you should use it and why.
> 
> **Hint:** 
> - **Sigmoid:** Think about binary classification and the need for a probability output between 0 and 1.
> - **Tanh:** Consider hidden layers where you want the data to be zero-centered to help optimization.
> - **Linear:** Where would you need an unbounded output that can take any real value (e.g., predicting a price)?
> - **ReLU:** Think about deep networks—which function is the most computationally efficient and best at preventing the vanishing gradient in positive regions?

> [!question] 
> **Question:** Why is it so important to choose a proper initialization for a neural network? Specifically, what are the potential consequences of a "wrong" initialization, and which methods are suggested to mitigate these issues?
> 
> **Hint:** 
> - Consider the "Vanishing" and "Exploding" gradient phenomena—how does the initial scale of weights affect the product of gradients across many layers?
> - Think about "Symmetry Breaking"—what happens if all neurons start with the exact same weight value?
> - For the suggested methods, recall which distributions are optimized for **Sigmoid/Tanh** (keeping variance constant) vs. **ReLU** (accounting for the "dead" half of the activation).

> [!question] 
> **Question:** What is the learning rate and how would you choose its value?
> 
> **Hint:** 
> - Define it as the step size in the direction of the negative gradient.
> - For the choice of value, consider the trade-off between convergence speed and stability, and mention techniques like grid search or learning rate finders.

> [!question] 
> **Question:** Which of the following is true?
> 1. A low learning rate does not compromise training.
> 2. A high learning rate might compromise training.
> 3. The higher the learning rate the better the training.
> 4. The learning rate should be reduced while training.
> 
> **Hint:** 
> - Evaluate (1) based on training time and local minima.
> - Evaluate (2) and (3) based on the risk of divergence/overshooting.
> - Evaluate (4) in the context of "Learning Rate Annealing" or schedules.

> [!question] 
> **Question:** Does the learning rate have some relationship with the "dying neurons" issue? Why?
> 
> **Hint:** 
> - Think about the ReLU activation function. 
> - If a learning rate is too high, can a single weight update push the neuron's input ($wx+b$) so far into the negative range that it never activates again?


