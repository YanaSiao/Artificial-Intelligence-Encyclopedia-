## The Confusion Matrix

The foundation of all classification metrics. For a binary problem:
- **TP (True Positive):** Correctly predicted positive.
- **TN (True Negative):** Correctly predicted negative.
- **FP (False Positive):** Type I Error (False Alarm).
- **FN (False Negative):** Type II Error (Miss).

## Threshold-Independent Metrics: ROC and AUC

In many models (Logistic Regression, SVMs), the output is a **probability** or a **score**, not a class. We usually use a threshold of 0.5, but this can be changed.

### ROC Curve (Receiver Operating Characteristic)

- A plot of the **True Positive Rate (Recall)** vs. the **False Positive Rate ($FP / (FP+TN)$)** at various threshold settings.
- It shows the trade-off between sensitivity and specificity.

### AUC (Area Under the Curve)

- **Definition:** The probability that the model will rank a randomly chosen positive instance higher than a randomly chosen negative one.
    
- **Interpretation:** 
	* **1.0:** Perfect classifier.
    - **0.5:** Random guessing (a diagonal line).
    - **< 0.5:** Worse than random (indicates a logic error in the model).


---

## Multi-class Evaluation

When you have more than two classes (e.g., Topic classification: Sports, Politics, Tech), you must aggregate your metrics.

### A. Macro-Averaging

- **Method:** Calculate the metric (e.g., F1) for each class independently and then take the unweighted average.
- **Best for:** When you want to treat all classes as equally important, regardless of how many samples they have. It punishes models that perform poorly on minority classes.

### B. Micro-Averaging

- **Method:** Aggregate the TPs, FPs, and FNs from all classes first, then calculate the metric.
- **Best for:** When you care about overall accuracy across all instances. In micro-averaging, $Precision = Recall = Accuracy$.

### C. Weighted-Averaging

- **Method:** Similar to Macro, but each class's score is weighted by its **support** (the number of true instances for that class)ю

---

## Ranking Metrics

In Recommender Systems and Search, we don't just care if an item is "relevant"; we care **where** it appears in the list.

- **MRR (Mean Reciprocal Rank):** $1/rank$ of the first relevant item.
- **NDCG (Normalized Discounted Cumulative Gain):** Rewards putting highly relevant items at the very top of the list.
- **Hit Rate @ K:** Was the correct item in the top K results?

---

## Summary of Formulas

|**Metric**|**Formula**|**Goal**|
|---|---|---|
|**Precision**|$TP / (TP + FP)$|Minimize False Positives (Avoid false alarms)|
|**Recall**|$TP / (TP + FN)$|Minimize False Negatives (Don't miss anything)|
|**F1-Score**|$2 \cdot \frac{P \cdot R}{P + R}$|Balance P and R (Harmonic Mean)|
|**FPR**|$FP / (FP + TN)$|Probability of a false alarm|