- **Core Concept**: RNNs are deterministic systems where the hidden state $h_t$ is a function of the current input $x_t$ and the previous hidden state $h_{t-1}$. This "distributed hidden state" allows the network to store information about the past efficiently.
    
- **Training (BPTT)**: Backpropagation Through Time involves "unrolling" the network for $U$ steps. Replicas of the weights are maintained across time steps, and their gradients are averaged for the update.
    ![[Pasted image 20260215164356.png]] ![[Pasted image 20260215164423.png]]
- **The Vanishing Gradient Problem**: In RNNs, this is caused by the repeated multiplication of the same weight matrix and activation derivatives (like Sigmoid or Tanh). If the norm of these terms is $< 1$, the gradient converges to 0 over many steps, preventing the learning of long-term dependencies.
    

#### [[Gated Architectures (LSTM & GRU)]]

- **LSTM (Long Short-Term Memory)**: Uses a "memory cell" with three primary gates to control information flow:
    - **Input Gate**: Controls when new information enters the cell.
    
    - **Forget Gate**: Controls how long old information is kept.
    
    - **Output Gate**: Controls when the cell state influences the output. 
    
- **GRU (Gated Recurrent Unit)**: A simplified variant that merges the forget and input gates into a single **update gate** and combines the cell state and hidden state.

#### [[Sequence-to-Sequence Models (Seq2Seq)]]

- **Architecture**: Composed of an **Encoder** (which compresses the input sequence into a context vector) and a **Decoder** (which generates the output sequence).

- **Decoding Strategies**:
    - **Greedy Decoding**: Picks the most probable token at each step; fast but cannot backtrack from errors.
    - **Beam Search**: Tracks several of the most probable hypotheses to find a better global sequence.
- **Special Tokens**: Uses tokens like `<PAD>` (for batching), `<EOS>` (end of sequence), and `<SOS>` (start of decoding) to manage varying sequence lengths.
#### [[Attention Mechanisms]]

- **The Bottleneck**: Traditional Seq2Seq models rely on a single fixed-size context vector, which is often insufficient for long sentences.
    
- **Solution**: Attention allows the decoder to "look back" at different parts of the input sequence at each step of the generation process.
    
- **Types**:
    
    - **Bahdanau Attention**: Uses a Multi-layer Perceptron to compute scores.
        
    - **Luong Attention**: Uses simpler dot-products or bilinear functions.