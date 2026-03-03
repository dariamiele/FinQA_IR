# Investigating Polarity Manipulation in LLM-Based Query Expansion for Financial Opinion Search

This project investigates how sentiment-controlled LLM-based query expansion affects retrieval effectiveness in financial opinion-oriented information retrieval.

We evaluate whether inducing **neutral, positive (bullish), or negative (bearish)** polarity in query expansions systematically alters ranking performance in a hybrid retrieval architecture.

---

## Abstract

Financial information retrieval often involves short, underspecified, opinion-driven queries where semantic precision is crucial.  

While Large Language Models (LLMs) can enrich queries through semantic expansion, sentiment-controlled expansions may introduce linguistic bias and query drift.

This project studies the impact of polarity manipulation within a **hybrid retrieval system (BM25 + Bi-Encoder re-ranking)** using the **FinQA retrieval dataset**.

Results show that all LLM-based expansions degrade performance compared to original queries, with **positive (bullish) expansions causing the largest performance drop**, while **negative (bearish) expansions are comparatively more resilient**.

---

## Task and Dataset

- **Dataset:** FinQA Retrieval Dataset  
- **Documents:** 57,638 financial documents  
- **Queries:** 648 short natural language queries  
- **Average query length:** ~5 tokens  
- **Relevance labels:** Binary  
- **Average relevant documents per query:** 2.63  

The dataset is challenging due to:
- Short and underspecified queries  
- Sparse relevance judgments  
- Heterogeneous corpus (microblogs, news, reports)  
- Domain-specific financial terminology  

---

## Methodology

The experimental setup follows a controlled two-phase structure.

### Phase I – Baseline Systems

We implemented:

- TF-IDF
- BM25
- BM25 + RM3 (pseudo-relevance feedback)
- Hybrid system:
  - BM25 candidate retrieval
  - Bi-Encoder semantic re-ranking (Sentence-BERT architecture)

The hybrid system serves as the main reference baseline.

---

### Phase II – Sentiment-Controlled LLM Query Expansion

For each query, three expanded variants were generated using prompt engineering:

- Neutral expansion  
- Positive (bullish) expansion  
- Negative (bearish) expansion  

All expansions were:

- Generated offline  
- Cached for reproducibility  
- Appended to original queries with fixed length  

To verify sentiment manipulation effectiveness, expansions were evaluated using **FinBERT**, a finance-specific sentiment classifier.

Polarity was validated to ensure:
Positive > Neutral > Negative in sentiment score.

---

## Experimental Setup

- Framework: PyTerrier
- Neural models: Hugging Face Transformers + Sentence-Transformers
- Execution environment: Python (Google Colab)
- Evaluation metrics:
  - MAP
  - nDCG@10
  - Precision@10
  - Recall@10

The document index, retrieval pipeline, and reranker remained constant across experiments.  
The only variable was query formulation.

---

## Results

Hybrid baseline with original queries achieved the best performance:

| Variant        | MAP   | nDCG@10 | P@10  | R@10  |
|---------------|--------|---------|-------|-------|
| Original      | 0.2991 | 0.3606  | 0.0974| 0.4239|
| Neutral QE    | 0.2225 | 0.2700  | 0.0691| 0.3149|
| Positive QE   | 0.1852 | 0.2282  | 0.0602| 0.2773|
| Negative QE   | 0.2132 | 0.2555  | 0.0676| 0.2988|

### Key Findings

- All query expansion strategies degrade performance.
- Positive (bullish) expansions produce the largest drop.
- Negative (bearish) expansions are more stable.
- Query drift is the primary cause of degradation.
- In sparse-relevance financial datasets, semantic precision is more valuable than expansion breadth.

---

## Discussion

Financial IR is highly sensitive to small linguistic variations.

In short, opinion-oriented queries:

- Expanding semantic breadth can dilute intent.
- Sentiment steering alters ranking behavior.
- Hybrid systems are fragile to polarity manipulation.

Future directions include:

- Intent-preserving query rewriting
- Gated or selective expansion
- Bias-aware LLM integration in retrieval pipelines

---

## Tech Stack

- Python
- PyTerrier
- ir_datasets
- Hugging Face Transformers
- Sentence-Transformers
- FinBERT
- NumPy / Pandas / SciPy

---

## Repository Structure

- `financial_query_expansion.ipynb` – Full experimental notebook
- `IR_report.pdf` – Complete academic report
- `requirements.txt` – Dependencies

---

## Authors

Daria Miele
Beatrice Camera
Zofia Pempera

Bachelor in Artificial Intelligence  
University of Pavia – University of Milan – University of Milano-Bicocca  
Academic Year 2025/2026
