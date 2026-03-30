An HMM is a **Generative** model. It assumes that there is an underlying sequence of "hidden states" (e.g., POS tags) that produces the "observed" words.

**The Core Components:**

- **States ($S$):** The hidden tags we want to predict.
    
- **Observations ($O$):** The actual words in the sentence.
    
- **Transition Probabilities ($A$):** The probability of moving from one state to another (e.g., $P(\text{Verb} | \text{Noun})$).
    
- **Emission Probabilities ($B$):** The probability of a state "emitting" a specific word (e.g., $P(\text{"ate"} | \text{Verb})$).

**The Formula:**

To find the best tag sequence, we maximize the joint probability:

$$P(O, S) = \prod_{i=1}^n P(s_i | s_{i-1}) P(o_i | s_i)$$

**Algorithms to Remember:**

- **[[Viterbi Algorithm]]:** A [[Dynamic Programming]] approach used to find the most likely sequence of hidden states (the "Decoding" problem).