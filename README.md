
<p align="center">
  <h1 align="center">🔐 Immigration RAG Assistant</h1>
  <p align="center"><i>A Privacy-Preserving RAG System for U.S. Student & Work Visa Procedures</i></p>

  <p align="center">
    <img src="https://img.shields.io/badge/Status-In%20Development-yellow.svg" />
    <img src="https://img.shields.io/badge/Python-3.10%2B-blue.svg" />
    <img src="https://img.shields.io/badge/RAG-Retrieval%20Augmented%20Generation-orange.svg" />
    <img src="https://img.shields.io/badge/Focus-Immigration%20AI%20Safety-red.svg" />
  </p>

  <p align="center">
    <a href="#-overview">Overview</a> •
    <a href="#-target-audience">Target Audience</a> •
    <a href="#-key-features">Features</a> •
    <a href="#-architecture--technology-stack">Architecture</a> •
    <a href="#-success-criteria">Success Criteria</a> •
    <a href="#-evaluation-plan">Evaluation</a> •
    <a href="#-future-work--roadmap">Future Work</a> •
    <a href="#-references">References</a> •
    <a href="#-author">Author</a> •
    <a href="#-acknowledgments">Acknowledgments</a>
  </p>
</p>

---

# 🌐 Overview
Navigating U.S. immigration rules is overwhelming for many international students. Information about F-1 status, OPT, STEM OPT, cap-gap extensions, SEVIS fees, I-20 issuance, and visa interview procedures is scattered across dense government websites.

This project implements a **modern Retrieval-Augmented Generation (RAG) agent** that centralizes these procedures into a clear, trustworthy system, retrieving answers exclusively from **official U.S. government sources**. All responses include **verifiable citations**.

**Disclaimer:**  
This tool does **not** give legal advice. It only summarizes official, public information.

---

# 🎯 Target Audience
- Prospective international students  
- Parents & guardians  
- High school counselors  
- University international student offices (DSO/ISSO)  
- Recent graduates preparing OPT, STEM OPT, or H-1B filings  

---

# ✨ Key Features
- Accurate Q&A using verified government sources  
- Citation-backed answers  
- Safety layer preventing legal advice  
- Modern, lightweight RAG architecture  
- Optimized for procedural immigration questions  

---

# 🛠 Architecture & Technology Stack

**Pipeline:**  
`Ingestion → Chunking → Embedding → Vector Store → Retrieval → LLM Generation → Safety Layer`

| Component | Technology / Model | Rationale |
|----------|--------------------|-----------|
| **Language Model (LLM)** | `GPT-4.1 mini` | Strong policy reasoning & clarity. |
| **Embeddings** | `OpenAI text-embedding-3-small` | High multilingual accuracy; ideal for government text. |
| **Vector Database** | `Chroma` | Fast, simple, effective for small corpora. |
| **Chunking Strategy** | Heading-based | 256–512 token chunks with stable semantic separation. |
| **Retrieval Strategy** | Vector Search (`top_k=5`) | Works well for structured policy documents. |
| **Safety Layer** | Custom implementation | Ensures no legal predictions or advice. |

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
  accuracy: "≥80% correct answers"
  citations: "100% answers include at least one citation"
  latency: "≤4s average response time"
  safety: "0 legal-advice violations"
```

---

# 📊 Evaluation Plan

15-question test set covering:
- Visa steps  
- Required documentation  
- SEVIS rules  
- OPT/STEM OPT  
- Travel & reentry  
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
- Hybrid retrieval (BM25 + vectors)  
- Reranking step for improved recall  
- Classifier for high-risk legal questions  
- Automated re-indexing each semester  
- Structured checklists (e.g., “Applying for OPT”)  
- Web UI for broader student-facing use  

---

# ⚠ Risks & Edge Cases
- Possible hallucinations despite citations  
- Rules change frequently → risk of outdated answers  
- Users may misunderstand answers as legal advice  
- Government PDFs vary in structure → chunking inconsistencies  

---

# 📚 References

Key sources informing this project:

- **USCIS Policy Manual**  
- **DHS SEVP – Study in the States**  
- **Department of State (travel.state.gov)**  
- **Code of Federal Regulations (Title 8)**  
- **OpenAI Embeddings Documentation**  
- **Chroma Documentation**  

---

# 👨‍💻 Author

**Kirill Sidorko**  
Student – IPHS 391  
Immigration RAG Assistant Project

---

# 🙏 Acknowledgments

ChatGPT was used for assistance with system design reasoning, formatting, and text clarity.

---
