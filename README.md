# 🧠 HELP MATE AI – Insurance Document Question Answering Assistant

## 📌 Overview

**HELP MATE AI** is an intelligent question-answering assistant that parses insurance policy PDFs and provides contextual, human-like responses to user queries.

This system leverages modern NLP techniques, including large language models (LLMs), semantic embeddings, and reranking algorithms to deliver accurate, cited answers. It is designed as a **retrieval-augmented generation (RAG)** pipeline for scalable and efficient semantic search over complex documents.

---

## 🎯 Objectives

- 📄 Automate extraction of relevant details from large insurance PDF documents.
- 🧠 Leverage **LLMs** and **embeddings** for deep semantic understanding.
- 📚 Use a fast, persistent **vector database (ChromaDB)** for document retrieval.
- ✅ Improve result precision using **CrossEncoder reranking**.
- 💰 Reduce API cost and latency using a **query caching mechanism**.
- 📖 Deliver **human-readable answers with source citations**.

---

## 🏗️ System Design

The system is built as a modular **RAG pipeline** with the following components:

### 🔄 Pipeline Steps

1. **PDF Parsing & Preprocessing**  
   → Extract structured text, headings, and tables.

2. **Vector Database (ChromaDB)**  
   → Store document embeddings and cache.

3. **Semantic Search**  
   → Retrieve top-k relevant chunks using cosine similarity.

4. **CrossEncoder Reranking**  
   → Re-rank retrieved passages for higher precision.

5. **LLM-Based Answer Generator**  
   → Produce natural language answers with in-text citations.

6. **Cache Layer**  
   → Save frequent query results to avoid redundant processing.

### 🧬 Architecture Flow

User Query → Check Cache → [Not Cached] → Semantic Search → Reranking → Top 3 Passages → LLM → Answer with Citations

---

## 🛠️ Implementation Details

### a. Environment Setup

- **Libraries Used**:
  - `pdfplumber`, `openai`, `chromadb`, `tiktoken`, `pandas`, `sentence-transformers`
- **Embeddings**: `text-embedding-ada-002` (OpenAI)
- **Reranking Model**: `ms-marco-MiniLM-L-6-v2` (CrossEncoder)
- **LLM**: `gpt-3.5-turbo` (OpenAI)

### b. PDF Parsing

- Extracts content page-wise: headings, body text, and tables.
- Stored as a DataFrame: `[Page No., Heading, Text, Metadata]`

### c. Vector Store (ChromaDB)

- **Collections**:
  - `InsurancePolicyDoc`: Stores all document embeddings.
  - `Insurance_Cache`: Stores query results and metadata.
- Uses persistent storage for scalability.

### d. Semantic Search

- Retrieves top 10 chunks using **cosine similarity**.
- Applies a relevance threshold (`0.2`) to filter results.

### e. Reranking

- Applies CrossEncoder scoring on `[query, chunk]` pairs.
- Selects the **top 3 highest-scoring** chunks.

### f. Response Generation

- Inputs: `User Query + Top 3 Chunks`
- Output: Natural, **LLM-generated response** with inline citations.

### g. Caching

- Query-answer pairs are stored in `Insurance_Cache`.
- Metadata is flattened and stored as JSON.

### h. Core Modular Functions

- `get_context()` – Checks cache and retrieves context if needed.
- `rerank()` – Scores relevance using CrossEncoder.
- `top_3_context()` – Extracts top 3 snippets.
- `get_reply()` – Generates final LLM answer.

---

## ⚠️ Challenges

- 📑 **PDF Complexity**: Insurance documents contain inconsistent formatting and tables.
- 💵 **Embedding Cost**: Long documents increase API usage.
- 🐢 **Latency**: CrossEncoder improves accuracy but adds delay.
- 🧾 **Cache Schema**: Flattening JSON metadata required tuning.
- 🤖 **LLM Hallucinations**: Occasional unsupported answers without citations.

---

## 💡 Lessons Learned

- 🔍 Combining **Embeddings + Reranking** greatly improves answer quality.
- ⚡ Caching is essential for performance and cost savings.
- 🧾 Structured PDF preprocessing (e.g., headings + tables) improves semantic retrieval.
- 📚 Maintaining metadata (e.g., page numbers) builds **trust via citations**.
- 🧩 Modular architecture simplifies debugging, upgrades, and future improvements.

---

## ✅ Conclusion

**HELP MATE AI** showcases an effective retrieval + generation pipeline for interpreting insurance documents. By integrating semantic search (via embeddings), precision tuning (via reranking), and advanced LLMs (for natural response generation), the system delivers trustworthy, cited answers.

---

## 🔄 Future Extensions

This solution can be adapted to other domains requiring semantic understanding of large documents, such as:

- ⚖️ Legal Contracts
- 📘 Compliance Manuals
- 📄 Research Papers
- 🏥 Medical Guidelines

---



