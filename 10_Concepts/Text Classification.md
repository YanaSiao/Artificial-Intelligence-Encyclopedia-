## Definition

Text classification is the process of training a model to automatically assign documents into predefined categories. It is built using **Supervised Machine Learning**, where a computer learns from "Experience" (labeled data) to improve its "Performance" at a "Task".

## Applied Use Cases

Text classification is an extremely common task with diverse applications:

- **Security:** Spam and phishing detection (e.g., identifying malicious emails).
- **Forensics:** Authorship identification (e.g., attempting to identify anonymous authors like Satoshi Nakamoto).
- **Customer Experience:** Sentiment analysis (e.g., analyzing hotel or product reviews).
- **Safety:** Offensive content detection (e.g., flagging posts that incite violence).
- **Search & UX:** Identifying web search query intent or personalizing news feeds.
- **Law Enforcement:** Identifying criminal behavior online (e.g., fraud or grooming).
- **Operations:** Routing communications (e.g., sending emails to the correct company department).
- **Virtual Assistants:** Task identification in spoken interfaces (e.g., "Alexa, tell me a joke").

## Types of Classification Problems

The structure of the output determines the type of problem:

- **Binary Classification:** The output is one of two options (e.g., Spam vs. Not-Spam, Positive vs. Negative).
    
- **Multi-class Classification:** The output is exactly one category from a set of multiple options (e.g., routing an email to _either_ Repairs, Sales, or Support).
    
- **Multi-label Classification:** The output is a _set_ of categories; a single document can belong to multiple classes (e.g., a news article tagged as both "Politics" and "Sport").
    
- **Ordinal Regression:** The output is an ordered category (e.g., 1-star to 5-star ratings)

### [[Linear Classifiers]]

These models find a flat "hyperplane" to separate classes in a high-dimensional space. They are often preferred for text because high-dimensional data is often linearly separable.

- **Naïve Bayes:** Fast and interpretable; assumes features are independent.
    
- **Logistic Regression:** Provides calibrated probability estimates; often uses L1/L2 regularization to prevent overfitting.
    
- **Support Vector Machine (SVM):** Maximizes the "buffer" (margin) between classes; very robust for high-dimensional text data.

### [[Non-linear Classifiers]]

These find curved boundaries and are more complex.

- **Decision Trees & Ensembles:** e.g., XGBoost. Popular for many ML tasks but less common for raw text.
    
- **Neural Networks:** Basic NNs are not very effective for text, but **RNNs** and **Transformers** are the current state-of-the-art.