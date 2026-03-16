### Lexical Search and Vocabulary Matching

Lexical search is the task of finding documents that contain the specific words (tokens) present in a user's query. This relies on an **Inverted Index**, a data structure that maps each word in the vocabulary to a list of documents (postings) where it appears.

Vocabulary matching requires strict preprocessing (Normalization, Stemming) to ensure that a query for "running" matches a document containing "run".

**Term Weighting** The importance of a term within a document is typically calculated using the **[[TF-IDF]]** framework, which balances local frequency against global rarity.

### The Vector Space Model for Retrieval

In search, both the **Query ($q$)** and the **Document ($d$)** are represented as vectors in a high-dimensional space where each dimension corresponds to a term in the vocabulary.

- **Similarity Measure:** The standard way to rank documents is **Cosine Similarity**, which measures the cosine of the angle between the query vector and the document vector.
    
- **Property:** This measure is length-normalized, meaning it prevents long documents from being ranked higher simply because they contain more words.

### Term Weighting and Scoring

Not all words in a query are equally important. Term weighting adjusts the "strength" of a word in the vector.

- **Linear (Raw) Score:** The weight is simply the count of the word.
    
- **Logarithmic Score:** Uses $1 + \log(\text{count})$ for terms that appear.
    
- **Justification:** Logarithmic scaling is used because the relevance of a word does not increase linearly with its frequency. If a word appears 20 times, it is more important than if it appears once, but it is not 20 times more important.

---
### Index Structures for Search

To enable fast retrieval without scanning every document, search engines use specialized data structures to map terms to their locations.

**The Inverted Index** The inverted index is the fundamental data structure in Information Retrieval. It consists of two main components:

- **Dictionary (Lexicon):** A list of all unique terms (tokens) in the corpus. This is usually kept in memory for fast lookup using hash tables or B-trees.
    
- **Postings Lists:** For each term in the dictionary, a list of document IDs (DocIDs) where that term occurs. These lists are typically sorted by DocID to allow for efficient "merging" during multi-word queries.
    

**Positional Indexes** A basic inverted index can tell you if a document contains "artificial" and "intelligence," but it cannot verify if they appear next to each other as a phrase. A **Positional Index** solves this by storing the specific word offsets within the postings:

- **Format:** `Term -> DocID: [pos1, pos2, ...]`
    
- **Use Case:** This is essential for **Phrase Queries** (e.g., "to be or not to be") and **Proximity Searches** (e.g., "climate" within 3 words of "change").
    
- **Trade-off:** Positional indexes significantly increase the storage size of the index compared to a standard inverted index.

---

### Web Crawlers

A crawler (or spider) is an automated software program that systematically browses the World Wide Web to build a local repository of documents for indexing. The process is cyclical and follows these steps:

1. **Seed Selection:** Start with a list of high-quality "Seed URLs."
    
2. **The Frontier (URL Queue):** New URLs discovered during crawling are added to a prioritized queue.
    
3. **Fetching:** The crawler requests the page content using the HTTP protocol.
    
4. **Parsing:** The HTML is parsed to extract text for the index and new links for the Frontier.
    
5. **Duplicate Detection:** Sophisticated hashes (SimHash) are used to avoid indexing the same content multiple times via different URLs.
    

**Politeness and Ethics** 
Crawlers must be "good citizens" to avoid crashing servers or violating privacy:

- **Robots.txt:** A protocol used by websites to communicate with web crawlers. It specifies which parts of the site should not be visited.
    
- **Crawl Delay:** Wait times between requests to a single server to prevent unintended Denial of Service (DoS).
    
- **User-Agent:** Identifying the crawler (e.g., "Googlebot") so webmasters can contact the operator.