## Parameters (Flash Footprint)

Parameters (weights and biases) are static and stored in non-volatile memory (**Flash**). The total memory in bytes depends on the data type precision ($B_{\text{element}}$):

- **FP32:** $B_{\text{element}} = 4 \text{ bytes}$
    
- **FP16 / Type-BF16:** $B_{\text{element}} = 2 \text{ bytes}$
    
- **INT8:** $B_{\text{element}} = 1 \text{ byte}$

### Fully Connected (Dense) Layer

Let:

- $N_{\text{in}} = \text{Number of input neurons}$
    
- $N_{\text{out}} = \text{Number of output neurons}$
    

$$\text{Params}_{\text{no\_bias}} = N_{\text{in}} \times N_{\text{out}}$$

$$\text{Params}_{\text{with\_bias}} = (N_{\text{in}} + 1) \times N_{\text{out}}$$

$$\text{Memory (Bytes)} = \text{Params} \times B_{\text{element}}$$

### 2D Convolutional Layer (Conv2D)

Let:

- $K_H, K_W = \text{Kernel height and width}$
    
- $C_{\text{in}} = \text{Number of input channels}$
    
- $C_{\text{out}} = \text{Number of output channels (filters)}$
    

$$\text{Params}_{\text{no\_bias}} = K_H \times K_W \times C_{\text{in}} \times C_{\text{out}}$$

$$\text{Params}_{\text{with\_bias}} = (K_H \times K_W \times C_{\text{in}} + 1) \times C_{\text{out}}$$

$$\text{Memory (Bytes)} = \text{Params} \times B_{\text{element}}$$

## Activations (RAM Footprint)

Activations are the intermediate feature maps generated during a forward pass. Because they change with every inference cycle, they must be stored in volatile memory (**SRAM/RAM**).

Let:

- $H, W = \text{Height and Width of the tensor}$
    
- $C = \text{Channels of the tensor}$
    

$$\text{Activation Size (Elements)} = H \times W \times C$$

$$\text{Activation Memory (Bytes)} = H \times W \times C \times B_{\text{activation}}$$

> [!IMPORTANT]
> 
> **Peak RAM vs. Total RAM**
> 
> In runtime frameworks (like TensorFlow Lite Micro), memory is optimized using a **ping-pong buffer** strategy. You do _not_ sum all activations across the entire network. Instead, the maximum RAM required at any given moment is usually:
> 
> $$\text{Peak RAM} = \max_{\text{layer } i} \left( \text{Input Activation}_i + \text{Output Activation}_i \right)$$

## Total Memory Footprint Summary

$$\text{Total Flash Required} = \sum (\text{Parameter Memory of all layers})$$

$$\text{Total RAM Required} = \text{Peak RAM Buffer} + \text{System/Workspace Overhead}$$