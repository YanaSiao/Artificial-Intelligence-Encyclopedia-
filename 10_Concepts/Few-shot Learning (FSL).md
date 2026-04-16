**Few-Shot Learning** is a subfield of machine learning where a model is designed to adapt to a new task or category given only a very small number of training examples (typically between 1 and 5). It relies on **Meta-Learning** (learning to learn), where the model gains experience across many tasks during pre-training to become an efficient learner of new tasks.

---

### The N-Way K-Shot Notation

In FSL, tasks are almost always described using the "N-way K-shot" terminology:

- **N-Way:** The number of different classes the model must choose from (e.g., 5-way means distinguishing between 5 types of objects).
    
- **K-Shot:** The number of examples provided for each of those classes (e.g., 1-shot means only one example per class).
    
- **Support Set:** The collection of N×K examples provided to the model to learn the new task.
    
- **Query Set:** The new, unlabeled samples that the model must classify using the knowledge from the Support Set.

---

### The Meta-Learning Logic 
Unlike standard supervised learning, which minimizes error on a specific dataset, FSL uses **Episodic Training**.

- **The Episode:** Instead of batches of data, the model is trained on a series of "episodes." Each episode simulates a few-shot task by sampling a subset of classes and a few examples from a large meta-training set.
    
- **Optimization Goal:** The model is not optimized to recognize a "cat" specifically; it is optimized so that _given any Support Set_, it can correctly classify the Query Set.
    
- **GPT Context:** In Large Language Models, FSL is often referred to as **In-Context Learning**. The model uses its massive pre-trained parameters to identify the pattern in the prompt (the shots) and continues it for the query.

---

### The Step-by-Step Pipeline

- **Meta-Training:** The model is exposed to thousands of different tasks (episodes) to learn general feature extraction and comparison logic.
    
- **Task Sampling:** At test time, a new, unseen task is sampled.
    
- **Conditioning:** The model "conditions" its internal state or weight updates based on the provided **Support Set**.
    
- **Prediction:** The model processes the **Query Set** by comparing the new features to the ones it just extracted from the Support Set.
    
- **Adaptation:** In some versions (like MAML), the model performs a few steps of gradient descent on the support set; in others (like Prototypical Networks or GPT), it simply uses the support set as a reference.

---

### When is it Used?

- **Rare Classes:** When data collection is physically or ethically limited (e.g., rare diseases in medical imaging or identifying specific individual animals in wildlife conservation).
    
- **Personalization:** When an AI needs to adapt to a specific user's voice, handwriting, or preferences with minimal input.
    
- **Cold-Start Problems:** In recommendation systems where a new user or item has very little interaction history.
    
- **Rapid Deployment:** When a developer needs to spin up a classifier for a new category (e.g., a specific product line) in minutes without a large labeling campaign.

---

### When it is Not Suitable?

- **High-Stakes Precision:** If the task requires 99.9% accuracy, FSL is usually too "noisy" compared to full-scale supervised learning.
    
- **Large-Scale Stable Classes:** If you already have 100,000 labeled images for a category, there is no benefit to using FSL; standard training will result in a more robust model.
    
- **Computational Constraints at Inference:** Some FSL methods require the model to "re-learn" or process the Support Set every time a query is made, which can be slower than a static classifier.

---

### Pros & Cons

|**Aspect**|**Pros**|**Cons**|
|---|---|---|
|**Data & Training**|**Data Efficiency**: Drastically reduces the "data hunger" of deep learning.|**High Variance**: Accuracy depends heavily on the specific examples chosen for the Support Set (e.g., a blurry "1-shot" image).|
|**Model Versatility**|**Flexibility**: Allows one model to serve thousands of different tasks without explicit re-training for each.|**Overfitting**: High risk of the model focusing on irrelevant background details in the few examples rather than the actual object.|
|**Logic & Design**|**Human-Like Learning**: Mirrors how humans can see a new tool once and understand its use cases.|**Complexity**: Designing a robust meta-learning architecture is significantly harder than standard supervised training.|

---

### The Major Bottlenecks

- **The Selection Bias:** Because the sample size is so small (K=1 or 5), any noise in the support set has a massive impact on the final decision boundary.
    
- **Task Distribution Shift:** If the tasks the model sees at test time are fundamentally different from the tasks it saw during meta-training (e.g., trained on animals, tested on circuit boards), performance crashes.
    
- **The Feature Reuse vs. Adaptation Trade-off:** Determining whether the model should simply use existing features to compare inputs or actually update its weights for the new task.

---

### Other Key Characteristics

- **Metric-Based Learning:** A common FSL approach (e.g., Prototypical Networks) that calculates a "mean" vector (prototype) for each class in the Support Set and uses distance metrics to classify the query.
    
- **Optimization-Based Learning:** An approach (e.g., MAML) that trains the model to be "easy to fine-tune," so that 1 or 2 steps of gradient descent are enough to achieve high accuracy on a new task.
    
- **In-Context Learning (ICL):** Specific to LLMs, where the "shots" are provided in the text prompt, and the model uses its attention mechanism to "learn" the pattern without changing any underlying weights.

---
_See relevant notes:_
- [[Zero-shot Learning (ZSL)]] 
- [[FSL vs ZSL]]: comparison of two approaches