**Zero-Shot Learning** is a machine learning paradigm where a model is trained to recognize or classify objects/concepts that it has **never seen** during the training phase. It is a subfield of **Transfer Learning** that aims to mimic human-like "reasoning by analogy."

---

### The Core Logic

ZSL relies on the existence of a **Shared Semantic Space** that connects known classes to unknown ones. Instead of learning a direct mapping from "Pixels/Tokens → Label," the model learns a mapping from **"Pixels/Tokens → Semantic Attributes."**

- **Seen Classes (S):** Classes used during training with full labeled data (e.g., "Horse").
- **Unseen Classes (U):** Classes encountered only at test time (e.g., "Zebra").
- **Auxiliary Information (A):** The "bridge" data (e.g., textual descriptions, word embeddings, or attribute lists like "has stripes," "is quadruped").

---

### The Step-by-Step Pipeline

1. **Attribute Definition:** Define a semantic space where all classes (seen and unseen) can be represented (e.g., Word2Vec embeddings or a manually curated list of traits).
    
2. **Projection Training:** Train the model on the **Seen Classes** to map raw inputs (images/text) into this semantic space.
    
3. **Semantic Embedding:** Represent the **Unseen Classes** in the same semantic space using their descriptions/attributes.
    
4. **Inference (The "Zero-Shot" Step):** 
	* A new input is projected into the semantic space.
    - The model calculates the **similarity** (e.g., Cosine Similarity) between the input's projection and the embeddings of the unseen classes.
    - The nearest neighbor in the semantic space is chosen as the predicted label.

---

### When is it Used?

- **Data Scarcity:** When it is impossible or too expensive to collect labeled data for every possible category (e.g., identifying rare medical conditions or obscure wildlife).
    
- **Dynamic Environments:** In systems where new categories emerge daily (e.g., e-commerce product categorization or identifying new internet slang).
    
- **Large-Scale Classification:** When the number of possible classes is in the thousands or millions, making traditional supervised learning unfeasible.
    
- **Cross-Modal Tasks:** Connecting different types of data, such as finding an image based on a textual description (CLIP models).

---

### When is it NOT Suitable?

- **Fine-Grained Distinction:** When the differences between classes are too subtle for general semantic descriptions (e.g., distinguishing between two very similar subspecies of birds).
    
- **Safety-Critical Systems:** Where a 90% accuracy is not enough (e.g., autonomous vehicle obstacle detection), as ZSL is inherently less reliable than supervised learning.
    
- **Lack of Auxiliary Data:** If there is no high-quality semantic information to link the new class to known concepts, ZSL will fail.

---

### Pros & Cons

|**Pros**|**Cons**|
|---|---|
|**Scalability:** Add new classes without retraining the entire model.|**Lower Accuracy:** Generally performs worse than supervised models on the same classes.|
|**Data Efficiency:** Eliminates the need for manual labeling of every new class.|**Semantic Gap:** Attributes might not perfectly capture the visual/textual reality.|
|**Human-Like Generalization:** Can "guess" what an object is based on its description.|**Bias Toward Seen Classes:** The model often tries to force new inputs into familiar categories.|


---

### The Major Bottlenecks

- **The Hubness Problem:** In high-dimensional spaces, certain points ("hubs") tend to become the "nearest neighbor" for almost everything, leading to many false positives for a few specific classes.
    
- **Domain Shift:** The semantic attributes for a "seen" class (e.g., "fur" on a dog) might look visually different from the same attribute on an "unseen" class (e.g., "fur" on a bear), causing the projection to miss.
    
- **The Semantic Bottleneck:** The quality of the ZSL model is strictly capped by the quality of the auxiliary information. If the word embeddings are noisy, the zero-shot performance will be poor.

---

### Other Key Characteristics

- **Generalized Zero-Shot Learning (GZSL):** A more realistic (and harder) evaluation where the test set contains **both** seen and unseen classes. Standard ZSL assumes the test sample _must_ be an unseen class.
    
- **Transductive ZSL:** A variant where the model is allowed to see the _unlabeled_ data of unseen classes during training to adjust its semantic projections.
    
- **Knowledge Transfer:** ZSL is often measured by how well "knowledge" (the ability to recognize features like "wings" or "metal") transfers across domain boundaries.

---
_See relevant notes:_
- [[Few-shot Learning (FSL)]] 
- [[FSL vs ZSL]]: comparison of two approaches