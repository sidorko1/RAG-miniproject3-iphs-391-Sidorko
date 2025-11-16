
<p align="center">
  <h1 align="center">🔐 Immigration RAG Assistant</h1>
  <p align="center"><i>A Privacy-Preserving, Locally-Deployed RAG System for U.S. Immigration Pathways</i></p>

  <p align="center">
    <img src="https://img.shields.io/badge/Python-3.8%2B-blue" />
    <img src="https://img.shields.io/badge/RAG-Retrieval%20Augmented%20Generation-orange" />
    <img src="https://img.shields.io/badge/Focus-Immigration%20AI%20Safety-red" />
    <img src="https://img.shields.io/badge/Security-Local%20Only-green" />
  </p>

  <p align="center">
    <a href="#overview">Overview</a> •
    <a href="#features">Features</a> •
    <a href="#architecture">Architecture</a> •
    <a href="#technology-stack">Technology Stack</a> •
    <a href="#evaluation-plan">Evaluation Plan</a> •
    <a href="#references">References</a>
  </p>
</p>

---

# 🎯 Overview
This project implements a **local, privacy-preserving RAG assistant** that explains U.S. immigration processes including OPT, STEM OPT, Cap-Gap, H-1B, and EB-1A.  
International students often struggle with conflicting or unclear information from different websites. This assistant retrieves content only from **official government sources**, summarizes rules, and provides **verified citations**.

The system **does not** offer legal advice — it only explains publicly available rules.

---

# 🌟 Features
- 🔒 **Fully local pipeline** (no external APIs)
- 📘 **Official data only** (USCIS, DHS, SEVP, CFR)
- 🔍 **Accurate, traceable citations**
- ⚠️ **Safety layer** preventing legal advice
- ⚡ **Fast retrieval** using lightweight models
- 🔐 **Privacy-first design**

---

# 🛠 Architecture

```
┌───────────────────────────────────────────────────────────────┐
│                Immigration RAG Processing Pipeline             │
└───────────────────────────────────────────────────────────────┘

Document Ingestion (PDF → TXT/MD)
         ↓
Preprocessing & Normalization
         ↓
Heading-Aware Chunking (~512 tokens)
         ↓
Local Embeddings (MiniLM-L6-v2)
         ↓
Vector Store (ChromaDB w/ metadata)
         ↓
Dense Retrieval (top-5 cosine similarity)
         ↓
Local LLM (Mistral 7B via Ollama)
         ↓
Safety & Legal-Advice Guardrails
         ↓
Citation Assembly (doc + section + paragraph)
         ↓
Final Answer
```

---

# 🧩 Technology Stack

**Pipeline:**  
`Ingestion → Chunking → Embedding → Vector Store → Retrieval → LLM → Safety Layer`

| Component | Technology/Model | Rationale |
|----------|------------------|-----------|
| **Language Model (LLM)** | `Mistral 7B (Ollama)` | Runs fully offline; strong factual grounding. |
| **Embeddings** | `MiniLM-L6-v2` | Lightweight, fast, high cosine accuracy. |
| **Vector Database** | `Chroma` | Simple and efficient with metadata support. |
| **Chunking Strategy** | Heading-based | ~512 tokens preserves semantic structure. |
| **Retrieval Strategy** | Vector search (`top_k=5`) | Works well for structured government text. |
| **Safety Layer** | Custom rules + guardrails | Ensures no legal advice is provided. |

---

# 📚 Data & Constraints

## Corpus (20–30 Documents)
- USCIS Policy Manual (selected chapters)
- DHS SEVP Study in the States
- I-765 instructions (OPT)
- I-129 instructions (H-1B)
- 8 CFR §§214, 274a, 204.5(h)

Formats: **PDF, HTML, TXT**  
All data is public and cleaned locally.

```yaml
data_constraints:
  sources:
    - "USCIS"
    - "DHS SEVP"
    - "CFR Title 8"
    - "Official OPT/H-1B Instructions"
  formats: ["pdf", "html", "md"]
  size: "20–30 documents"
  local_only: true
  security: "public-data only; no personal data"
```

---

# 🏗 Architecture (MVP)

```yaml
architecture_mvp:
  steps: ["ingest", "chunk", "embed", "store", "retrieve", "generate", "postprocess"]
  chunking: "heading-based, ~512 tokens"
  embeddings: "MiniLM-L6-v2"
  vector_db: "Chroma"
  retrieval: "dense, top_k=5"
  llm: "Mistral 7B (local)"
  citations: true
  safety_layer: true
```

---

# ⚖️ Component Alternatives (Mini-Bakeoff)

```yaml
component_selection:
  vector_db:
    selected: "Chroma"
    reason: "simple local setup"
  embeddings:
    selected: "MiniLM"
    reason: "fast, local, free"
  llm:
    selected: "Mistral 7B"
    reason: "private + offline"
  retrieval:
    selected: "vector-only"
    reason: "corpus is small and clean"
  reranker:
    selected: "None"
    reason: "not needed for MVP"
```

---

# 📈 Evaluation Plan

## Test Set (15 Questions)
Covers:
- OPT eligibility  
- STEM OPT requirements  
- Cap-Gap  
- H-1B basics  
- EB-1A criteria  
- Travel / employment rules  

### Metrics

```yaml
evaluation:
  questions: 15
  baseline:
    correct: TBD
    with_citations: TBD
    avg_latency: TBD
    unsafe_outputs: 0
```

---

# ⚠️ Risks & Edge Cases
- Policy updates → outdated content  
- Long PDFs → loss of formatting  
- Conflicting information across documents  
- Users may expect legal advice  
- Acronym-heavy sections may confuse retrieval  

---

# 🚀 Future Work

```yaml
improvements:
  - "add hybrid retrieval (BM25 + dense)"
  - "add reranker"
  - "add uncertainty scoring"
  - "add version-aware retrieval"
  - "add structured checklists for visa steps"
```

---

# 📖 References
- USCIS (uscis.gov)  
- DHS SEVP Study in the States  
- Code of Federal Regulations (Title 8)  
- Chroma Documentation  
- SentenceTransformers Documentation  
- Ollama (Mistral) Documentation  

---
