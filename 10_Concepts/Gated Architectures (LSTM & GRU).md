## Long Short-Term Memory (LSTM)

Proposed by Hochreiter & Schmidhuber (1997), the LSTM is designed to "remember" information for long periods by using a **Memory Cell** and a set of **logistic gates**.
### Architecture & Gates

An LSTM unit consists of four main components that interact multiplicatively to control the flow of information:

- **Input Gate**: Determines which new information will be stored in the cell state.
    
- **Forget Gate**: Decides what information to discard from the previous cell state.
    
- **Cell State (Memory Cell)**: Acts as a "highway" that carries information across time steps with minimal interaction, allowing gradients to flow back effectively.
    
- **Output Gate**: Decides what the next hidden state should be based on the cell state and current input.
![[Pasted image 20260215164653.png]]
### Properties & Use Cases

- **Constant Error Carousel (CEC)**: LSTMs can backpropagate through the cell state because the internal loop has a fixed weight, preventing the gradient from vanishing.

- The LSTM introduces the **Cell State ($C_t$)**. Look at the update equation for the cell state:
	$$C_t = (f_t \odot C_{t-1}) + (i_t \odot \tilde{C}_t)$$
	
	- $f_t$ is the Forget Gate.
	    
	- $i_t$ is the Input Gate.
	    
	- $\odot$ is element-wise multiplication.
	
	Notice there is **no weight matrix $W$** being multiplied by $C_{t-1}$ directly. The $W$ weights in an LSTM are used to calculate the _gates_ ($f_t, i_t, o_t$), but the cell state itself just flows through.
	
- **Why the "Carousel" stays constant**
	
	When the network wants to remember something for a long time, it learns to set the **Forget Gate ($f_t$) to approximately 1**.
	
	If $f_t \approx 1$, the equation for the gradient flow through the cell state becomes:
	
	$$\frac{\partial C_t}{\partial C_{t-1}} \approx 1$$
	
	When you backpropagate through 100 steps, you are multiplying by 1 over and over:
	
	$$\text{Gradient} \approx 1 \times 1 \times 1 \dots = 1$$
	
	The gradient does not vanish because it is **trapped in a linear loop** (the carousel). It doesn't pass through a "squashing" activation function (like Tanh or Sigmoid) and it isn't repeatedly multiplied by a weight matrix $W$ during its trip across the cell state.

- The gradient is mitigated because the LSTM separates the **information storage** (Cell State) from the **information processing** (Hidden State/Gates).
	
	- In the **Hidden State**, gradients still vanish because of the non-linearities.
	- In the **Cell State**, the gradient is "protected." As long as the forget gate is open ($1$), the error signal can travel back 1,000 steps without losing its strength, allowing the network to learn that something that happened in Step 1 is relevant to Step 1,001.

- **Use Cases**: Complex sequence tasks such as **Machine Translation**, **Speech Recognition**, and **Handwriting Generation** where long-distance context is vital.
---

## Gated Recurrent Unit (GRU)

The GRU is a more recent and simplified version of the LSTM (Cho et al., 2014).

### Architecture

The GRU streamlines the LSTM design by merging states and gates:

- **Update Gate**: Combines the functions of the LSTM's forget and input gates. It decides how much of the previous memory to keep and how much new information to add.
  
  The primary reason GRUs solve the vanishing gradient problem is the **Update Gate ($z_t$)**.
	In a standard RNN, the hidden state update is purely **multiplicative** ($h_t = \sigma(W \cdot h_{t-1} + \dots)$), which leads to exponential decay of the gradient. In a GRU, the update to the hidden state $h_t$ is **additive**:
	
	$$h_t = (1 - z_t) \odot h_{t-1} + z_t \odot \tilde{h}_t$$
	
	- **Linear Path:** If the update gate $z_t$ is close to 0, the model "remembers" the previous state $h_{t-1}$ almost perfectly ($h_t \approx h_{t-1}$).
	    
	- **Gradient Flow:** During backpropagation, this additive structure creates a "shortcut." The gradient of $h_t$ with respect to $h_{t-1}$ contains a term close to 1 (specifically $1-z_t$), which allows the error signal to flow backward across many time steps without vanishing.
    
- **Reset Gate**: Determines how much of the previous hidden state to forget.
	 While the Update Gate handles long-term memory, the **Reset Gate** determines how much of the past information to _forget_ when calculating the new candidate state $\tilde{h}_t$.
	 If the Reset Gate is near 0, the model ignores the previous hidden state and acts like it is seeing the first word of a new sequence.
    
- **Merged State**: Unlike LSTM, it does not have a separate "cell state"; it merges the cell state and hidden state into a single vector.
![[Pasted image 20260215172104.png]]
---

## Directionality: Unidirectional vs. Bidirectional

Both LSTM and GRU can be implemented in one or two directions depending on the availability of data.

- **Unidirectional (One-directional)**: The network traverses the sequence only from left to right. This is necessary for real-time applications where "future" data is not yet available (e.g., real-time speech-to-text).
    
- **Bidirectional (Two-directional)**: Two separate RNNs process the sequence—one from left-to-right and one from right-to-left. Their hidden states are concatenated at each time step.
    
    - **Advantage**: It allows the model to have context from both the past and the future for any given point in the sequence.
        
    - **Use Case**: **Part-of-Speech Tagging** or **Named Entity Recognition**, where the meaning of a word depends on the words both before and after it.
    ![[Pasted image 20260215172456.png]]

---

## Pros, Cons, and Comparison

### Comparison Table

| **Feature**     | **LSTM**                                              | **GRU**                                             |
| --------------- | ----------------------------------------------------- | --------------------------------------------------- |
| **Gates**       | 3 (Forget, Input, Output)<br>                         | 2 (Update, Reset)                                   |
| **States**      | Hidden state + Cell state                             | Single Hidden state                                 |
| **Complexity**  | Higher; more parameters to learn.                     | Lower; faster to train and more efficient.          |
| **Performance** | Theoretically better on very long, complex sequences. | Often matches LSTM performance on smaller datasets. |
| **Memory**      | Uses more memory due to extra gate/state.             | More memory-efficient.                              |

### Summary of Pros and Cons

- **LSTM Pros**: Most robust for long-term dependencies; widely researched with many variations.
    
- **LSTM Cons**: Computationally expensive and slower to converge than simpler models.
    
- **GRU Pros**: Faster training and requires less data to generalize; simple architecture.
    
- **GRU Cons**: May lack the "capacity" of LSTM for extremely nuanced long-term memory tasks.