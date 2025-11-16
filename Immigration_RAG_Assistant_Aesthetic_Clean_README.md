
<p align="center">
  <h1 align="center">🔐 Immigration RAG Assistant</h1>
  <p align="center"><i>A Privacy-Preserving RAG System for U.S. Student & Work Visa Procedures</i></p>

  <p align="center">
    <img src="https://img.shields.io/badge/License-MIT-green.svg" />
    <img src="https://img.shields.io/badge/Status-In%20Development-yellow.svg" />
    <img src="https://img.shields.io/badge/Python-3.10%2B-blue.svg" />
  </p>

  <p align="center">
    <a href="#-overview">Overview</a> •
    <a href="#-target-audience">Target Audience</a> •
    <a href="#-key-features">Features</a> •
    <a href="#-architecture--technology-stack">Architecture</a> •
    <a href="#-success-criteria">Success Criteria</a> •
    <a href="#-evaluation-plan">Evaluation</a> •
    <a href="#-future-work--roadmap">Future Work</a> •
    <a href="#-license">License</a>
  </p>
</p>

---

# 🌐 Overview
Navigating U.S. immigration rules is overwhelming for many international students. Information about F-1 status, OPT, STEM OPT, cap-gap extensions, SEVIS fees, I-20 issuance, and visa interview procedures is scattered across dense government websites.

This project implements a **modern Retrieval-Augmented Generation (RAG) agent** that centralizes these procedures into a clear, trustworthy system, retrieving answers only from **official U.S. government sources** and providing fully verifiable citations.

**Disclaimer:**  
This tool does **not** give legal advice. It only summarizes official, public information.

---

# 🎯 Target Audience
This assistant is designed for:

- **Prospective international students** applying for the F-1 visa  
- **Parents & guardians** supporting students  
- **High school counselors**  
- **University international student advisors (DSO/ISSO)**  
- **Recent graduates** preparing OPT, STEM OPT, or H-1B filings  

---

# ✨ Key Features
- **Accurate Q&A** using verified government sources  
- **Citation-backed answers** for transparency and trust  
- **Safety layer** blocking legal advice  
- **Modern RAG architecture** built for clarity and efficiency  
- **Optimized for procedural & policy questions**  

---

# 🛠 Architecture & Technology Stack

The agent follows a classic RAG architecture designed for accuracy, clarity, and fast response times.

**Pipeline:**  
`Ingestion → Chunking → Embedding → Vector Store → Retrieval → LLM Generation → Safety Layer`

| Component | Technology / Model | Rationale |
|----------|--------------------|-----------|
| **Language Model (LLM)** | `GPT-4.1 mini` | Excellent reasoning for rules & procedures. |
| **Embeddings** | `OpenAI text-embedding-3-small` | High multilingual accuracy; ideal for dense policy docs. |
| **Vector Database** | `Chroma` | Simple, fast, ideal for this project scale. |
| **Chunking Strategy** | Heading-based | 256–512 token chunks preserve document structure. |
| **Retrieval Strategy** | Vector Search (`top_k=5`) | Effective for structured government pages. |
| **Safety Layer** | Custom rule-based filters | Prevents legal advice & unsafe outputs. |

---

# 🧩 Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│              Immigration RAG Processing Pipeline             │
└─────────────────────────────────────────────────────────────┘

Document Ingestion (PDF → HTML → TXT)
                ↓
Preprocessing & Normalization
                ↓
Heading-Based Chunking (256–512 tokens)
                ↓
Embeddings (OpenAI text-embedding-3-small)
                ↓
Vector Store (ChromaDB)
                ↓
Dense Retrieval (top-5 vector search)
                ↓
LLM Generation (GPT-4.1 mini)
                ↓
Safety Layer (legal-advice filters + refusal policies)
                ↓
Citation Assembly (source + section + link)
                ↓
Final Answer
```

---

# 📈 Success Criteria

```yaml
success:
  accuracy: "≥80% correct answers on evaluation set"
  citations: "100% answers include at least one citation"
  latency: "≤4s average response time"
  safety: "Zero legal-advice violations"
```

---

# 📊 Evaluation Plan

15 questions covering:
- Visa steps  
- Required documents  
- SEVIS & DS-160  
- OPT/STEM OPT rules  
- Travel & reentry  
- H-1B basics  
- Common denial scenarios  

```yaml
evaluation:
  questions: 15
  correct: TBD
  with_citations: TBD
  avg_latency: TBD
  unsafe_outputs: 0
```

---

# 🔮 Future Work & Roadmap

- **Hybrid retrieval** (add BM25 keyword search)  
- **Reranker integration** to improve retrieval precision  
- **Risk classifier** for legal-advice detection  
- **Scheduled re-indexing** each semester  
- **Structured outputs** (checklists for visa tasks)  
- **Web UI** for student-facing deployment  

---

# ⚠️ Risks & Edge Cases
- **Hallucinations:** Possible despite citations  
- **Outdated documents:** Rules change frequently  
- **Misinterpretation:** Users may assume legal authority  
- **Complex formatting:** Some PDFs reduce chunking quality  

---

# 📜 License
MIT License — free for personal and educational use.

---
