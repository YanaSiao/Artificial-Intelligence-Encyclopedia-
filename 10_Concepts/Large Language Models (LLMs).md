### What is a Large Language Model (LLM)?

An LLM is a language model characterized by a massive number of parameters (typically billions) trained on an unprecedented scale of data (hundreds of billions to trillions of tokens). While a standard LM predicts the next word, an LLM demonstrates "intelligence" not through explicit programming, but as a byproduct of this massive scale.

### LLMs vs. Traditional Models

The primary difference between a "standard" model (like original BERT) and an LLM (like [[GPT-3]]/4) is not just size, but the shift from task-specific training to general-purpose capability.

|**Feature**|**Traditional Sequence Models (e.g., RNN, BERT)**|**Large Language Models (e.g., GPT-3, Llama)**|
|---|---|---|
|**Training Goal**|Fine-tuning for a specific task (NER, Sentiment).|General next-token prediction (Universal Learning).|
|**Parameters**|Millions (e.g., BERT-Base: 110M).|Billions (e.g., GPT-3: 175B).|
|**Dataset Size**|GBs of text (Wikipedia/Books).|TBs of text (Common Crawl/Web).|
|**Primary Usage**|Fine-tuning on labeled data.|Prompting (In-Context Learning).|
|**Abilities**|Predictable based on training data.|**Emergent Abilities** (Reasoning, Arithmetic).|

---

### In-Context Learning: The "Shot" Hierarchy

LLMs can perform tasks by observing examples in the prompt. This removes the need for fine-tuning in many scenarios.

**Zero-shot Learning**

The model is given only a natural language instruction of the task. It relies entirely on its pre-trained internal knowledge.

- _Example:_ "Translate the following to French: The cat is on the mat."

**One-shot Learning**

The model is given exactly one example of the task to define the expected format and style.

- _Example:_ "English: Apple, French: Pomme. English: Dog, French: [Model fills]"

**Few-shot Learning**

The model is given multiple examples (usually 5–10). This is the most robust method for complex tasks, as it helps the model identify subtle patterns in the expected output.

---

### Emergent Abilities and Scaling Laws

One of the most important concepts in LLMs is **Emergence**. These are abilities that are not present in smaller models but appear suddenly once a model reaches a certain size or computational threshold.

- **Emergent Tasks:** Logical reasoning, multi-step arithmetic, and word unscrambling often have 0% accuracy until the model hits a specific number of parameters (e.g., 10B or 100B), at which point accuracy spikes.
    
- **Scaling Laws:** Research shows that model performance follows a power law. To get a linear increase in performance, you typically need an exponential increase in data and computation.

---

### Training and Computational Bottlenecks

Training an LLM requires massive distributed GPU clusters.

- **Dataset Diversity:** Modern LLMs prioritize "quality" over "quantity," filtering web data to remove toxicity and low-quality content.
    
- **Context Window:** The quadratic complexity $O(n^2)$ remains the primary bottleneck for processing long documents.
    
- **RLHF (Reinforcement Learning from Human Feedback):** After pre-training, models undergo an alignment phase where humans rank outputs to make the model more helpful and less harmful.