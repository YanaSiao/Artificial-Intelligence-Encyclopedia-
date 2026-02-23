Once the FEN has extracted high-level features into a 3D volume (e.g., $7 \times 7 \times 512$), we need to convert that volume into a single class prediction.

## Bridging the Gap: Flatten vs. GAP

We must transform the 3D feature map into a 1D vector before feeding it to the Dense layers.
### Flattening

- **How it works**: Simply unrolls all values in the volume into one long vector.
- **Impact**: If you have a $7 \times 7 \times 512$ volume, flattening creates a vector of $25,088$ elements.
- **Downside**: It creates a massive number of parameters in the following Dense layer, leading to high memory usage and a high risk of **Overfitting**.

### Global Average Pooling (GAP)

- **How it works**: It takes the average of _all_ pixels in each feature map. A $7 \times 7 \times 512$ volume becomes a $1 \times 1 \times 512$ vector (length 512).
- **Impact**: Drastically reduces parameters (by a factor of $7 \times 7 = 49$ in this case).
- **Benefit**: It makes the network more robust to spatial shifts and allows for **Weakly Supervised Localization** (seeing which feature maps "fire" for a specific class).
    

## Dense Layers (Fully Connected)

This is where the classification logic happens.

- **Neurons**: Each neuron in a Dense layer looks at _every_ feature provided by the FEN/GAP.
- **Bigger vs. Smaller**:
    
    - **More Neurons**: Increases "capacity" to learn complex relationships but increases the risk of overfitting and training time.
    - **Fewer Neurons**: Acts as a bottleneck, forcing the network to keep only the most vital information.

## The Output Layer: Softmax

The final layer always has a number of neurons equal to the number of classes.

- **Softmax Function**: Turns the raw "logits" (scores) into probabilities that sum to 1.0 (100%).
- **Interpretation**: If the Softmax output for "Cat" is 0.85, the model is 85% confident the image is a cat.

![[Pasted image 20260219125525.png]]

## Multilabel vs. Multitask Classification

In standard classification (Multiclass), we assume one image belongs to exactly one class. These two variants change that assumption:

|**Feature**|**Multilabel Classification**|**Multitask Classification**|
|---|---|---|
|**Goal**|One image can have **multiple** labels (e.g., "Cat" AND "Indoor").|One image is used for **different** types of tasks (e.g., "What is it?" AND "Where is it?").|
|**Output Layer**|One Dense layer with $N$ neurons.|Multiple "Heads" (separate Dense layers for each task).|
|**Activation**|**Sigmoid** for each output neuron (independent probabilities).|Task-dependent (Softmax for class, Linear for coordinates).|
|**Loss Function**|**Binary Cross Entropy** summed over all classes.|A weighted sum of losses (e.g., $L_{total} = L_{class} + \lambda L_{box}$).|