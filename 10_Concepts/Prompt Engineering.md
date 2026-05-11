Prompt engineering is the process of optimizing the input (the "prompt") to guide a Large Language Model (LLM) toward a specific, high-quality output. Because LLMs are **Universal Learners**, they do not require weight updates to perform new tasks; instead, they rely on the information provided within their context window to "condition" their response.

---

### The Anatomy of a Prompt

A professional-grade prompt is typically composed of four distinct components that provide the model with a clear "frame of reference."

**System Prompt (Instruction)**: Defines the model's persona, boundaries, and overall goal. This is often "locked" in the background of chat applications.

**Context**: Background information or external data (e.g., a retrieved document or a previous conversation) that the model needs to reference.

**Examples (Shots)**: Demonstrations of the task to show the model the desired pattern or output format.

**Input Data & Cue**: The specific query or data the user wants processed, often followed by a "cue" (e.g., "Output:") to trigger the generation.

---

### Zero-Shot Learning

In zero-shot prompting, the model is given a task description without any examples. The model must rely entirely on its pre-trained internal representations to understand the request.

| **Pros**      | **Efficiency**: Uses the fewest tokens and is the fastest/cheapest method. **Unbiased**: Prevents the model from mimicking specific quirks of examples.                                       |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cons**      | **Fragility**: Highly sensitive to the specific wording of the instruction. **Formatting Risk**: The model may output correct information in an unusable format (e.g., text instead of JSON). |
| **Use Cases** | General summarization, creative writing, basic Q&A, and sentiment analysis on common topics.                                                                                                  |

---

### One-Shot Learning

One-shot prompting involves providing exactly **one** example of the task. This is the "minimum viable demonstration" needed to stabilize the model's behavior.

| **Pros**      | **Format Alignment**: Drastically improves the model's ability to follow a specific output structure (e.g., a specific CSV header).            |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cons**      | **Narrow Focus**: If the single example is an outlier or poorly written, the model will follow that flawed pattern for every subsequent query. |
| **Use Cases** | Simple data extraction, stylistic mimicry (e.g., "write in this specific tone"), and basic translation tasks.                                  |

---

### Few-Shot Learning

Few-shot prompting provides a small set of examples (typically 3–10). This is the standard for high-reliability industrial applications.

| **Pros**      | **High Robustness**: Reduces the impact of "word choice" in the instruction. **Pattern Recognition**: Excellent for teaching the model nuanced logic or niche terminology.          |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cons**      | **Token Cost**: Consumes significantly more of the context window. <br>**Bias**: If you provide 4 positive examples and 1 negative, the model will develop a "majority label bias." |
| **Use Cases** | Complex classification, code generation, medical/legal text processing, and sentiment analysis for "sarcastic" or tricky text.                                                      |

---

### Many-Shot (Multiple Shot) Learning

With the advent of massive context windows (e.g., 128k or 1M tokens), "Many-Shot" learning has emerged. This involves providing hundreds or even thousands of examples within the prompt.

| **Pros**      | **Fine-tuning Alternative**: Can achieve accuracy near that of a fine-tuned model without the engineering overhead of retraining.                                                                                                       |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cons**      | **Cost & Latency**: Extremely expensive and slow due to $O(n^2)$ complexity. **Jailbreak Risk**: "Many-shot jailbreaking" can occur where a model is "overwhelmed" into bypassing safety filters by long lists of adversarial examples. |
| **Use Cases** | Learning an entire codebase, simulating a specific person's lifelong writing style, or highly complex scientific reasoning.                                                                                                             |

---

### [[Chain-of-Thought Prompting|Chain of Thought (CoT) Prompting]]

A specialized prompting technique where the model is instructed to "think step-by-step." This forces the model to generate intermediate reasoning tokens before arriving at a final answer.

**How to do it**: Add a simple phrase like "Let's think step by step" or provide few-shot examples where the "Answer" is preceded by a "Reasoning" section.

**Why it works**: It allows the model to allocate more computation (tokens) to the logic phase rather than trying to calculate a complex answer in a single forward pass.

---

### Special Tokens & Delimiters

To prevent "Prompt Injection" (where the model confuses the input data with instructions), engineers use specific delimiters to create clear boundaries.

**Role Indicators**: `System:`, `User:`, and `Assistant:`.

**Section Delimiters**: Using `###`, `---`, or `"""` to wrap input text.

**Structural Tokens**: Using `<|endoftext|>` or specific XML tags like `<context></context>` to wrap sensitive data.

**Format Cues**: Ending a prompt with a partial string like `{ "result":` to force the model to complete a JSON object.

---

### Prompting Strategies & Templates

Successful prompt engineering often follows a "Positive Constraint" template:

1. **Persona**: "Act as a senior software architect."
    
2. **Task**: "Review the following Python code for security vulnerabilities."
    
3. **Constraints**: "Do not use external libraries. Limit your response to 3 bullet points."
    
4. **Steps**: "First, identify the vulnerability. Second, explain the risk. Third, provide a fix."
    
5. **Output Format**: "Format your response as a JSON object with the keys 'vuln', 'risk', and 'fix'."