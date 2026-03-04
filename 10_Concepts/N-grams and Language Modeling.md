In n-gram models, one-hot vectors can be used to represent the words in a corpus.
Each unique word in the corpus is represented by a vector with a single element set to 1, and all other elements set to 0. The goal of N-gram language models is to predict the probability of a sentence given the previous words, called “context”.

To predict the next word, we use the **Markov Assumption**: the probability of a word depends only on the previous $n-1$ words.

- **Unigram ($n=1$):** $P(w_1, w_2, w_3) = P(w_1)P(w_2)P(w_3)$
    
- **Bigram ($n=2$):** $P(w_n | w_{n-1})$

![[Pasted image 20260214183743.png]]

---
**The "Versatile Actress" Example:**
In the slides, the professor shows how context fixes a typo ("acress" $\rightarrow$ "actress" vs "acres"):
$$P(\text{actress} | \text{acress, versatile}) \propto P(\text{acress} | \text{actress}) \cdot P(\text{versatile} | \text{actress}) \cdot P(\text{actress})$$
- **The Logic:** Even if "actress" and "acres" are both valid words, $P(\text{versatile} | \text{actress})$ is much higher than $P(\text{versatile} | \text{acres})$ in a natural language corpus.

