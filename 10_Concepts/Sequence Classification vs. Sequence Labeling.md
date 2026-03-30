### Sequence Classification (Many-to-One)

The goal is to assign a single category to an entire sequence of words.

- **Input:** A sequence $X = (x_1, x_2, \dots, x_n)$.
    
- **Output:** A single label $y$.
    
- **Examples:** 
	* **Sentiment Analysis:** "I loved this movie" $\rightarrow$ Positive.
    - **Topic Classification:** "The team won the match" $\rightarrow$ Sports.

### Sequence Labeling (Many-to-Many)

The goal is to assign a label to _each_ element (token) in the input sequence. The labels often depend on the context of surrounding words.

- **Input:** A sequence $X = (x_1, x_2, \dots, x_n)$.
    
- **Output:** A sequence of labels $Y = (y_1, y_2, \dots, y_n)$.
    
- **Examples:**
    - **Part-of-Speech (POS) Tagging:** "Alice (Noun) ate (Verb) cake (Noun)."
    - **Named Entity Recognition (NER):** "Apple (Org) is in Cupertino (Loc)."
