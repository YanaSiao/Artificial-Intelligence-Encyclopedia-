A CRF is a **Discriminative** model. Unlike [[Hidden Markov Models (HMM)|HMM]], which tries to model how the data is generated, CRF directly models the conditional probability $P(y|x)$.

**Why use CRF over HMM?**

- **Feature Richness:** HMMs are limited because each word only depends on its current state. CRFs can look at global features (e.g., "Is the word capitalized?", "Does it end in -ing?", "What was the word two positions ago?").
    
- **Global Normalization:** CRFs compute a score for the _entire_ sequence of labels at once, avoiding the "label bias" problem found in simpler models.
    

**The Formula:**

$$P(y|x) = \frac{1}{Z(x)} \exp \left( \sum_{i,k} \lambda_k f_k(y_i, y_{i-1}, x, i) \right)$$

- $f_k$: Feature functions.
    
- $\lambda_k$: Weights learned during training.
    
- $Z(x)$: The "Partition Function" used for normalization.

### Connecting to RNNs/LSTMs

In modern NLP, we often combine these. For example, a **BiLSTM-CRF** architecture uses a [[Gated Architectures (LSTM & GRU)|Bidirectional LSTM]] to extract high-level features from the words and then uses a **CRF layer** at the top to ensure the predicted labels make sense as a sequence (e.g., ensuring an "End-of-Entity" tag doesn't appear without a "Begin-of-Entity" tag).