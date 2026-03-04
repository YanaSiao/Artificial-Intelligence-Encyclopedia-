## Why do we need it?

Algorithms (both Linear and Neural) cannot "read" strings. They require numeric input. Feature extraction is the process of transforming raw text into a **feature vector** $\mathbf{x} \in \mathbb{R}^n$ that represents the characteristics of the text.

## Types of Features

### A. Surface & Orthographic Features

These are often extracted using **Regular Expressions (RegEx)**.

- **Capitalization:** Does the word start with an uppercase letter? (Crucial for Named Entity Recognition).
    
- **Punctuation Patterns:** Presence of "!" or "?" (Useful for Sentiment Analysis).
    
- **Word Length:** Average length of words in a document.
    
- **Digit Count:** Useful for identifying spam or financial reports.

### B. Structural Features (N-grams)

To recover some of the context lost by the **Bag-of-Words** model, we use N-grams.

- **Unigrams:** Single words ("not", "good").
    
- **Bigrams:** Pairs of consecutive words ("not good").
    
- **Trigrams:** Triplets ("not very good").
    
- **Impact:** Increasing $N$ captures more context but leads to **Feature Explosion** (the dimensionality of your vector space grows exponentially).

### C. Linguistic Features

These require more sophisticated tools like `spaCy` or `NLTK`.

- **Part-of-Speech (POS) Tags:** Is the word a Noun, Verb, or Adjective?
    
- **Lemma:** The root form of the word (e.g., "running" $\rightarrow$ "run").
    
- **Dependency Relations:** How words relate to each other grammatically.
