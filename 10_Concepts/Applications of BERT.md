BERT (Bidirectional Encoder Representations from Transformers) is highly flexible due to its bidirectional nature, allowing it to be adapted for a variety of Natural Language Understanding (NLU) tasks through fine-tuning.

### 1. Text Classification

BERT can be adapted to categorize an entire sentence or document into specific classes.

- **How to Adapt:** A special `[CLS]` (Classification) token is added to the beginning of the input text. During fine-tuning, the model is taught to produce a class label in place of the `[CLS]` token’s final hidden state, effectively treating it as an aggregate representation of the whole sequence.
    
- **Examples:** Sentiment analysis (positive/negative), spam detection, or topic labeling (e.g., "highly relevant" vs. "spam").

### 2. Sequence Labelling

This task involves assigning a label to each individual token in a sequence.

- **How to Adapt:** Instead of using just the `[CLS]` token, the model produces an output for every token position in the sequence. The final hidden states of all tokens are passed through a classification layer to predict a label for each word.
    
- **Examples:** Part-of-Speech (POS) tagging or Named Entity Recognition (NER) using Begin/Inside/Outside (B/I/O) labels to identify entities like names or locations.

### 3. Text Pair Classification

BERT is exceptionally strong at comparing two different pieces of text to determine their relationship.

- **How to Adapt:** Two pieces of text are concatenated into a single input sequence, separated by a special `[SEP]` (Separator) token. The model then uses the `[CLS]` token representation to predict the relationship between the two segments.
    
- **Examples:** Determining if two statements agree with each other (Natural Language Inference) or assessing the relevance between a search query and a document.

### 4. Document Re-ranking

In search systems, BERT can be used to perform high-precision re-ranking of candidate documents.

- **How to Adapt:** The model is fine-tuned to predict a relevance label for a `<query, document>` pair. The query and document are separated by the `[SEP]` token. This is often used in a hybrid approach where a faster method (like TF-IDF or BM25) finds candidates, and BERT re-ranks them for the final result.
    
- **Example:** A web search where a query like "banana pancake recipe" is paired with a document to produce a "highly relevant" score.

### 5. Document Embedding

Document embeddings represent entire texts as dense vectors, which are essential for semantic search and vector databases.

- **How to Adapt:** The final hidden state of the `[CLS]` token is used as the numerical representation (embedding) of the document. Similarity between documents is then measured using the dot-product of these embeddings.
    
- **Sentence BERT (sBERT):** This is a specialized version of BERT fine-tuned on large datasets of paired documents specifically to produce high-quality embeddings.

### 6. Contrastive Loss Use

[[Metric-Based Learning & Triplet Loss|Contrastive loss]] is the primary method for training BERT to produce useful document embeddings where distance in the vector space corresponds to semantic similarity.

- **Goal:** To ensure that the dot-product between embeddings is high for similar documents and low for irrelevant ones.
    
- **Procedure:**
    
    1. Create batches of relevant `<query, document>` pairs.
        
    2. Compute the dot-product between all queries and documents in the batch to form a similarity matrix.
        
    3. The **diagonal elements** (correct pairs) should have high values, while **off-diagonal elements** (irrelevant pairs) should have low values.
        
    4. Apply a **softmax** along the rows or columns to determine how well the model identifies the relevant pairs.
        
    5. Use **cross-entropy classification loss** to update the model; this "contrasts" the relevant pairs against the irrelevant ones.