## The Neural Autoencoder

The fundamental idea is to train a network to output its own input (learning the identity function). Because we constrain the internal layers, the network is forced to learn a useful "summary" of the data.

### 1. Structure and Mechanism

- **Encoder:** Maps input $x$ to a hidden representation $h$.
    
- **Bottleneck:** A hidden layer with a limited number of units, creating a **compressed representation**.
    
- **Decoder:** Maps the compressed representation back to the original input space.
    
![[Pasted image 20260214180319.png]]
### 2. Sparse Autoencoders

If the hidden layer has many units, the network could simply copy the input. To prevent this, we add a **sparsity constraint**, forcing only a few neurons to be active at a time.

**Loss Function Formula:**

$$E = \|g_i(x_i|w) - x_i\|^2 + \lambda \sum_j |h_j(x_i, w)|$$

- The first term is the **Reconstruction Error**.
    
- The second term is the **Sparsity Penalty** (controlled by hyperparameter $\lambda$).