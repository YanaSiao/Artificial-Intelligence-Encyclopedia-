GPT represents the "Generative" side of the Transformer family. Its architecture is designed to predict the next token in a sequence, making it the foundation for modern conversational AI.

## The Core Innovation: Autoregressive (Causal) Modeling

GPT is a **unidirectional** autoregressive model that processes text from left to right.

- **Causal Language Modeling (CLM):** During pre-training, GPT is given a sequence of words and must predict the single next word.
    
- **The Mathematical Goal:** To maximize the likelihood of the next token $x_t$ given the previous tokens:
    
    $$P(x_t | x_1, \dots, x_{t-1})$$
    
- **No Masking Required:** Because it only looks at the past, it doesn't need to "hide" words with a `[MASK]` token; the "future" is naturally hidden by the architecture's causal mask.

---

## Architecture and Complexity

GPT is built by stacking **Transformer Decoder** blocks. It removes the "Encoder-Decoder Attention" layer found in the original Transformer because there is no Encoder to look at.

- **GPT-2:** Ranged from **117 Million** to **1.5 Billion parameters**.
    
- **GPT-3:** A massive jump to **175 Billion parameters** (96 layers, 12,288-dimensional embeddings).
    
- **Complexity:** Standard $O(n^2)$ quadratic complexity relative to sequence length.

---

## Tokens and Input

GPT uses tokenization :

- **Byte-Pair Encoding (BPE):** GPT uses BPE to break words into sub-word units, allowing it to handle rare words and balance vocabulary size.
    
- **`<|endoftext|>`:** A special token used to signify the end of a document or a transition between samples in a training batch.
    
- **No `[CLS]` or `[SEP]`:** Since GPT is not designed for sentence-pair tasks by default, it doesn't require these specific BERT tokens, though they can be added during fine-tuning.

---

## Transfer Learning: In-Context Learning (ICL)

While BERT popularized "Fine-tuning," GPT (especially GPT-3 and 4) shifted the focus toward **In-Context Learning**.

- **Pre-training:** Trained on massive swaths of the internet (Common Crawl, books) to learn the general structure of human language.
    
- **[[Zero-Shot Learning & Knowledge Distillation|Zero-shot]] / Few-shot:** Instead of retraining the weights (fine-tuning), you provide the model with a few examples in the prompt, and it "learns" the task on the fly without updating its parameters.

---

## Text Generation Use Case

GPT is the master of **open-ended generation**. To generate text:

1. Provide a "Prompt" (the starting sequence).
    
2. The model predicts the next token.
    
3. That token is added back to the input, and the process repeats (Recursion).
    
4. **Decoding Strategies:** Uses techniques like **Top-k sampling**, **Nucleus (Top-p) sampling**, or **Beam Search** to ensure the generated text is creative yet coherent.

---

## Variants and Specialties

- **GPT-1:** The proof of concept (117M parameters). Showed that generative pre-training helps downstream tasks.
    
- **GPT-2:** Demonstrated "Zero-shot" capabilities—learning tasks without specific training.
    
- **GPT-3:** Proved that scaling (more data, more parameters) leads to emergent behaviors (reasoning, coding).
    
- **InstructGPT / ChatGPT:** Fine-tuned using **RLHF ([[Reinforcement Learning from Human Feedback]])** to align the model with human instructions and safety.

---

## Pros, Cons, and Bottlenecks

|**Pros**|**Cons**|
|---|---|
|**Superior Generation:** Best-in-class for creative writing, coding, and chatting.|**Hallucinations:** Because it only predicts the "most likely" next word, it can confidently state falsehoods.|
|**Flexibility:** Can perform almost any task via "prompting" without needing new training data.|**Unidirectional:** Struggles with tasks that require looking at the "future" context (like filling in the middle of a sentence).|

**The Bottleneck:** Like BERT, GPT is limited by the **Context Window**. As the prompt grows longer, the memory required for the attention matrix grows quadratically ($O(n^2)$), making it difficult to process entire books or massive codebases at once.

---
_See related notes:_
- [[BERT vs GPT]]: comparison of the models
- [[Applications of GPT]]: use cases and practical application