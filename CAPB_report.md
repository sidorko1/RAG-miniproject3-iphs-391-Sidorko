
# Mini-Project #3: Real-World RAG Implementation  
## CAPB Report — Immigration RAG Assistant  
**Student:** Kirill Sidorko  
**Course:** IPHS 391 — Mini-Project #3  

---

# 1. Project Context & Use Case

## 1.1 Problem / Context
U.S. immigration rules for international students—spanning F-1 requirements, OPT and STEM OPT procedures, cap-gap extensions, and early career transitions such as H-1B—are scattered across several government agencies. Students often rely on unofficial sources, leading to confusion and misinformation.

The core issue is **fragmentation**: the correct information exists, but it is spread across USCIS, DHS SEVP, the Department of State, and university international student offices. Advisors must answer the same questions repeatedly, and students lack a unified, trustworthy reference system.

This project develops a **Retrieval-Augmented Generation (RAG) assistant** that consolidates official U.S. immigration documents and provides **clear, citation-backed answers**. The system emphasizes factual accuracy, safety, and avoidance of legal advice.

---

## 1.2 Primary Use Case
A factual, citation-based Q&A agent for explaining U.S. student and work visa procedures (F-1, OPT, STEM OPT, cap-gap, H-1B, EB-1A).

---

## 1.3 Users / Audience
- International students  
- Parents and guardians  
- High school counselors  
- University advisors (DSO/ISSO)  
- Recent graduates navigating OPT or H-1B  

---

## 1.4 Success Criteria

```yaml
context:
  domain: "U.S. student & work visa procedures (F-1, OPT, STEM OPT, H-1B, EB-1A)"
  use_case: "Citation-backed Q&A over official immigration documents"
  users: ["international students", "parents", "counselors", "university advisors"]
  success:
    - "≥80% correct answers on evaluation"
    - "100% answers include at least one citation"
    - "≤4s latency"
    - "0 legal-advice violations"
```

---

# 2. Data & Constraints

## 2.1 Corpus Details
The corpus consists of **20–30 official U.S. government and university documents**, including:

### Government Sources  
- USCIS Policy Manual  
- DHS SEVP — Study in the States  
- Department of State — F-1 Student Visa pages  
- Form Instructions: I-20, I-765, I-129  
- CFR Title 8 (student & employment categories)

### University Sources  
- International student office guides  
- Checklists, timelines, and FAQs  

### Formats  
- PDF  
- HTML  
- Cleaned TXT/Markdown  

### Preprocessing Steps  
- Remove navigation/UI elements  
- Normalize headings  
- Convert PDFs to text  
- Split into structured semantic sections  

---

## 2.2 Constraints

```yaml
data_constraints:
  sources:
    - "USCIS official pages"
    - "DHS SEVP Study in the States"
    - "Department of State visa pages"
    - "Official OPT and H-1B form instructions"
  formats: ["pdf", "html", "txt"]
  size: "20–30 documents"
  local_only: false
  cost_limit: "low OpenAI API usage"
  latency_target: 4
  security: "public, non-personal data only"
```

Additional considerations:
- Policies change frequently  
- Must avoid legal advice  
- Must warn users to verify using official sources  

---

# 3. RAG Architecture (MVP)

## 3.1 Pipeline Overview
**Ingestion → Chunking → Embedding → Vector Store → Retrieval → GPT-4.1 mini → Safety Layer → Citations**

---

## 3.2 Component Choices

- **Chunking:** heading-based, 256–512 tokens  
- **Embeddings:** OpenAI `text-embedding-3-small`  
- **Vector DB:** Chroma  
- **Retrieval:** dense vector search (`top_k = 5`)  
- **LLM:** GPT-4.1 mini  
- **Safety Layer:** custom rule-based filters  
- **Citations:** link + section + snippet  

---

## 3.3 YAML Architecture

```yaml
architecture_mvp:
  steps: ["ingest", "chunk", "embed", "store", "retrieve", "generate", "safety"]
  chunking: "heading-based, 256–512 tokens"
  embeddings: "OpenAI text-embedding-3-small"
  vector_db: "Chroma"
  retrieval: "vector-only, top_k=5"
  llm: "GPT-4.1 mini"
  citations: true
  safety_layer: "custom rule-based"
  rationale: "Accurate, cost-effective, aligned with structured immigration documents"
```

---

# 4. Component Alternatives (Mini-Bakeoff)

```yaml
component_selection:
  vector_db:
    options: ["FAISS", "Chroma"]
    selected: "Chroma"
    reason: "Simple, persistent, good metadata handling"

  embeddings:
    options: ["OpenAI text-embedding-3-small", "BGE-small"]
    selected: "OpenAI text-embedding-3-small"
    reason: "High performance on long-form policy text"

  retrieval:
    options: ["vector-only", "hybrid (BM25 + vector)"]
    selected: "vector-only"
    reason: "Structured corpus doesn't require hybrid retrieval"

  llm:
    options: ["GPT-4.1 mini", "Local 7B"]
    selected: "GPT-4.1 mini"
    reason: "Better reasoning for procedural questions"

  reranker:
    selected: "None"
    reason: "Not necessary for MVP"
```

---

# 5. Evaluation Plan & Expected Results

## 5.1 Test Set
15 questions covering:
- Visa process  
- Required documentation  
- SEVIS & DS-160  
- OPT/STEM OPT  
- Travel/reentry rules  
- Denial scenarios  

---

## 5.2 Evaluation YAML

```yaml
evaluation:
  questions: 15
  baseline:
    approach: "dense retrieval + GPT-4.1 mini"
    correct: TBD
    partially_correct: TBD
    incorrect: TBD
    with_citations: TBD
    avg_latency: TBD
  notes: "Evaluation pending MVP deployment."
```

---

# 6. Risks, Edge Cases & Future Work

## 6.1 Edge Cases
- Very long government PDFs  
- Country-specific consular differences  
- Sudden USCIS policy changes  
- Ambiguous user questions  

---

## 6.2 Risks
- Hallucinations despite citations  
- Conflicting government pages  
- Users mistaking summaries for legal advice  

---

## 6.3 Future Improvements

```yaml
improvements:
  - "Add BM25 + vector hybrid retrieval"
  - "Add reranker"
  - "Add classifier for legal-advice detection"
  - "Re-index corpus regularly"
  - "Generate structured visa checklists"
  - "Build simple web UI"
```

---

# 7. References
- USCIS Policy Manual  
- DHS SEVP — Study in the States  
- Department of State F-1 Visa pages  
- CFR Title 8  
- OpenAI Embeddings documentation  
- Chroma documentation  

---
