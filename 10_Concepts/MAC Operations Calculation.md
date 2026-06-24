
## Expanding to Full Layer Complexity (Total MAC Counts)

To find the total computational cost of an entire layer, you must scale the cost of a single spatial position by the total number of elements in the output feature map and the total number of filters deployed. 

Let the output feature map spatial dimensions be $E \times F$ (Height $\times$ Width) and the number of output channels (number of distinct filters) be $K$.

---

### **1. Standard 2D Convolution (Images / Static Vision)**
In a standard 2D convolutional layer, a bank of $K$ filters moves across the spatial dimensions of the input tensor to generate $K$ output channels.

* **Input Shape:** $H \times W \times C$
* **Filter Shape:** $R \times S \times C$ (per filter, with $K$ total filters)
* **Output Shape:** $E \times F \times K$



$$\text{Total 2D Conv MACs} = E \times F \times R \times S \times C \times K$$

> **Core Intuition:**  
> * $\text{Total Outputs Generated} = E \times F \times K$
> * $\text{Cost per Output Element} = R \times S \times C$
> * Multiplying them yields the complete layer cost.

---

### **2. 3D Convolution (Video / Spatiotemporal Data)**
For video streams or 3D medical imaging (like CT scans), data possesses an extra temporal or depth dimension ($D$). The filters become 3D cubes ($T \times R \times S$) moving across time and space.

* **Input Shape:** $D \times H \times W \times C$ (where $D$ is number of frames/depth slices)
* **Filter Shape:** $T \times R \times S \times C$ (where $T$ is the temporal/depth span of the filter)
* **Output Shape:** $G \times E \times F \times K$ (where $G$ is the output temporal/depth dimension)

$$\text{Total 3D Conv MACs} = G \times E \times F \times T \times R \times S \times C \times K$$

---

### **3. 1D Convolution (Audio / Time-Series Signals)**
For 1D signals such as raw audio waveforms, ECG data, or vibration sensor readings, the spatial width dimension disappears, leaving only a single temporal length dimension ($L$).

* **Input Shape:** $L \times C$ (Length $\times$ Channels)
* **Filter Shape:** $R \times C$ (Filter Length $\times$ Channels, with $K$ total filters)
* **Output Shape:** $E \times K$ (Output Length $\times$ Output Channels)

$$\text{Total 1D Conv MACs} = E \times R \times C \times K$$

---

### **4. Depthwise Separable Convolution (MobileNet Optimized)**
MobileNet splits a standard 2D convolution into two distinct, ultra-efficient operations to bypass the heavy $C \times K$ multiplication penalty:

#### **Step A: Depthwise Convolution (Spatial Filtering)**
Each channel is filtered independently by its own dedicated spatial filter. No cross-channel accumulation occurs ($K = 1$ per channel).
* **Input Shape:** $H \times W \times C$
* **Filter Shape:** $R \times S \times 1$ (repeated across $C$ independent channels)
* **Output Shape:** $E \times F \times C$

$$\text{Depthwise MACs} = E \times F \times R \times S \times C$$

#### **Step B: Pointwise Convolution (Channel Mixing)**
A standard convolution utilizing $1 \times 1$ spatial filters maps the $C$ channels into $K$ target output channels.
* **Filter Shape:** $1 \times 1 \times C \times K$
* **Output Shape:** $E \times F \times K$

$$\text{Pointwise MACs} = E \times F \times 1 \times 1 \times C \times K$$

#### **Total Layer Cost Comparison**
$$\text{Total Depthwise Separable MACs} = (E \times F \times R \times S \times C) + (E \times F \times C \times K) = E \times F \times C \times (R \times S + K)$$

---

### **Summary Table: MAC Equations At-a-Glance**

| Layer Type                   | Input Type         | Mathematical MAC Formula                                           |
| :--------------------------- | :----------------- | :----------------------------------------------------------------- |
| **1D Convolution**           | Audio, IMU signals | $E \times R \times C \times K$                                     |
| **2D Convolution**           | Standard Images    | $E \times F \times R \times S \times C \times K$                   |
| **3D Convolution**           | Videos, CT Scams   | $G \times E \times F \times T \times R \times S \times C \times K$ |
| **Depthwise Separable**      | MobileNet Layer    | $E \times F \times C \times (R \times S + K)$                      |
| **Fully Connected (Linear)** | Flattened Vector   | $I \times O$ *(where $I = \text{Inputs}, O = \text{Outputs})$*     |


### Translating Hardware Actions into Complexity Metrics

A MAC operation modifies a hardware accumulator register via the expression $a \leftarrow a + (b \times c)$. In edge platforms, it is important to understand how isolated mathematical operations map back into execution costs:

#### The Golden Equivalence

$$\mathbf{1 \text{ MAC}} = 1 \text{ Multiplication} + 1 \text{ Addition} = \mathbf{2 \text{ FLOPs (or OPs)}}$$

- **Standalone Multiplication ($y = b \times c$):**
    
    - _Context:_ Scaling a tensor by a constant or executing channel-wise scaling layers without accumulation.
        
    - _Cost:_ Evaluates to **1 FLOP** (or 0.5 MACs). In hardware profiling tools, pure multiplications are frequently grouped directly into generic "OPs" or assigned a fractional 0.5 MAC penalty.
        
- **Standalone Addition/Sum ($y = a + b$):**
    
    - _Context:_ Element-wise additions in Residual Skip Connections (like ResNet or MobileNetV2), or adding bias vectors ($+b$).
        
    - _Cost:_ Evaluates to **1 FLOP** (or 0.5 MACs).
        
    - _Edge Warning:_ Skip connections add **zero parameters**, but they are _not free_; they carry an explicit execution penalty of exactly 1 addition operation per activation tensor element.
        
- **Non-Linear Actions / Activations (ReLU, Sigmoid):**
    
    - _Context:_ Computing $\max(0, x)$ or transcendental equations.
        
    - _Cost:_ These do not contain a product-sum structure and cannot execute inside a standard hardware MAC lane. They are measured directly as **1 OP (Operation)** per element for element-wise comparisons (ReLU) or broken down into multi-cycle approximations (Taylor series/Lookup Tables) for hardware execution of Sigmoid/Tanh.