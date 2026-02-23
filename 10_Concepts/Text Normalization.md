## Defining NLP

Natural Language Processing is the field concerning the **computational analysis, interpretation, and production** of natural language, whether in written or spoken form.

- **Etymology:** The terms derive from "birth" (natural), "tongue" (language), and "progress" (process).
- **Key Distinction:** Human language is **compositional** (allowing endless new sentences from finite words) and **referential** (conveying temporal and spatial information), which distinguishes it from simpler animal communication.

## The Core Challenges

Processing text is difficult because human language is:

- **Extremely Expressive:** We can express anything, even logically inconsistent or nonsensical statements that remain grammatically correct (e.g., Noam Chomsky's famous _"Colorless green ideas sleep furiously"_).
    
- **Highly Ambiguous:** Ambiguity exists at multiple levels:
    - **Lexical:** A word can be a noun or a verb (e.g., "duck").
    - **Semantic:** One word can have multiple meanings (e.g., "make" can mean "create" or "cook").
    - **Grammatical:** How words relate to one another.
        
- **Redundant and Noisy:** There are many ways to say the same thing, including misspellings or variations in emphasis (**prosody**) that change meaning.

## Text Preprocessing Pipeline

### A. Extraction & Cleaning
- **Source Extraction:** Removing markup like HTML tags or reconstructing text from complex formats like PDFs or OCR-scanned images.
- **Encoding:** Transitioning from ASCII (128 characters) to **UTF-8 (Unicode)** to support 160+ languages and special characters like diacritics.

### B. Tokenization & Segmentation
- **Tokenization:** Segmenting text into sequences of characters or words.
    - _Issues:_ Blindly removing punctuation can break URLs or hashtags; some languages (like Chinese) do not use spaces to separate words.
- **Sentence Segmentation:** Dividing text into sentences using punctuation (e.g., ".", "!", "?"), though this is tricky for abbreviations like "Dr." or "Inc.".

### C. Normalization
Putting tokens into a standard format to improve search and classification.

- **Case Folding:** Reducing everything to lowercase (e.g., "WHO" (organization) and "Who" (music band) become "who"), which simplifies models but can lose information.
- **Stemming:** A rule-based approach (e.g., **Porter Stemmer**) that chops off affixes to find a "stem". It is fast but can cause "collisions" where different words produce the same stem.
- **Lemmatization:** A more complex process that uses a **lexicon** (dictionary) to find the shared root or "lemma" (e.g., "am, are, is" $\rightarrow$ "be")

