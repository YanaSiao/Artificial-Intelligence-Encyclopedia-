## Joint Training vs. Alternating

Adding early exits complicates the backpropagation pipeline. Two main training approaches are utilized:

- **Joint Training (JT)**: The entire backbone and all added early classifiers are trained simultaneously using a weighted, composite loss function:
    
    $$\mathcal{L}_{\text{total}} = \sum_{e=1}^{E} \alpha_e \mathcal{L}_e(y, \hat{y}_e) + \beta \mathcal{L}_{\text{final}}(y, \hat{y}_{\text{final}})$$
    
    - _Pros_: Captures structural correlations smoothly across the entire network.
        
- **Alternating / Post-Hoc Training**: The backbone is trained to completion first. Once frozen, early classifiers are attached to intermediate layers and trained independently.