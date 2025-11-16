# 🔐 Immigration RAG Assistant  
### *A Privacy-Preserving, Locally-Deployed Retrieval-Augmented Generation System for U.S. Immigration Pathways*  
**Local RAG • Trustworthy AI • Secure Information Retrieval**

## 📘 Overview

The **Immigration RAG Assistant** is a security-focused, fully local Retrieval-Augmented Generation (RAG) system that provides accurate, citation-grounded explanations of U.S. immigration rules for F-1 students, OPT/STEM OPT applicants, H-1B candidates, EB-1A extraordinary ability petitions, and related employment-based pathways.

The system is built for **IPHS 391 (AI Security & Governance)** and demonstrates modern principles of **trustworthy AI**, including privacy preservation, provenance tracking, low hallucination risk, and controlled generation via guardrails.

Designed to operate entirely **offline**, this RAG pipeline eliminates privacy concerns associated with cloud-based LLM services while ensuring the reliability and correctness required for high-stakes legal topics.

## 🎯 Motivation

U.S. immigration regulations are:

- Complex and multi-layered  
- Spread across dozens of PDFs and policy manuals  
- Frequently updated  
- High-stakes and high-risk if misinterpreted  
- Often misrepresented or hallucinated by general LLMs

Despite growing interest in AI assistants for immigration, **no existing system provides verifiable, citation-driven, local-only analysis** appropriate for academic or professional settings.

This project addresses that gap.

## 🧩 Objectives

The system aims to:

1. **Provide accurate, explainable answers** grounded in authenticated sources.  
2. **Run 100% locally**, ensuring complete privacy and full academic reproducibility.  
3. **Demonstrate a security-aware RAG pipeline**, aligned with industry best practices.  
4. **Showcase reliable citation pathways** including document title, section, and paragraph.  
5. **Incorporate lightweight guardrails** to mitigate unsafe, incorrect, or over-confident responses.  

## 👥 Intended Users

- International students (F-1/OPT/STEM OPT)  
- Workers navigating H-1B, EB-1A, or L-1 paths  
- University advising offices  
- AI researchers studying safe RAG design  
- Students learning trustworthy AI principles  
- Engineers evaluating local-only RAG feasibility  

## 📚 Corpus Summary

The curated corpus includes ~450–550 pages from authoritative, public sources:

- USCIS Policy Manual (Volumes 1–2)
- DHS/SEVP STEM OPT 24-Month Guidance  
- I-765 and I-129 official instructions  
- 8 CFR §214, §274a  
- EB-1A regulatory criteria: 8 CFR §204.5(h)  
- DHS employment authorization documents  

All documents were converted to normalized `.txt` and `.md` formats, with:

- Boilerplate removal  
- Header/footer elimination  
- Deduplication of repeated policy disclaimers  
- Standardized section headings  
- Paragraph-level metadata assignment  

This produces a clean corpus suitable for high-precision retrieval.

## 🏛️ System Architecture

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
                 Local Embeddings (MiniLM-L6-v2, HF Transformers)
                                    ↓
              Vector Storage (ChromaDB with metadata fields)
                                    ↓
                  Dense Retrieval (Top-5 cosine similarity)
                                    ↓
               Local LLM Inference (Mistral 7B via Ollama)
                                    ↓
          Security Guardrails (jailbreak prevention + safety checks)
                                    ↓
              Citation Assembly (document + section + paragraph)
                                    ↓
                         Final Answer Generation
```

## 🛠️ Technical Stack

| Component | Choice | Justification |
|----------|--------|----------------|
| **Embeddings** | `sentence-transformers/all-MiniLM-L6-v2` | Lightweight, high-quality, fully local |
| **Vector DB** | ChromaDB | Persistent storage, efficient metadata queries |
| **LLM** | Mistral 7B (Ollama) | Fast local inference, strong factual grounding |
| **Guardrails** | NeMo Guardrails + custom refusal rules | Prevent unsafe/legal-advice queries |
| **Chunking** | Heading-aware 512-token segments | Enhances semantic retrieval |
| **Retrieval** | Dense cosine similarity (top-5) | Reliable performance on small corpora |

## 🔒 Security Principles

The system is intentionally designed around **trustworthy AI** concepts:

### **1. Privacy Preservation**
- All computation performed locally  
- No API calls or external telemetry  
- Suitable for sensitive or high-stakes data  

### **2. Hallucination Mitigation**
- Retrieval-required generation  
- Strict citation enforcement  
- Refusal to answer when evidence does not exist  

### **3. Guardrail Enforcement**
- Detection of unsafe, misleading, or legally actionable queries  
- Model responds with:  
  *“I cannot provide legal advice. Here is what the official documentation states…”*  

### **4. Provenance Tracking**
- Each chunk retains:  
  - document title  
  - section heading  
  - paragraph index  
- All answers include these metadata references  

## 📈 Evaluation Design

A curated evaluation set of 15 questions covering:

- OPT → STEM OPT eligibility  
- Cap-gap rules  
- H-1B lottery vs cap-exempt categories  
- EB-1A extraordinary ability criteria  
- L-1A vs L-1B distinctions  
- Authorized employment categories under §274a.12  

### Metrics

| Metric | Target | Description |
|--------|--------|-------------|
| **Accuracy** | ≥ 85% | Human-verified correct answers |
| **Citation Inclusion** | ≥ 90% | Responses include valid, traceable citations |
| **Latency** | ≤ 5 seconds | Average end-to-end runtime |
| **Hallucinated Citations** | 0 | No nonexistent document references |
| **Safety Compliance** | 100% | Unsafe/legal-advice questions refused |

### Expected Baseline Performance
- ~13/15 correct answers  
- ~2.8–3.5 second latency  
- ~0 hallucinated citations  

## ⚠️ Known Limitations

- Long tables in SEVP guidance lose formatting during extraction  
- Acronym-dense sections may require hybrid retrieval  
- Contradictions between older CFR versions and recent SEVP memos  
- Guardrails may over-filter legitimate “edge case” questions  

## 🚀 Future Extensions

- **Hybrid Retrieval:** BM25 + dense vectors  
- **Cross-encoder Reranking:** ColBERT or Cohere ReRank  
- **Conformal RAG (C-RAG):** uncertainty-aware abstention  
- **Change-Tracking:** versioning for yearly immigration rule updates  
- **Evidence Graphs:** visual explanation of document-level evidence  
- **Conversation Memory:** multi-turn session reasoning (safely constrained)  

## 📑 Repository Structure

```
.
├── data/
│   ├── corpus_raw/
│   ├── corpus_clean/
│   └── metadata/
├── src/
│   ├── ingest.py
│   ├── preprocess.py
│   ├── chunker.py
│   ├── embedder.py
│   ├── vectorstore.py
│   ├── retriever.py
│   ├── generator.py
│   └── guardrails.py
├── evaluation/
│   ├── questions.json
│   └── results.md
├── diagrams/
├── README.md
└── CAPB_report.md
```

## 📚 References

- **USCIS Policy Manual** (2025 edition)  
- **DHS/SEVP STEM OPT 24-Month Guidance**  
- **8 CFR §214 and §274a**  
- **8 CFR §204.5(h)** – EB-1A extraordinary ability  
- **I-765 & I-129 form instructions**  
- **NVIDIA NeMo Guardrails Documentation** (2024–2025)  
- Recent RAG trust & robustness literature (2023–2025)

## 👤 Author

**Kirill Sidorko**  
IPHS 391 — AI Security & Governance  
Kenyon College  
