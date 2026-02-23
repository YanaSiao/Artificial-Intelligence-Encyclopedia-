## 1. Overview of Sequence Modeling Problems

Sequential data problems are categorized by the relationship between the input and output sizes:

- **One-to-Many (Sequence Output)**: A fixed-size input produces a sequence of varying length.
    - _Example_: **Image Captioning**, where one image is the input and a sentence (sequence of words) is the output.
        
- **Many-to-One (Sequence Input)**: A varying-length sequence input produces a single fixed-size output.
    - _Example_: **Sentiment Analysis**, where a tweet (sequence) is classified as positive or negative.
        
- **Many-to-Many (Sequence Input & Output)**: Both input and output are sequences.
    - **Non-Synced**: The output sequence length differs from the input, often produced after the entire input is processed. _Example_: **Machine Translation**.
    - **Synced**: Every input frame corresponds to an output label. _Example_: **Video Classification** (labeling each frame).

![[Pasted image 20260216183523.png]]

---

## 2. The Encoder-Decoder Framework

The **Encoder-Decoder** architecture is the standard framework for Seq2Seq models like Machine Translation. It aims to maximize the conditional probability $P(y|x)$.

Core Components

- **Encoder**: Processes the input sequence $x_1, \dots, x_m$ and compresses it into a fixed-size **context vector**. This vector acts as a summary of the entire input.
- **Decoder**: Takes the context vector and generates the output sequence $y_1, \dots, y_n$ one step at a time, often using the previous predicted word as input for the next step.
![[Pasted image 20260216183830.png]]
---

## 3. Training and Special Tokens

Teacher Forcing

During **training**, the decoder does not use its own (possibly wrong) predictions for the next step. Instead, it uses the **ground-truth target** from the training data as input to the next time step to stabilize learning.

Special Characters
To manage varying sequence lengths and batching, several special tokens are used:

- `<PAD>`: Pads shorter sequences to match the fixed width of a training batch.
    
- `<EOS>` (End of Sequence): Signals the end of a sentence for both input and output.
    
- `<SOS>` (Start of Sequence) or `<GO>`: The trigger input for the first step of the decoder.
- `<UNK>` (Unknown): Replaces rare words not found in the vocabulary to improve efficiency. 

---

## 4. Inference: Decoding Strategies

At **inference time**, the model must generate a sequence without knowing the true targets.

- **Greedy Decoding**: At each step, the model picks the token with the highest probability. While fast, it cannot backtrack or correct early mistakes, which may lead to suboptimal final sequences.
- **Beam Search**: Tracks the $k$ most probable hypotheses (sequences) simultaneously. This provides a better global search for the most likely sequence than greedy decoding