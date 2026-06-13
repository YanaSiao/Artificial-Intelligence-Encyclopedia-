## The Selection and Inference Scheme (Confidence Metrics)

To decide whether a sample should exit early or continue down the backbone, the network evaluates an **inference selection scheme** based on **uncertainty** or **confidence thresholds**:

- **Entropy-based Confidence**: The system calculates the Shannon Entropy $H(y)$ of the soft targets outputted by an EEC:
    
    $$H(y) = -\sum_{i=1}^{N} y_i \log(y_i)$$
    
    - _Low Entropy_ (Close to 0) implies a sharp "one-hot" distribution where the early classifier is highly confident. If $H(y) < \text{Threshold}$, the classification is accepted, and **computational execution terminates immediately**.
        
    - _High Entropy_ implies a uniform distribution (uncertainty). The system rejects the early output, hands the activation over to the next backbone layer, and proceeds deeper.
        
- **Threshold Selection**: Thresholds are tuned on a validation set to balance the exact target latency constraints against tolerated accuracy degradation.