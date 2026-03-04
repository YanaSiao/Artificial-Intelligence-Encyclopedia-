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
- **Stop Word Removal:** High-frequency words (the, a, is) are often removed in BoW models because they follow **Zipf's Law**—they are very frequent but carry little discriminative power for classification.

- **Source Extraction:** Removing markup like HTML tags or reconstructing text from complex formats like PDFs or OCR-scanned images.

- **Encoding:** Transitioning from ASCII (128 characters) to **UTF-8 (Unicode)** to support 160+ languages and special characters like diacritics.

### B. Tokenization & Segmentation
Tokenization is more than just splitting by spaces; it requires handling complex linguistic edge cases.

- **Deterministic Tokenization:** Standard libraries like **NLTK** use complex RegEx patterns to handle:
    - **Clitics:** Separating "doesn't" into `does` and `n't`.
    - **Abbreviations:** Recognizing "U.S.A." as a single token rather than three sentences.
    - **Currency/Punctuation:** Keeping "$12.40" together while separating the trailing period.

- **Language-Specific Challenges:** 
	- Languages like **Chinese, Japanese, and Thai** do not use spaces to mark word boundaries.
    - **Chinese Segmentation:** Can be done at the word level (e.g., "Yao Ming") or character level ("Yao" + "Ming"), which drastically changes the resulting vocabulary.

- **Sentence Segmentation:** Uses punctuation cues but must be "abbreviation-aware" to avoid false sentence breaks after titles like "Dr." or "Inc.".

### C. Normalization
Putting tokens into a standard format to improve search and classification.

- **Case Folding:** Lowercasing text to map "Apple" and "apple" to the same dimension.
    - _Risk:_ Can be harmful for **Named Entity Recognition** where "US" (country) and "us" (pronoun) must be distinct.
    
- **Stemming (Porter Stemmer):** A fast, crude heuristic that chops suffixes (e.g., "ponies" $\rightarrow$ "poni").
    - _Issue:_ **Over-stemming** (collapsing unrelated words) or **Under-stemming** (failing to link related words).
    
- **Lemmatization:** Uses a **morphological analysis** and a dictionary to return the "Lemma" (dictionary headword).
    - _Example:_ "saw" $\rightarrow$ "see" (if used as a verb) or "saw" (if used as a noun). This requires **Part-of-Speech (POS)** tagging to be accurate.



