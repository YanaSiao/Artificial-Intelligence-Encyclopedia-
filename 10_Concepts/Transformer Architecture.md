The Transformer (Vaswani et al., 2017) abandoned recurrence (RNNs) and convolutions (CNNs) entirely, relying solely on **Attention Mechanisms**. This allows for massive **parallelization** and the ability to capture dependencies between words regardless of their distance in a sentence.


---
### The Core Idea: Parallelism and Global Reach

Before Transformers, [[Recurrent Neural Networks (RNN)|sequence models]] (like LSTMs) processed tokens one by one. To relate the 1st word to the 100th word, the signal had to pass through 99 hidden states, leading to information loss.

The Transformer's central idea is **[[Self-Attention (Intra-Attention)]]**: every word in a sequence "looks" at every other word simultaneously. This allows the model to:

- **Parallelize training:** No waiting for the previous hidden state.
- **Capture Long-Range Dependencies:** The distance between words does not affect the model's ability to relate them.

---

|**Component**|**Primary Purpose**|
|---|---|
|**Positional Encoding**|Restores the word order lost by moving away from RNNs.|
|**Self-Attention**|Captures global context and relationships between distant words.|
|**FFNN**|Adds non-linearity and allows the model to learn complex patterns at each position.|
|**Residuals/Norm**|Ensures the model can be stacked deep (12+ layers) without losing signal.|

## The Input Module

Since Transformers process all words in a sequence simultaneously, they have no inherent sense of word order (unlike RNNs).

- **Input Embeddings:** Converts tokens into dense vectors.
    
- **Positional Encoding:** A vector is added to the embedding to provide information about the word's position in the sequence.
    
    - **Why it's needed:** Without this, the model would treat the sentence "The dog bit the man" exactly the same as "The man bit the dog." It uses sine and cosine functions of different frequencies to encode relative and absolute positions.

---

## The Transformer Stack

The model is composed of a stack of $N$ identical layers (usually $N=6$ or $N=12$).

- **The Encoder:** Processes the input sequence to create a high-level representation of the context.
    
- **The Decoder:** Uses the Encoder's output and its own previous outputs to generate the final sequence (e.g., a translation).
    
- **The "Stack" Logic:** Each layer in the stack refines the representation. Lower layers often capture low-level syntax (grammar), while higher layers capture high-level semantics (meaning).

---

## Inside the Transformer Block

Each block is divided into two main sub-layers, wrapped in **Residual Connections** and **Layer Normalization**.

### A. Multi-Head Self-Attention (MHA)

- **What it does:** Allows each word to "attend" to every other word in the sequence to determine which are most relevant.
    
- **Why it's needed:** It captures **long-range dependencies**. In a sentence like _"The chef, who worked in the famous restaurant for years, was out of food,"_ MHA allows the model to directly link "chef" to "was" despite the 10 words in between.

### B. Feed-Forward Neural Network (FFNN)

- **What it does:** A simple position-wise fully connected network (two linear layers with a ReLU/GELU activation).
    
- **Why it's needed:** While Attention mixes information _across_ positions, the FFNN processes information _at_ each position independently. It provides the model with the "processing power" to transform the attention-weighted features into a more complex representation.

### C. The "Add & Norm" (Residuals)

- **What it does:** $LayerNorm(x + Sublayer(x))$.
    
- **Why it's needed:** **Residual connections** (the "$+ x$") allow gradients to flow directly through the network, preventing the "Vanishing Gradient" problem in deep stacks. **Layer Normalization** keeps the activations in a stable range, speeding up training.

---

## The Output Module

The final representation from the top of the Decoder stack is converted back into words.

- **Linear Layer:** Projects the high-dimensional vector back to the size of your vocabulary ($V$).
    
- **Softmax:** Converts the scores into a probability distribution, allowing the model to pick the most likely next word.
---

## Pros, Cons, and the Bottleneck

|**Pros**|**Cons**|
|---|---|
|**Highly Parallelizable:** Much faster to train on GPUs than RNNs.|**No Recurrent Bias:** Requires much more data to learn the concept of "order" from scratch.|
|**Global Context:** Path length between any two tokens is $O(1)$.|**High Memory Usage:** Storing the $N \times N$ attention matrix for long sequences is expensive.|
|**State-of-the-art:** Foundation for BERT, GPT, and modern LLMs.|**Quadratic Complexity:** The main bottleneck.|

### The Bottleneck: Quadratic Complexity $O(n^2)$

The primary bottleneck is the **Self-Attention Matrix**. Because every word attends to every other word, a sequence of length $n$ requires $n^2$ calculations.

- If you double the sentence length, the computational cost and memory requirement **quadruple**.
    
- This is why many models (like BERT) have a maximum context window of 512 tokens.

### Layers vs. Logic: The 96-Layer Bottleneck

It is a common misconception that more layers equal more logical "steps." While a 96-layer model (like GPT-3) is incredibly deep, it is fundamentally a **parallel processor** within a single forward pass.

- **The "One-Shot" Forward Pass**: When the model generates a single token, the data moves through all 96 layers. However, each layer is looking at the _same_ snapshot of the input context. Layer 50 can see what Layer 49 produced, but it cannot "wait" for the result of a logical calculation that hasn't been written down yet.
    
- **Feature Depth vs. Logical Depth**: The layers are great at extracting high-level abstract features (e.g., "this sentence is sarcastic" or "this is a coding problem"). But they cannot perform sequential reasoning—like solving an equation—within that single pass because the answer to "Step 1" needs to be physically present in the input for the model to "see" it and work on "Step 2."
    
- **The Autoregressive Constraint**: Transformers are autoregressive. They can only perform **one unit of sequential logic per token**. To perform 10 steps of logic, the model must generate at least 10 tokens. This is why Chain-of-Thought (CoT) is so effective; it essentially "borrows" the token generation process to give the layers new, intermediate data to chew on.
---
_See relevant notes on architectures:_
- [[BERT (Bidirectional Encoder Representations from Transformers)]]
- [[GPT (Generative Pre-trained Transformer)]]