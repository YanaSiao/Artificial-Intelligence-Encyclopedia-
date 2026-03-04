## 1. The Rationale: Overcoming the Bottleneck

In a standard Encoder-Decoder (Seq2Seq) model, the Encoder compresses the entire input sequence into a single fixed-size **context vector**.

- **The Problem**: For long sentences, a single vector becomes a "bottleneck." It is impossible to represent all the nuances of a 50-word sentence in one small vector.
    ![[Pasted image 20260216185048.png]]
- **The Solution**: Instead of forcing the Decoder to look only at the final state of the Encoder, **Attention** allows the Decoder to "look back" at all the hidden states of the Encoder at every step of the generation process.
    

---

## 2. Mathematical Calculation

Attention works by creating a unique context vector $c_i$ for every output step $i$. The process involves three main steps:

### Step A: Alignment Scores ($e_{ij}$)

The model calculates a score comparing the current Decoder hidden state ($s_{i-1}$) with every Encoder hidden state ($h_j$). This score represents how relevant word $j$ in the input is to word $i$ in the output.

Common **Scoring Functions**:

1. **Bilinear (Luong Attention)**: $e_{ij} = s_{i-1}^T W_a h_j$
    ![[Pasted image 20260216185946.png]]
2. **Multi-layer Perceptron (Bahdanau Attention)**: $e_{ij} = v_a^T \tanh(W_a [s_{i-1}; h_j])$
    ![[Pasted image 20260216185930.png]]

### Step B: Attention Weights ($\alpha_{ij}$)

The scores are normalized using a **Softmax** function so they sum to 1. This produces a probability distribution over the input sequence.

$$\alpha_{ij} = \frac{\exp(e_{ij})}{\sum_{k=1}^m \exp(e_{ik})}$$

### Step C: The Context Vector ($c_i$)

The final context vector is a **weighted sum** of the Encoder hidden states:

$$c_i = \sum_{j=1}^m \alpha_{ij} h_j$$

---

## 3. Properties and Advantages

- **Soft Alignment**: Unlike traditional "hard" alignment in statistical translation, Attention is differentiable. The model learns which words "align" (e.g., that "maison" in French corresponds to "house" in English) through backpropagation.
    
- **Visualizability**: We can plot the attention weights in a matrix. This makes "Black Box" neural networks interpretable, as we can see exactly which input words the model focused on to produce a specific output.
    
- **Variable Context**: The model is no longer limited by a fixed-size memory; it effectively has an "infinite" memory of the input sequence.
    

---

## 4. Hierarchical Attention Networks (HAN)

Attention can be applied at multiple levels to handle documents or long conversations.

### Levels of Attention

1. **Word-level Attention**: Determines which words are important within a sentence.
    
2. **Sentence-level Attention**: Determines which sentences are important within a document or multi-turn chat.
    

### Use Cases from Research

- **Generative Multi-turn Chatbots**: Uses hierarchical attention to track context across several "turns" of a conversation.
    
- **Document Classification**: The model can "localize" specific topic-related words (e.g., focusing on "zebra" and "stripes" to classify a text as "Science/Biology").
    
- **Sentiment Analysis**: Focuses on sentiment-rich words like "amazing" or "terrible" while ignoring neutral filler words.
    

---

## 5. Summary: Bahdanau vs. Luong

|**Feature**|**Bahdanau (Additive)**|**Luong (Multiplicative)**|
|---|---|---|
|**Alignment Score**|Uses a small Neural Network (MLP).|Uses Dot-product or Bilinear matrix.|
|**Timing**|Calculates attention _before_ the hidden state.|Calculates attention _after_ the hidden state.|
|**Complexity**|Higher computational cost.|Faster and more memory-efficient.|