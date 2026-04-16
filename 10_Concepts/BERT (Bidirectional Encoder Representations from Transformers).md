**BERT (Bidirectional Encoder Representations from Transformers)** is a landmark model in NLP that shifted the field from unidirectional processing to bidirectional context. Unlike the original Transformer, which has an encoder and a decoder, BERT is an **Encoder-only** architecture.

---

### The Core Innovation: Masked Language Modeling (MLM)

Traditional language models (like GPT) are unidirectional; they read left-to-right to predict the next token. BERT's "secret sauce" is its ability to look at both the left and right context simultaneously.

- **Learning by Masking:** During pre-training, BERT masks a random 15% of the input tokens (replaces them with a `[MASK]` token).
    
- **The Task:** The model must predict what the masked word is based _only_ on the surrounding context.
    
- **Next Sentence Prediction (NSP):** BERT also learns to determine if Sentence B logically follows Sentence A, helping it understand relationships between sentences.

---

### Architecture and Complexity

BERT is built by stacking Transformer Encoder blocks.

- **BERT Base:** 12 layers ($L$), 768 hidden units ($H$), 12 attention heads ($A$). Total: **110 Million parameters**.
    
- **BERT Large:** 24 layers ($L$), 1024 hidden units ($H$), 16 attention heads ($A$). Total: **340 Million parameters**.
    
- **Complexity:** Like all standard Transformers, BERT has a **Quadratic Complexity** $O(n^2)$ relative to the sequence length $n$.

---

### Special Tokens

BERT uses a specific set of tokens to manage input structure:

- `[CLS]`: The "Classification" token. It is always the first token. The final hidden state of this token is used as the aggregate representation for classification tasks.
    
- `[SEP]`: A "Separator" token used to denote the end of a sentence or to separate two sentences in a pair.
    
- `[MASK]`: Used only during pre-training for the MLM task.

---

### Transfer Learning: Pre-training vs. Fine-tuning

BERT popularized the "Pre-train once, fine-tune many" strategy.

1. **Pre-training:** The model is trained on a massive, unlabeled corpus (like Wikipedia and BookCorpus) using MLM and NSP. This is computationally expensive and takes weeks.
    
2. **Fine-tuning:** You take the pre-trained BERT and add a single, small output layer for your specific task (e.g., sentiment analysis). You then train the whole model on your much smaller, labeled dataset. This is fast and can be done in hours.

---

### Text Classification Use Case

BERT is the "gold standard" for sequence classification. To classify text:

1. Pass the sentence into BERT.
    
2. Take the output vector of the `[CLS]` token.
    
3. Feed that vector into a simple Linear layer + Softmax.
    
4. The model outputs the probability for each class (e.g., "Spam" vs. "Not Spam").

---

### Variants and Specialties

- **RoBERTa:** A "Robustly Optimized" version. It removes the NSP task, uses more data, and uses dynamic masking (masking different tokens in different epochs).
    
- **DistilBERT:** A smaller, faster, cheaper version. It uses **Knowledge Distillation** to reach 97% of BERT’s performance with 40% fewer parameters.
    
- **ALBERT:** "A Lite BERT." It reduces parameters by sharing weights across layers and factorizing the embedding matrix.
    
- **LegalBERT / BioBERT:** Domain-specific versions pre-trained on legal or medical texts.

---

### Pros, Cons, and Bottlenecks

|**Pros**|**Cons**|
|---|---|
|**Bidirectional Context:** Understands "bank" in "river bank" vs. "bank account" much better than unidirectional models.|**Heavy Computation:** BERT Large requires significant GPU memory.|
|**State-of-the-Art:** Highly effective for NLU (Natural Language Understanding) tasks like NER, QA, and Classification.|**Pre-train/Fine-tune Mismatch:** The `[MASK]` token appears during training but never during real-world inference.|

**The Bottleneck:** BERT has a **Hard Limit of 512 tokens**. Because of the $O(n^2)$ attention matrix, processing sequences longer than 512 is extremely memory-intensive.

---
_See related notes:_
- [[BERT vs GPT]]: comparison of the models
- [[Applications of BERT]]: use cases and practical application