### The Problem: Localization as Regression

Unlike classification (where we predict a category), localization is a **Regression** task because the output is a set of continuous numbers.

- **Target**: A Bounding Box defined by 4 variables: $(x, y, w, h)$.
    
    - $(x, y)$: Coordinates of the center (or top-left corner).
        
    - $(w, h)$: Width and height of the box.
        

### Multi-task Learning

In practice, we don't just localize; we classify and localize simultaneously. The network splits into two "heads" after the FEN:

1. **Classification Head**: Output is a Softmax probability (e.g., "Dog").
    
2. **Regression Head**: Output is 4 linear units $(x, y, w, h)$.
    

**The Loss Function**: $L_{total} = L_{cls} + \lambda L_{reg}$

- $L_{cls}$: Cross-Entropy.
    
- $L_{reg}$: Usually **L2 (Mean Squared Error)** or **L1 (Mean Absolute Error)**.
    
- $\lambda$: A hyperparameter to balance the two tasks.