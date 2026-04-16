While BERT excels at understanding and labeling, GPT is designed to be a **Universal Learner**—a model that can perform nearly any NLP task by treating it as a text-to-text generation problem.

## Text Generation (The Core Task)

GPT is fundamentally an **autoregressive** model. It generates text by predicting one token at a time, where each predicted token becomes part of the input for the next prediction.

- **How it works:** The model takes a sequence (a "prompt"), processes it through the decoder stack using **causal masking** (ensuring it doesn't "see" the future), and outputs a probability distribution over the vocabulary for the next word.
    
- **Sampling Strategies:** Since the model provides probabilities, we don't always pick the most likely word (which can lead to repetitive text). Strategies like **Top-p (Nucleus) sampling** or **Top-k sampling** are used to inject creativity and variety.

---

## Fine-tuning with Special Tokens

Though GPT is often used "out-of-the-box," it can be fine-tuned for high-precision tasks by using task-specific formatting.

- **Special Tokens/Delimiters:** To help the model distinguish between different parts of an input (like a premise and a hypothesis), we insert delimiters (e.g., `$`, `|`, or `->`).
    
- **The Architecture Change:** During fine-tuning, we take the hidden state of the **last token** in the sequence and feed it into a task-specific linear layer (the "Head").
    
- **Why this works:** The final token's vector in GPT has "seen" all previous tokens due to the causal attention mechanism, making it a summary of the entire context.

---

## The Concept of "Universal Learners"

One of the most significant discoveries in the GPT era is that a model trained purely on a massive web-scale corpus (like GPT-2 or GPT-3) becomes a **Universal Learner**.

- **The Definition:** A model that can perform a wide range of tasks (translation, summarization, etc.) without having been explicitly trained on them or having its weights updated for them.
    
- **Why it is true:** The internet contains millions of examples of people performing tasks—e.g., "Translate 'apple' to French: pomme." By learning to predict the next token on the whole internet, the model inadvertently learns the _concept_ of translation, coding, and reasoning.

---

## Use Without Fine-tuning: Zero-Shot and Few-Shot

GPT pioneered the move away from traditional transfer learning (which requires labels and training) toward **In-Context Learning (ICL)**.

- **[[Zero-shot Learning (ZSL)]]:** The model is given a natural language description of the task and must perform it immediately.
    - _Example:_ "Summarize this article: 'Text'"
    
- **[[Few-shot Learning (FSL)]]:** The model is given a few examples of the task within the prompt itself.
    - _Example:_ "English: Apple, French: Pomme. English: Dog, French: Chien. English: Cat, French: 'Model completes'"
    
- **Why it works:** During pre-training, the model has already "seen" many examples of few-shot learning patterns in its training data.

---

## Major Application Domains

When used without fine-tuning, GPT typically tackles these tasks:

- **Translation:** Even if not specifically trained on a parallel corpus (like English-French pairs), it can translate effectively because fragments of various languages exist in the massive pre-training data.
    
- **Question Answering (QA):** The model "retrieves" facts from its parameters to answer questions.
    
- **Reading Comprehension:** The model is given a paragraph and asked a question about it; its attention mechanism allows it to pinpoint the answer within the text.
    
- **Summarization:** The model identifies the most "attended to" parts of a long sequence and generates a condensed version.

---

## Evaluation and Comparison

|Feature|GPT Fine-tuning|GPT Zero/Few-Shot|
|---|---|---|
|**Data Requirement**|Requires labeled data for the specific task.|Requires **zero** labeled data.|
|**Performance**|Usually higher accuracy on niche/specific domains.|High flexibility; handles tasks it wasn't built for.|
|**Complexity**|Requires retraining weights (expensive).|Only requires a well-written prompt (cheap).|
|**State-of-the-Art**|Best for production-grade classification.|Best for creative and general-purpose assistants.|
