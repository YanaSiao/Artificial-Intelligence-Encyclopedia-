## 1. The Pioneers: LeNet-5 (1998)

The first functional CNN, designed by Yann LeCun for handwritten digit recognition (MNIST).
- **Key Idea**: Introduced the sequence of **Conv -> Pool -> Conv -> Pool -> Dense**.
- **Limitation**: Only 7 layers deep; restricted by the computing power of the 90s.
    ![[Pasted image 20260219125033.png]]

## 2. The Breakthrough: AlexNet (2012)

The winner of the ImageNet challenge that triggered the Deep Learning revolution.

- **Key Innovations**:
    - Used **ReLU** instead of Tanh (faster training).
    - Used **Dropout** to prevent overfitting.
    - Trained on **GPUs** to handle its 60 million parameters.
- **Architecture**: 5 Convolutional layers followed by 3 Dense layers.
![[Pasted image 20260219192424.png]]
## 3. Going Deeper: VGGNet (2014)

VGG proved that "Depth is Key."

- **Design Philosophy**: Used a very simple, uniform architecture. It exclusively used **$3 \times 3$ filters** with stride 1 and **$2 \times 2$ Max Pooling** with stride 2.
    
- **Pros**: Very easy to implement and use for transfer learning.
    
- **Cons**: Extremely heavy (VGG-16 has ~138M parameters), mostly due to the massive first Dense layer after flattening.
    

## 4. The Inception Era: GoogLeNet (2014)

Introduced the "Inception Module" to solve the problem of choosing the right kernel size.

- **Inception Module**: Instead of choosing a $3 \times 3$ or $5 \times 5$ kernel, it performs **all of them in parallel** and concatenates the results.
    
- **$1 \times 1$ Convolutions**: Used as "bottleneck" layers to reduce the number of channels before expensive $3 \times 3$ or $5 \times 5$ operations, keeping the parameter count low.
    ![[Pasted image 20260219193809.png]]
To reduce the computational load of the network, the number of input channels of each conv layer is reduced thanks to **1x1 convolution layers** ("bottleneck") before the 3x3 and 5x5 convolutions.

1. Blocks made of multiple connections instead of having a single tread.
2. 1x1 convolutions: a (general) practical tool to introduce bottlenecks to reduce the number of operations and parameters of the network.
3. Additional losses: you might want to train your network on additional tasks just for improving training convergence.

![[Pasted image 20260219193918.png]]

## 5. Solving the Depth Limit: ResNet (2015)

Before ResNet, adding more layers eventually _decreased_ accuracy because gradients vanished.

- **Residual Learning**: Introduced **Skip Connections** (Shortcuts) that allow the gradient to flow through the network without being multiplied by weights.
    
- **The Logic**: It is easier for a layer to learn the "residual" (the difference) than to learn the entire transformation from scratch.
    
- **Impact**: Allowed for networks with hundreds or thousands of layers (e.g., ResNet-50, ResNet-101).
    ![[Pasted image 20260219194418.png]]

## 6. Dense Connectivity: DenseNet (2017)

Takes ResNet a step further by connecting **every layer to every other layer** within a block.

- **Feature Re-use**: Each layer receives the feature maps of all preceding layers as input.
    
- **Efficiency**: Requires fewer parameters than ResNet because it doesn't need to re-learn redundant features.
    ![[Pasted image 20260219194847.png]]

## 7. Scaling Smartly: EfficientNet (2019)

Instead of just making networks deeper, EfficientNet uses **Compound Scaling**.

- **Compound Scaling**: Balances three dimensions: **Depth** (number of layers), **Width** (number of channels), and **Resolution** (input image size).
    
- **Result**: Achieve state-of-the-art accuracy with 10x fewer parameters than previous models.
    

---

### Summary Table: Architecture Comparison

|**Architecture**|**Main Contribution**|**Key Layer/Feature**|
|---|---|---|
|**LeNet-5**|First CNN|Conv-Pool sequence|
|**AlexNet**|Deep Learning era|ReLU & Dropout|
|**VGG**|Simplicity & Depth|Uniform $3 \times 3$ kernels|
|**GoogLeNet**|Computational efficiency|Inception Module / $1 \times 1$ Conv|
|**ResNet**|Solving Vanishing Gradient|Skip Connections|
|**EfficientNet**|Systematic Scaling|Compound Scaling|
![[Pasted image 20260219194655.png]]
