
# Mini-Project #3: Real-World RAG Implementation  
## RAG Agent for Student Visa & Study Abroad Procedures  
**Student:** Kirill Sidorko

---

# 1. Project Context & Use Case

## 1.1 Problem / Context
International students face confusing steps when applying to study abroad. F-1 visa procedures, I-20 rules, SEVIS payments, DS-160 instructions, and consular interview requirements are scattered across numerous U.S. government and university websites. Most students struggle to identify which sources are official and trustworthy, resulting in repeated questions and misinformation.

This project develops a **Retrieval-Augmented Generation (RAG) assistant** that answers common questions about the F-1 visa and study-abroad process using only **official sources** (Department of State, DHS SEVP, USCIS, university ISS offices). The assistant summarizes verified content, provides citations, and delivers accurate, high-level procedural guidance. It never offers legal advice or outcome predictions.

---

## 1.2 Primary Use Case
Citation-backed Q&A on F-1 visa rules, study-abroad requirements, timelines, and documentation steps.

---

## 1.3 Users / Audience
- Prospective international students  
- Parents & guardians  
- High school counselors  
- University international student advisors  

---

## 1.4 Success Criteria

```yaml
context:
  domain: "Student visa and study abroad procedures (F-1)"
  use_case: "Q&A over official policy and application steps"
  users: ["international students", "parents", "school counselors"]
  success:
    - "≥80% correct answers on evaluation set"
    - "100% of answers have at least one citation"
    - "≤4s average latency"
    - "No answers provide personalized legal advice"
```

---

# 2. Data & Constraints

## 2.1 Corpus Details
Sources include:  
- U.S. Department of State F-1 visa pages  
- DHS SEVP Study in the States  
- USCIS student-related pages  
- University international student office guides  
- NGO study-abroad documentation  

Formats: **HTML**, **PDF**  
Size: **20–40 documents**

Preprocessing steps:  
- Remove navigation UI and boilerplate  
- Normalize headings  
- Clean and convert PDF text  
- Split pages into clear, policy-linked sections  

---

## 2.2 Constraints

```yaml
data_constraints:
  sources:
    - "Official U.S. F-1 visa pages"
    - "SEVP and USCIS student pages"
    - "University international student office guides"
  formats: ["html", "pdf"]
  size: "20–40 docs"
  local_only: false
  cost_limit: "low cost or free credits"
  latency_target: 4
  security: "no private data, public sources only"
```

Additional constraints:
- Policies change frequently  
- Users require simple, clear responses  
- System must warn users to verify through official links  

---

# 3. RAG Architecture (MVP)

## 3.1 Pipeline
**Ingestion → Chunking → Embedding → Vector Store → Retrieval → LLM Generation → Safety Layer**

---

## 3.2 Design Choices

```yaml
architecture_mvp:
  steps: ["ingest", "chunk", "embed", "store", "retrieve", "generate", "postprocess"]
  chunking: "heading-based, 256–512 tokens with small overlap"
  embeddings: "OpenAI text-embedding-3-small"
  vector_db: "Chroma"
  retrieval: "vector-only top_k=5"
  llm: "GPT-4.1 mini"
  citations: true
  safety_layer: true
  rationale: "Simple pipeline with enough accuracy for structured visa documents"
```

---

# 4. Component Alternatives (Mini-Bakeoff)

```yaml
component_selection:
  vector_db:
    options: ["FAISS", "Chroma"]
    selected: "Chroma"
    reason: "simple local setup"

  embeddings:
    options: ["OpenAI", "BGE-small"]
    selected: "OpenAI"
    reason: "strong multilingual accuracy"

  retrieval:
    options: ["vector-only", "hybrid"]
    selected: "vector-only"
    reason: "good enough for structured text"

  llm:
    options: ["GPT-4.1 mini", "local 7B"]
    selected: "GPT-4.1 mini"
    reason: "better policy reasoning"

  reranker:
    selected: "None"
    reason: "time limit"
```

---

# 5. Evaluation Plan & Expected Results

## 5.1 Test Set
Includes 15 questions covering:  
- Visa steps  
- Required documents  
- SEVIS & DS-160  
- Timing  
- Travel rules  
- Common denial scenarios  

---

## 5.2 Evaluation Format

```yaml
evaluation:
  questions: 15
  baseline:
    approach: "vector-only"
    correct: TBD
    partially_correct: TBD
    incorrect_or_unsafe: TBD
    with_citations: TBD
    avg_latency: TBD
  notes: "Evaluation will occur after system implementation."
```

---

# 6. Risks, Edge Cases & Future Work

## Edge Cases
- Long government PDFs  
- Country-specific differences  
- Sudden policy updates  
- Students with complex histories  

---

## Risks
- LLM hallucinations  
- Outdated information  
- User misinterpretation as legal guidance  

---

## Future Work

```yaml
improvements:
  - "add hybrid retrieval"
  - "add reranker"
  - "add classifier for legal-risk questions"
  - "schedule reindexing each semester"
  - "output structured checklists for common visa steps"
```

---

# 7. References
- U.S. Department of State F-1 Visa pages  
- SEVP “Study in the States”  
- USCIS Student Guide  
- University ISS office guides  
- Chroma documentation  
- OpenAI embeddings documentation  

---

