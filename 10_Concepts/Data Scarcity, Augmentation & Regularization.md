## Data Augmentation: The "Why" and "How"

**Why:** It artificially increases the size of the training set by creating modified versions of existing images. It forces the model to be invariant to irrelevant transformations (e.g., a cat is still a cat if it's flipped).

**How:** It is performed **on-the-fly** during training. The original image is loaded, a random transformation is applied in RAM, and the augmented image is fed to the GPU. The original image is never modified on disk.

### Common Techniques
#### 1. Geometric Transformations

These modify the spatial arrangement of pixels. They teach the model that the **identity** of an object does not change even if its position, size, or orientation does.

##### A. Shifts, Scaling, and Rotation

- **Shifts (Translation)**: Moving the image along the X or Y axis. This enforces **Translation Invariance**—the model learns to find the "cat" whether it is in the top-left or bottom-right corner.
- **Rotation**: Rotating the image by a random degree.
    - _Note_: Use "reflection" or "nearest" padding to fill the empty corners created by the rotation.

- **Scaling**: Zooming in or out. This helps the model recognize objects at different distances from the camera.

##### B. Shear and Affine/Perspective Distortions

- **Shear**: Tilting the image along an axis (making a square look like a parallelogram). This simulates viewing an object from a slanted angle.
- **Affine Transformations**: A combination of translation, rotation, scaling, and shear that preserves parallel lines.
- **Perspective Distortions**: Simulates the way 3D objects look from different camera viewpoints. Unlike Affine, parallel lines may converge.

##### C. Flip

- **Horizontal Flip**: Mirroring the image left-to-right. Extremely common in almost all tasks.
- **Vertical Flip**: Mirroring top-to-bottom.
    - _Caution_: Use only when the orientation doesn't matter (e.g., satellite imagery, medical cells, or top-down views).

---

#### 2. Photometric Transformations

These modify the pixel values (color/intensity) without changing the spatial coordinates. They teach the model to ignore lighting conditions and sensor noise.

##### A. Intensity and Contrast

- **Modifying Average Intensity (Brightness)**: Adding or subtracting a constant value from all pixels. This simulates different times of day or exposure settings.
- **Modifying Contrast**: Scaling the range of pixel values. This helps the model focus on edges and shapes even when the image is "washed out" or very dark.

##### B. Noise and Superimposition

- **Adding Noise**: Injecting random Gaussian or "Salt and Pepper" noise. This acts as a regularizer, forcing the FEN to learn robust, high-level features rather than relying on individual pixel values.
- **Superimposing Other Images**: Briefly overlapping textures or other images (similar to the logic of Mixup) to prevent the model from becoming too confident in specific background patterns.


---

## 2. Advanced Augmentation: Mixup

**Mixup** is a powerful technique where you create a new training sample by taking a weighted average of two different images and their labels.

- **Formula**: $I_{new} = \lambda I_A + (1-\lambda) I_B$ and $Label_{new} = \lambda L_A + (1-\lambda) L_B$
    
- **Why it works**: It encourages the model to behave linearly in between classes, leading to smoother decision boundaries and better calibration.
    ![[Pasted image 20260219135628.png]]

---

## 3. Dealing with Class Imbalance

When one class (e.g., "Background") has 1000 images and another (e.g., "Defect") has only 10, the model will simply learn to predict "Background" every time.

**Strategies to prevent Overfitting while balancing:**

1. **Oversampling**: Repeat images from the minority class.
    - _Risk_: High risk of overfitting to those 10 specific images. 
    - *Solution*: Apply heavy augmentation to the oversampled images.
        
2. **Weighted Loss**: Assign a higher "penalty" in the Loss Function when the model misclassifies the minority class.
    
3. **Undersampling**: Remove images from the majority class.
    - _Risk_: You throw away potentially useful data.


---

## 4. Test Time Augmentation (TTA)

**TTA** is augmentation applied during **Inference** (prediction), not training.

- **How it works**:
    1. Take the test image and create $N$ versions of it (e.g., original, flipped, rotated).
    2. Run the model on all $N$ versions.
    3. **Average** the predictions (probabilities) for the final result.
    
- **Is it always okay?**:
    
    - **Pros**: Usually gives a 1-2% boost in accuracy; provides a more "stable" prediction.
    - **Cons**: Increases inference time by $N$ times (slower). It is **not** okay for real-time applications (like self-driving cars) where latency is critical.
    

---

## 5. Regularization Beyond Data

If you cannot get more data, you must "constrain" the model:

- **Dropout**: Randomly kill neurons to prevent "co-adaptation."
    
- **Weight Decay (L2)**: Penalizes large weights to keep the model "simple."
    
- **Early Stopping**: Stop training as soon as the Validation Loss starts to rise.
More detailed: [[Best Practices. Overfitting and Regularization]]

## 6. Transfer Learning & Fine-Tuning
### Why use Transfer Learning?

- **Data Scarcity**: You don't have the millions of labeled images required to train a deep FEN from scratch.
    
- **Computational Cost**: Training a state-of-the-art model (like ResNet or EfficientNet) takes days/weeks on expensive GPUs.
    
- **Feature Re-use**: The early layers of a CNN learn "generic" features (edges, textures, color blobs) that are useful for _any_ vision task.

### The Anatomy of TL: Frozen vs. Trainable

In Transfer Learning, we split the pre-trained model into two parts:

1. **The Backbone (FEN)**: Usually a model pre-trained on a massive dataset like ImageNet.
    
2. **The Head (Classifier)**: A new set of Dense layers specifically designed for _your_ number of classes.

#### Feature Extraction (Freezing)

- **Action**: You "freeze" the weights of the FEN.
    
- **Logic**: During training, backpropagation only updates the weights of the new Head. The FEN acts as a fixed "feature extractor."
    
- **Why**: This preserves the "good features" learned on ImageNet and prevents them from being destroyed by large gradients during the early stages of training.

### Fine-Tuning: The "Refining" Phase

Once the Head is somewhat trained, you can perform **Fine-Tuning**.

- **Action**: Unfreeze the top layers of the FEN (the ones closest to the head) and continue training the whole model.
    
- **Best Practice**: Use a **very low Learning Rate** (e.g., 10x smaller than usual).
    
- **Goal**: To slightly nudge the specialized features of the FEN to adapt specifically to your unique images without losing the original knowledge.

### Best Practices: The Decision Matrix

How you apply TL depends on two factors: the **Size** of your dataset and the **Similarity** to the source dataset (e.g., ImageNet).

|**Situation**|**Strategy**|**Reasoning**|
|---|---|---|
|**Small Data + Similar Task**|**Freeze FEN**, train only Head.|Low risk of overfitting; features are already relevant.|
|**Large Data + Similar Task**|**Fine-tune** the whole model.|You have enough data to refine everything without overfitting.|
|**Small Data + Different Task**|**Freeze early layers**, train Head + late FEN.|Early features (edges) are still useful, but late features (shapes) are too specific.|
|**Large Data + Different Task**|**Train from scratch** or heavy Fine-tuning.|Pre-training provides a better initialization than random noise.|

---

### Summary of Best Practices

- **Replace the Output Layer**: The original model likely had 1000 classes (ImageNet). You must replace this with a layer matching _your_ class count.
    
- **Match Pre-processing**: You **must** use the same scaling and normalization (e.g., subtracting the ImageNet mean) used when the original model was trained.
    
- **Lower Learning Rate**: Always use a smaller learning rate for fine-tuning to prevent "catastrophic forgetting."