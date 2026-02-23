## 1. The Feature Extraction Perspective
Images cannot be directly fed to a classifier. An intermediate step is required to:
- **Extract meaningful information**: Focus on relevant patterns (edges, shapes) while discarding noise.
- **Reduce data dimension**: Transform a high-dimensional pixel grid into a compact vector of features.
- **Principle**: The better the features, the simpler and more effective the classifier becomes.
![[Pasted image 20260217222520.png]]
## 2. Hand-Crafted Features
Before the Deep Learning era, features were manually designed by engineers based on domain knowledge.
- **Traditional Process**: Image $\to$ Hand-crafted Feature Extraction $\to$ Simple Classifier (SVM, MLP).
- **Examples**:
    - **Physical Attributes**: For parcel classification, features like height, perimeter, and area.
    - **SIFT (Scale-Invariant Feature Transform)**: Identifies local interest points that are invariant to scaling and rotation.
    - **HOG (Histogram of Oriented Gradients)**: Counts occurrences of gradient orientation in localized portions of an image (commonly used for pedestrian detection).
- **The Limitation**: Hand-crafting features is a "bottleneck." Features that work for one task (detecting cars) might fail completely for another (identifying cells), requiring a total redesign.

## 3. Data-Driven (Learned) Features
Instead of designing features, we let the network **learn** them from data. This is the core philosophy of Convolutional Neural Networks (CNNs).
- **Process**: Image $\to$ **FEN (Feature Extraction Network)** $\to$ Classifier.
- **Hierarchical Learning**:
    - **Early Layers**: Learn low-level features (edges, corners).
    - **Deep Layers**: Learn high-level semantic features (eyes, wheels, textures).
- **The Advantage**: The network automatically identifies which visual cues are most important for the specific task at hand.

## 4. Latent Space & Image Retrieval
The output of the FEN (the last layer before the classifier) is called the **Latent Representation** or **Feature Vector**.
- **Latent Space**: A space where images with similar semantic content are mapped to points that are "close" to each other.
- **Image Retrieval**: By computing the distance (e.g., L1 or L2) between feature vectors in this latent space, we can retrieve images that are visually or semantically similar, which is much more effective than pixel-wise comparison.

## 5. Summary: Hand-Crafted vs. Data-Driven
| Feature Type | Source | Domain Dependency | Efficiency |
| :--- | :--- | :--- | :--- |
| **Hand-Crafted** | Human Engineer | High (Task-Specific) | Hard to scale to complex visuals |
| **Data-Driven** | Neural Network | Low (Learns from data) | Highly flexible and robust |

---