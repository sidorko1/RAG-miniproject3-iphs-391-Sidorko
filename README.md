<p align="center">
  <h1 align="center">🔐 Immigration RAG Assistant</h1>
  <p align="center"><i>A Privacy-Preserving, Locally-Deployed RAG System for U.S. Immigration Pathways</i></p>

  <p align="center">
    <img src="https://img.shields.io/badge/Python-3.8%2B-blue.svg" />
    <img src="https://img.shields.io/badge/RAG-Retrieval%20Augmented%20Generation-orange.svg" />
    <img src="https://img.shields.io/badge/Focus-Immigration%20AI%20Safety-red.svg" />
    <img src="https://img.shields.io/badge/Security-Local%20Only-green.svg" />
  </p>

  <p align="center">
    <a href="#-overview">Overview</a> •
    <a href="#-features">Features</a> •
    <a href="#-architecture">Architecture</a> •
    <a href="#-evaluation-plan">Evaluation Plan</a> •
    <a href="#-documentation">Documentation</a>
  </p>
</p>

---

# 🎯 Overview
This project proposes a **secure, local-first RAG (Retrieval-Augmented Generation) system** designed to assist with U.S. immigration pathways including OPT, STEM OPT, H-1B, cap-gap, EB-1A, and L-1.  
It demonstrates modern principles of **trustworthy AI**, including privacy preservation, low hallucination rates, robust retrieval, and citation-level provenance.

Because immigration rules are high-stakes and legally sensitive, the system runs **100% offline** and uses **only open-source components**, making it suitable for academic, research, and constrained enterprise environments.

---

# ✨ Features

### 🔒 Privacy-Preserving (Fully Local)
- No API calls  
- All embeddings, retrieval, and inference run on-device  
- Suitable for sensitive regulatory content  

### 🧠 High-Precision Retrieval
- Heading-aware chunking (512 tokens)  
- Dense vector retrieval using MiniLM embeddings  
- Each answer grounded in verifiable paragraph citations  

### 🛡️ Safety Guardrails
- Blocks unsafe or legally actionable queries  
- Ensures all answers remain purely informational  
- Includes hallucination rejection logic (“insufficient evidence”)  

### 📎 Provenance Tracking
Each answer includes:
- Document source  
- Section  
- Paragraph index  
- Confidence / uncertainty flag  

---

# 🏗 Architecture

```
┌───────────────────────────────────────────────────────────────┐
│                  Immigration RAG Processing Pipeline            │
└───────────────────────────────────────────────────────────────┘

   Document Ingestion (PDF → TXT/MD)
                    ↓
   Preprocessing & Normalization
                    ↓
   Heading-Aware Chunking (512 tokens)
                    ↓
   Local Embeddings (MiniLM-L6-v2)
                    ↓
   Vector Store (ChromaDB w/ metadata)
                    ↓
   Dense Retrieval (top-5 cosine similarity)
                    ↓
   Local LLM (Mistral 7B via Ollama)
                    ↓
   Guardrails (NeMo + custom constraints)
                    ↓
   Citation Assembly (doc + section + paragraph)
                    ↓
   Final Answer
```

---

# 🧩 Technical Stack

| Component | Choice | Rationale |
|----------|--------|-----------|
| **Embeddings** | MiniLM-L6-v2 | Fast, local, high-quality |
| **Vector DB** | ChromaDB | Persistent metadata search |
| **LLM** | Mistral 7B (Ollama) | Accurate + runs offline |
| **Guardrails** | NeMo + custom regex | Prevent unsafe/legal requests |
| **Chunking** | 512-token heading-aware | Maximizes semantic relevance |
| **Retrieval** | Dense cosine, top-k=5 | Reliable for small corpora |

---

# 📚 Corpus Summary

- USCIS Policy Manual  
- DHS/SEVP STEM OPT Guidance  
- I-765 and I-129 official instructions  
- 8 CFR §214, §274a  
- EB-1A criteria (§204.5(h))  
- DHS employment authorization categories  

All documents converted to normalized text, with:
- Header/footer removal  
- Standardized section identifiers  
- Deduplication  
- Paragraph-level metadata tags  

---

# 📈 Evaluation Plan

### **Test Set (15 Questions)**
Covers:
- OPT & STEM OPT eligibility  
- H-1B lottery vs cap-exempt  
- Cap-gap extension  
- EB-1A extraordinary ability criteria  
- L-1 intracompany transfers  
- Employment restrictions under §274a.12  

### **Metrics**

| Metric | Target |
|--------|--------|
| **Accuracy** | ≥ 85% |
| **Citation Coverage** | ≥ 90% |
| **Latency** | ≤ 5 sec |
| **Hallucinated Citations** | 0 |
| **Safety Compliance** | 100% |

### **Expected Results**
- ~13/15 accurate answers  
- ~3 seconds average latency  
- 0 hallucinated citations  

---

# ⚠ Limitations
- Long tables (SEVP PDFs) may degrade during OCR  
- Acronym-heavy sections may require hybrid retrieval  
- Conflicting versions of federal rules may require versioning  
- Guardrails may over-filter certain nuanced questions  

---

# 🚀 Future Work
- Hybrid retrieval (BM25 + dense)  
- Cross-encoder reranking (ColBERT v2)  
- Conformal RAG uncertainty estimation  
- Evidence graph generation  
- Multi-turn session memory with safety boundaries  

---

# 📘 Documentation
- `CAPB_report.md` (full project blueprint)  
- `evaluation/questions.json`  
- `src/*` (pipeline code modules)  

---

# 👤 Author
**Kirill Sidorko**  
IPHS 391 — AI Security & Governance  
Kenyon College  
