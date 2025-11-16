# Mini-Project #3: Real-World RAG Implementation
## Immigration RAG Assistant (OPT, STEM OPT, H-1B, EB-1A)
**Student:** Kirill Sidorko  

---

# 1. Project Context & Use Case

## 1.1 Problem / Context
U.S. immigration procedures—especially for students—are difficult to navigate. Rules for OPT, STEM OPT, cap-gap, H-1B filings, and even basic F-1 regulations are spread across USCIS, DHS/SEVP, and various PDF instruction packets. Students constantly ask the same questions, and most do not know which sources are reliable.

This project builds a **small RAG system** that answers common questions about U.S. student and work visa processes using **only official public documents**.  
The system explains steps, summarizes rules, and cites retrieved content. It does **not** give legal advice or personalized guidance.

---

## 1.2 Primary Use Case
Q&A over immigration rules using official government text.

---

## 1.3 Users / Audience
- International students (F-1, OPT, STEM OPT)  
- University advisors  
- Recent graduates preparing H-1B  
- Individuals researching EB-1A or L-1  

---

## 1.4 Success Criteria

```yaml
context:
  domain: "U.S. immigration (OPT, STEM OPT, H-1B, EB-1A)"
  use_case: "Q&A over official rules with citations"
  users: ["international students", "university advisors", "early-career workers"]
  success:
    - "≥85% correct answers"
    - "≥90% answers include at least one citation"
    - "≤4s latency"
    - "no personalized legal advice"
```

---

# 2. Data & Constraints

## 2.1 Corpus Details
Sources include:
- USCIS Policy Manual (Vol. 1 & 2)  
- DHS SEVP “Study in the States”  
- I-765 instructions (OPT)  
- I-129 instructions (H-1B)  
- 8 CFR §§214, 274a, 204.5(h)  

Formats: **PDF**, **HTML**, **TXT**  
Size: **20–30 documents**

---

## 2.2 Constraints

```yaml
data_constraints:
  sources:
    - "USCIS Policy Manual"
    - "SEVP Study in the States"
    - "DHS STEM OPT rule"
    - "Official instructions (I-20, I-765, I-129)"
  formats: ["pdf", "html", "md"]
  size: "20–30 docs"
  local_only: true
  cost_limit: "free / open-source only"
  latency_target: 4
  security: "public sources only, no private data"
```

Additional constraints:
- Must warn users to check updated rules  
- Must run locally for privacy  
- Must avoid legal advice  

---

# 3. RAG Architecture (MVP)

## 3.1 Pipeline
Ingestion → Cleaning → Chunking → Embedding → Vector Store → Retrieval → Local LLM → Safety Filter → Citation

---

## 3.2 Design Choices

```yaml
architecture_mvp:
  steps: ["ingest", "clean", "chunk", "embed", "store", "retrieve", "generate", "postprocess"]
  chunking: "heading-based, ~512 tokens"
  embeddings: "MiniLM-L6-v2 (local)"
  vector_db: "Chroma"
  retrieval: "vector-only, top_k=5"
  llm: "Mistral 7B (Ollama)"
  citations: true
  safety_layer: true
  rationale: "Simple, local, reliable pipeline for immigration documents"
```

---

# 4. Component Alternatives (Mini-Bakeoff)

```yaml
component_selection:
  vector_db:
    options: ["FAISS", "Chroma"]
    selected: "Chroma"
    reason: "straightforward setup + metadata support"
  embeddings:
    options: ["MiniLM", "OpenAI"]
    selected: "MiniLM"
    reason: "free, local, no API required"
  llm:
    options: ["Mistral 7B", "GPT-4.1 mini"]
    selected: "Mistral 7B"
    reason: "offline + privacy-preserving"
  retrieval:
    options: ["vector-only", "hybrid"]
    selected: "vector-only"
    reason: "corpus is small and structured"
  reranker:
    selected: "None"
    reason: "not needed for MVP"
```

---

# 5. Evaluation Plan

## 5.1 Test Set
15 questions covering:
- OPT eligibility  
- STEM OPT rules  
- Cap-gap extension  
- H-1B basics  
- EB-1A evidence categories  
- Travel & employment restrictions  

---

## 5.2 Metrics

```yaml
evaluation:
  questions: 15
  baseline:
    approach: "dense retrieval + local LLM"
    correct: TBD
    partially_correct: TBD
    incorrect: TBD
    with_citations: TBD
    avg_latency: TBD
  notes: "Evaluation completed after pipeline is finalized."
```

---

# 6. Risks, Edge Cases & Future Work

## 6.1 Edge Cases
- Long DHS/USCIS PDFs with tables  
- Conflicting information across documents  
- Policy changes  
- Ambiguous user questions  

---

## 6.2 Risks
- Potential hallucinations  
- Incorrect interpretation of legal text  
- Misleading users if data becomes outdated  

---

## 6.3 Future Improvements

```yaml
improvements:
  - "add hybrid retrieval"
  - "add reranker"
  - "add version-aware retrieval"
  - "add safety classifier for legal-risk questions"
  - "generate one-page checklists for common visa tasks"
```

---

# 7. References
- USCIS (uscis.gov)  
- DHS SEVP (studyinthestates.dhs.gov)  
- CFR Title 8 (eCFR)  
- Mistral documentation  
- SentenceTransformers documentation  
- Chroma documentation  
