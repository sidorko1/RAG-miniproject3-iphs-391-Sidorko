
# Mini-Project #3: Real-World RAG Implementation
## 🔐 Immigration RAG Assistant (F-1, OPT, STEM OPT, H-1B, EB-1A)
**Student: Kirill Sidorko**

---

# 1. Project Context & Use Case

## 1.1 Problem / Context
U.S. immigration rules for students and early-career workers are difficult to navigate. F-1 maintenance requirements, OPT/STEM OPT rules, I-20 procedures, SEVIS obligations, and H-1B filing steps are spread across USCIS, DHS/SEVP, the Department of State, and multiple PDF instruction packets. The information is fragmented, inconsistent across websites, and often misinterpreted.

This project builds a **Retrieval-Augmented Generation (RAG) assistant** that helps international students and advisors understand official immigration procedures.  
The agent retrieves information **strictly from public, official sources** and summarizes it with citations. It does **not** offer personalized legal advice.

---

## 1.2 Primary Use Case
High-level Q&A about immigration processes using official U.S. government documentation.

---

## 1.3 Users / Audience
- Prospective and current international students  
- University international student advisors (DSOs)  
- Recent graduates preparing H-1B  
- Workers researching EB-1A or L-1 pathways  
- Parents and counselors seeking accurate information  

---

## 1.4 Success Criteria

```yaml
context:
  domain: "U.S. immigration and student visa procedures"
  use_case: "Q&A using official government text with verified citations"
  users: ["international students", "university advisors", "recent graduates"]
  success:
    - "≥85% correct answers"
    - "100% answers include at least one citation"
    - "≤4s latency"
    - "no personalized legal advice"
```

---

# 2. Data & Constraints

## 2.1 Corpus Details
Sources include:
- USCIS Policy Manual (Volume 1 & 2 sections relevant to students & employment)  
- DHS SEVP “Study in the States”  
- Department of State F-1 visa pages  
- I-765 instructions (OPT)  
- I-129 instructions (H-1B)  
- 8 CFR §§214, 274a, 204.5(h) (EB-1A)  

Formats: **HTML**, **PDF**, **TXT**  
Corpus size: **20–30 documents**

---

## 2.2 Constraints

```yaml
data_constraints:
  sources:
    - "USCIS Policy Manual"
    - "DHS SEVP Study in the States"
    - "Department of State student visa pages"
    - "I-765 and I-129 Instructions"
  formats: ["html", "pdf", "md"]
  size: "20–30 documents"
  local_only: false
  cost_limit: "low-cost or free API credits"
  latency_target: 4
  security: "public sources only; no personal data"
```

Additional constraints:
- System must warn users to verify with official sources  
- Must avoid legal advice  
- Policies change frequently → RAG must pull from authoritative text only  

---

# 3. 🛠️ Architecture and Technology Stack

The agent follows a classic RAG pipeline designed for accuracy, clarity, and fast response times.

**Pipeline:**  
`Ingestion → Chunking → Embedding → Vector Store → Retrieval → LLM Generation → Safety Layer`

| Component | Technology / Model | Rationale |
|----------|--------------------|-----------|
| **Language Model (LLM)** | `GPT-4.1 mini` | Excellent policy reasoning and explanation quality. |
| **Embeddings** | `OpenAI text-embedding-3-small` | Strong multilingual performance + efficient. |
| **Vector Database** | `Chroma` | Simple, fast, and ideal for small to mid-size datasets. |
| **Chunking Strategy** | Heading-based | 256–512 token chunks preserve document structure. |
| **Retrieval Strategy** | Vector search (`top_k=5`) | Effective for highly structured government text. |
| **Safety Layer** | Custom rule-based filters | Prevents legal advice and unsafe outputs. |

---

# 4. 🏗 Architecture (MVP)

```yaml
architecture_mvp:
  steps: ["ingest", "clean", "chunk", "embed", "store", "retrieve", "generate", "safety"]
  chunking: "heading-based, 256–512 tokens"
  embeddings: "OpenAI text-embedding-3-small"
  vector_db: "Chroma"
  retrieval: "vector-only, top_k=5"
  llm: "GPT-4.1 mini"
  citations: true
  safety_layer: "custom rule-based"
  rationale: "Designed for clarity, accuracy, and safe explanations of immigration rules"
```

---

# 5. ⚖️ Component Alternatives (Mini-Bakeoff)

```yaml
component_selection:
  vector_db:
    options: ["FAISS", "Chroma"]
    selected: "Chroma"
    reason: "simple setup + good metadata handling"
  embeddings:
    options: ["OpenAI text-embedding-3-small", "BGE-small"]
    selected: "OpenAI text-embedding-3-small"
    reason: "better multilingual accuracy and stable performance"
  llm:
    options: ["GPT-4.1 mini", "local 7B model"]
    selected: "GPT-4.1 mini"
    reason: "superior reasoning for procedural questions"
  retrieval:
    options: ["vector-only", "hybrid"]
    selected: "vector-only"
    reason: "corpus is structured and consistent"
  reranker:
    selected: "None"
    reason: "not needed for MVP"
```

---

# 6. 📈 Evaluation Plan

## 6.1 Test Set
15 questions covering:
- OPT eligibility  
- STEM OPT employer requirements  
- Cap-Gap rules  
- H-1B general eligibility  
- EB-1A extraordinary ability criteria  
- Travel/maintenance of status  
- Employment restrictions (CFR 274a)  

---

## 6.2 Metrics

```yaml
evaluation:
  questions: 15
  baseline:
    correct: TBD
    partially_correct: TBD
    incorrect: TBD
    with_citations: TBD
    avg_latency: TBD
  notes: "To be completed when the pipeline is implemented."
```

---

# 7. ⚠️ Risks, Edge Cases & Future Work

## 7.1 Edge Cases
- Conflicting information between CFR and USCIS guidance  
- Long PDFs with tables may reduce chunk quality  
- Policy updates require periodic corpus refresh  
- Ambiguous questions (e.g., personal visa situations)  

---

## 7.2 Risks
- Hallucination risk if retrieval is weak  
- Potential misinterpretation of legal text  
- Users may assume legal authority  

---

## 7.3 Future Improvements

```yaml
improvements:
  - "add hybrid retrieval (BM25 + dense)"
  - "add reranker"
  - "add uncertainty estimation"
  - "add version-aware retrieval (year-specific)"
  - "auto-generate checklists for common visa tasks"
```

---

# 8. 📖 References
- USCIS (uscis.gov)  
- DHS SEVP Study in the States  
- Department of State (travel.state.gov)  
- CFR Title 8  
- ChromaDB documentation  
- OpenAI embeddings documentation  
- Awesome RAG GitHub lists  

---
