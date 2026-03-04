---
Topic:
  - dropout
  - early stopping
  - weight decay
---
## Early Stopping: Limiting Overfitting by Cross-validation

Early stopping is a form of regularization used to terminate the training process before the model begins to "memorize" the noise in the training data. It relies on monitoring the model's performance on a separate validation set.

### The Mechanism

1. **Split the data:** The dataset is divided into Training, Validation, and Test sets.
2. **Monitor Loss:** During training, we track both the **Training Loss** and the **Validation Loss**.
3. **The Turning Point:** 
	* Initially, both losses decrease as the model learns general patterns.
    - Eventually, the training loss continues to decrease, but the validation loss starts to increase. This indicates the start of **overfitting**.
4. **Stop Training:** Training is halted at the point where the validation error is at its minimum. The weights from this specific iteration are saved as the final model.

### Mathematical Context

While early stopping doesn't have a single "formula" like a loss function, it can be viewed as an implicit constraint on the weight optimization. It limits the number of gradient descent steps $t$, effectively restricting the weights from growing too large.

If we assume a simple quadratic loss, early stopping is mathematically related to **L2 Regularization (Weight Decay)**. Both limit the effective complexity of the model, but early stopping does so in the "time" domain (iterations) rather than the "space" domain (weight magnitude).
### Pros and Cons

| **Feature**                | **Description**                                                                                                                           |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **Pro: Efficiency**        | Saves time and computational resources by ending training early.                                                                          |
| **Pro: Simplicity**        | Requires no changes to the loss function or architecture.                                                                                 |
| **Con: Data Requirement**  | Requires a dedicated validation set, which reduces the amount of data available for training.                                             |
| **Con: "Patience" Tuning** | If the "patience" (number of epochs to wait) is too low, training might stop during a temporary plateau before reaching a better minimum. |
## Weight Decay: Limiting Overfitting by Weights Regularization

Weight decay is a regularization technique that penalizes large weights by adding a constraint term directly to the cost function. It is based on the principle that smaller weights lead to a "simpler" model that is less likely to fit the noise in the training data.

### Mathematical Formulation

Weight decay modifies the original loss function $E_0(w)$ by adding a regularization term (typically the $L_2$ norm of the weights):

$$E(w) = E_0(w) + \frac{\lambda}{2} \|w\|^2$$

Where:

- $E_0(w)$ is the standard loss (e.g., Cross-Entropy).
    
- $\lambda$ (Lambda) is the **regularization strength** hyperparameter.
    
- $\|w\|^2 = \sum w_i^2$ is the squared sum of all weights in the network.
    

**The Update Rule:**

When we take the gradient of this new loss function for SGD, the weight update becomes:

$$w_{k+1} = w_k - \eta \left( \frac{\partial E_0}{\partial w} + \lambda w_k \right)$$

$$w_{k+1} = (1 - \eta\lambda)w_k - \eta \frac{\partial E_0}{\partial w}$$

The term $(1 - \eta\lambda)$ is why it is called **Weight Decay**: at every step, the weight is first "decayed" by a small factor before the standard gradient update is applied.

### Pros and Cons

| **Feature**                    | **Description**                                                                                                                         |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| **Pro: Differentiable**        | Fits perfectly into the gradient descent framework as a simple addition to the gradient.                                                |
| **Pro: Interpretability**      | Directly corresponds to a Gaussian prior in Maximum A Posteriori (MAP) estimation.                                                      |
| **Con: Parameter Sensitivity** | Choosing $\lambda$ is difficult. If too high, the model underfits (weights are pushed too close to zero); if too low, it has no effect. |
| **Con: Global Bias**           | It treats all weights equally, whereas some weights might actually need to be large to represent specific features.                     |

## Dropout: Limiting Overfitting by Stochastic Regularization

Dropout is a computationally efficient regularization technique where, during each training iteration, a subset of neurons is randomly "dropped out" (temporarily removed). This forces the network to learn more robust features and prevents the "co-adaptation" of neurons.

### The Mechanism

1. **Training Phase:** For each training case and each layer, every neuron has a probability $p$ (a hyperparameter, typically 0.5 for hidden layers) of being ignored.
    
2. **Sparsity:** The remaining sub-network is trained on that specific batch. This effectively means that for a network with $n$ neurons, we are sampling from $2^n$ possible sub-architectures.
    
3. **Testing/Inference Phase:** Dropout is **turned off**. All neurons are used, but their outgoing weights are scaled by $(1-p)$ to account for the fact that more neurons are active than during training. This ensures the expected total input to a neuron at test time is the same as during training.
![[Pasted image 20260212195136.png]]

### Mathematical Interpretation

Dropout can be seen as a way of performing **Model Averaging** (Ensemble Learning) with an exponential number of models sharing the same parameters. By randomly removing neurons, we prevent the network from relying too heavily on any single input or specific combination of inputs.

Mathematically, if $y$ is the output of a layer, dropout applies a mask $r$ of Bernoulli variables:

$$\tilde{y} = r \odot y, \quad r \sim \text{Bernoulli}(1-p)$$

### Pros and Cons

|**Feature**|**Description**|
|---|---|
|**Pro: Robustness**|Prevents "co-adaptation," where neurons only work well in the presence of specific other neurons.|
|**Pro: Approximate Ensemble**|Provides the benefits of training many different models without the actual cost of doing so.|
|**Con: Training Time**|Usually requires more training epochs because the gradient is noisier (only a fraction of weights are updated per batch).|
|**Con: Inconsistent Behavior**|If not handled correctly at test time (scaling weights), the model's predictions will be completely inaccurate.|
## Comparison 

|**Feature**|**Early Stopping**|**Weight Decay (L2)**|**Dropout**|
|---|---|---|---|
|**Method**|Stops training based on validation performance.|Penalizes large weights in the loss function.|Randomly deactivates neurons during training.|
|**Computational Cost**|Low (actually reduces training time).|Low (adds a small term to the gradient).|Moderate (requires more epochs to converge).|
|**Hyperparameters**|Patience (how long to wait after the minimum).|$\lambda$ (regularization strength).|$p$ (probability of dropping a neuron).|
|**Main Advantage**|Very intuitive and automatically prevents over-training.|Provides a smooth differentiable constraint.|Forces the network to learn redundant representations.|
## Questions

> [!question] Generalization vs. Speed
> **Question:** Identify whether the following techniques primarily address **Generalization** or **Training Speed/Stability**:
> 1. Dropout
> 2. Weight decay
> 3. Early stopping
> 4. Learning rate 
> 5. Xavier initialization 
> 6. Batch Normalization
> 7. ReLU activation functions 
> 8. Shortcut / Skip connections
> 
> **Hint:** Think about whether these methods are designed to stop the model from "memorizing" the training data or to help the loss function reach its minimum faster. 
> **Hint:** Focus on the "flow" of training. Do these help the gradients stay healthy and the weights update efficiently, or are they meant to simplify the final model? 
> **Hint:** Consider the "Vanishing Gradient" problem. These techniques are often used to ensure that gradients can actually reach the early layers of a very deep network.

