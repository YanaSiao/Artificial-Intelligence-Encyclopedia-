In Recommender Systems and Search, we don't just care if an item is "relevant"; we care **where** it appears in the list.

- **MRR (Mean Reciprocal Rank):** $1/rank$ of the first relevant item.
- **NDCG (Normalized Discounted Cumulative Gain):** Rewards putting highly relevant items at the very top of the list.
- **Hit Rate @ K:** Was the correct item in the top K results?