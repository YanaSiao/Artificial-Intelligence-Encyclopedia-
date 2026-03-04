## The Core Theory

Naïve Bayes is a **Generative Classifier** based on **Bayes' Theorem**. It estimates the probability of a class $c$ given a set of features $x_1, x_2, ..., x_n$.

### Bayes' Theorem

$$P(c | x) = \frac{P(x | c)P(c)}{P(x)}$$

- **$P(c|x)$ (Posterior):** Probability of the class given the features.
- **$P(x|c)$ (Likelihood):** Probability of features given the class.
- **$P(c)$ (Prior):** General probability of the class (how common it is).
- **$P(x)$ (Evidence):** Normalization constant (usually ignored in classification since it's the same for all classes).

## The "Naïve" Assumption

The model is called "Naïve" because it assumes that **all features are conditionally independent** given the class.

- **Reality:** In text, the word "San" is highly dependent on "Francisco."
- **Assumption:** NB assumes $P(\text{"San", "Francisco"} | c) = P(\text{"San"} | c) \times P(\text{"Francisco"} | c)$.

**The Resulting Formula:**
$$C_{NB} = \text{argmax}_{c \in C} P(c) \prod_{i=1}^{n} P(x_i | c)$$
## Training & Parameters

Training a Naïve Bayes model is incredibly fast because it only requires **counting**:

1. **Prior $P(c)$:** $\frac{Count(c)}{Total\_Samples}$

2. **Likelihood $P(x_i | c)$:** $\frac{Count(x_i \text{ in } c)}{Total\_Features\_in\_c}$

### The Zero-Frequency Problem & Laplace Smoothing

If a word never appeared in the training set for a specific class, the probability becomes 0, which "wipes out" the entire product.

- **Solution:** Add a small constant $\alpha$ (usually 1) to every count.
    $$\hat{P}(x_i | c) = \frac{count(x_i, c) + \alpha}{\sum (count(w, c) + \alpha)}$$

## Loss Function

Naïve Bayes doesn't use Gradient Descent to minimize a loss function like Logistic Regression does. Instead, it maximizes the **Joint Likelihood**.

- In practice, we work with the **Log-Likelihood** to prevent **numerical underflow** (multiplying many small probabilities results in numbers too small for computers to handle).
    $$\log P(c|x) \propto \log P(c) + \sum_{i=1}^n \log P(x_i | c)$$

## Variants

|**Variant**|**Data Type**|**Use Case**|
|---|---|---|
|**Multinomial NB**|Discrete counts|Text classification (BoW).|
|**Bernoulli NB**|Binary (0/1)|Short text (did the word appear or not?).|
|**Gaussian NB**|Continuous|Physical measurements (height, weight).|

## Pros and Cons

### ✅ Pros

- **Extremely Fast:** Linear time complexity for training and prediction.
- **Small Data:** Works surprisingly well with very little training data.
- **Robust to Irrelevant Features:** If a feature has no correlation with the class, its probability distribution will be similar across all classes and won't affect the result much.

### ❌ Cons

- **The Independence Assumption:** It cannot capture relationships between words (e.g., "New York").
- **Probability Estimates:** While it is good at picking the _correct_ class, the actual probability percentages it outputs are often not very accurate (calibrated).


