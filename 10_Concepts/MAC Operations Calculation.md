### MAC Count for a 3x3 Filter at One Spatial Position

To calculate the exact number of Multiply-Accumulate (MAC) operations required to apply a filter to a single spatial coordinate, you must account for the **channel depth ($C$)** of the incoming feature map.

A single convolutional filter is a 3D tensor of shape $R \times S \times C$ (where $R, S$ are spatial filter dimensions, and $C$ is input channels).

- **Case A: Single-Channel Input ($C = 1$)**
    
    - The filter has exactly $3 \times 3 \times 1 = 9$ weights.
        
    - Applying this to one spatial position requires 9 scalar multiplications and 8 additions to combine the terms into a single scalar output.
        
    - Because 1 MAC incorporates 1 multiplication and 1 accumulation step, this requires exactly **9 MAC operations**.
        
- **Case B: Multi-Channel Input ($C > 1$)**
    
    - The filter has $3 \times 3 \times C = 9C$ weights.
        
    - The operation performs element-wise multiplications across all 9 spatial points across all $C$ channels simultaneously, accumulating them into a single value.
        
    - **Formula for one spatial position:**
        
        $$\text{MACs} = R \times S \times C = 3 \times 3 \times C = \mathbf{9C \text{ MACs}}$$
        

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