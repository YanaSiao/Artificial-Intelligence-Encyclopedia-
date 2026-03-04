## 1. Convolutional Layers (`Conv2D`)

- **Parameters**: High. Weights are shared across the spatial extent but exist for every input channel.
    - **Formula**: $\text{Params} = \left[ (K_H \times K_W \times C_{in}) + 1 \right] \times C_{out}$
        
- **Channels ($C$)**: The number of output channels is determined strictly by the number of filters ($C_{out}$).
    
- **Spatial Dimensions ($H, W$)**:
    - **Formula**: $O = \left\lfloor \frac{I - K + 2P}{S} \right\rfloor + 1$
        
    - **Behavior**: Decreases with **Valid** padding ($P=0$) or **Stride** ($S>1$). Stays the same with **Same** padding (if $S=1$).

## 2. Pooling Layers (`MaxPool2d`, `AvgPool2d`)

- **Parameters**: **0**. These are fixed mathematical rules (Max or Average).
- **Channels ($C$)**: **No change**. Each channel is processed independently.
- **Spatial Dimensions ($H, W$)**:
    - **Formula**: $O = \left\lfloor \frac{I - K + 2P}{S} \right\rfloor + 1$
        
    - **Behavior**: Specifically designed to decrease $H$ and $W$ (Downsampling).

## 3. Dense (Linear) Layers

- **Parameters**: Very High. Every input neuron connects to every output neuron.
    - **Formula**: $\text{Params} = (InputNeurons + 1) \times OutputNeurons$
        
- **Channels & Spatial Dimensions**: These layers operate on 1D vectors. Before entering a Dense layer, the 3D volume $(H \times W \times C)$ must be **Flattened** or passed through **GAP**.

## 4. Batch Normalization (`BatchNorm2d`)

- **Parameters**: Small, scales with the number of channels.
    - **Trainable**: $2 \times C$ ($\gamma$ and $\beta$ scale/shift).
        
    - **Non-Trainable**: $2 \times C$ (Running Mean and Running Variance).
        
- **Channels, $H$, $W$**: **No change**. It normalizes values pixel-wise/channel-wise.

## 5. Structural & Regularization Layers

|**Layer Type**|**Parameters**|**Effect on C**|**Effect on H & W**|
|---|---|---|---|
|**Activation (ReLU)**|0|None|None|
|**Dropout**|0|None|None|
|**Flatten**|0|Collapses to 1D|Collapses to 1D|
|**GAP**|0|None|Collapses to $1 \times 1$|
|**Upsampling**|0|None|**Increase** (Scale Factor)|

### Note on Global Average Pooling (GAP) vs. Flattening

- **Flattening** results in $H \times W \times C$ neurons. For a $7 \times 7 \times 512$ volume, this is **25,088** neurons.
    
- **GAP** results in $1 \times 1 \times C$ neurons. For the same volume, this is only **512** neurons.
    
- **Result**: GAP significantly reduces the parameters of the following Dense layer.
    

---

## Summary Table

|**Layer Type**|**H & W Change**|**C Change**|**Trainable Params**|
|---|---|---|---|
|**Conv2D**|$\lfloor \frac{I - K + 2P}{S} \rfloor + 1$|Becomes $C_{out}$|$[(K^2 \cdot C_{in}) + 1] \cdot C_{out}$|
|**Pooling**|$\lfloor \frac{I - K + 2P}{S} \rfloor + 1$|None|0|
|**Dense**|N/A (1D)|N/A (1D)|$(N_{in} + 1) \cdot N_{out}$|
|**Batch Norm**|None|None|$2 \times C_{in}$|
|**Upsample**|$I \times \text{Scale}$|None|0|