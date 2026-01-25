# Academic Search Examples

## Example 1: AI/ML Paper Search

**User Request**:
```
/academic-search transformer attention mechanism neural networks
```

**Expected Workflow**:

1. **Analyze Query**:
   - Primary Keywords: transformer, attention mechanism
   - Domain: Computer Science, Machine Learning
   - Paper Type: Technical/Empirical

2. **Search Execution**:
   - Use `mcp__arxiv__search_papers` with categories `cs.LG, cs.CL, cs.AI`
   - Use `mcp__paper-search__search` for broader coverage
   - WebSearch for Google Scholar results

3. **Sample Output**:

```markdown
## Academic Search Results: Transformer Attention Mechanism

**Search Summary**:
- Databases searched: arXiv, Semantic Scholar, Google Scholar
- Total papers found: 47
- Filtered to top 10 most relevant

---

### 1. Attention Is All You Need

**Authors**: Vaswani, A., Shazeer, N., Parmar, N., et al.
**Year**: 2017
**Venue**: NeurIPS 2017
**Quality Rating**: A

**Abstract**: The dominant sequence transduction models are based on complex recurrent or convolutional neural networks that include an encoder and a decoder. We propose a new simple network architecture, the Transformer, based solely on attention mechanisms...

**Key Contributions**:
- Introduced the Transformer architecture
- Self-attention mechanism for sequence modeling
- Achieved state-of-the-art on machine translation

**Citation**:
- arXiv: 1706.03762
- URL: https://arxiv.org/abs/1706.03762

**Citation Count**: 95,000+
**Relevance Score**: High (Foundational paper)

---

### 2. BERT: Pre-training of Deep Bidirectional Transformers

**Authors**: Devlin, J., Chang, M.W., Lee, K., Toutanova, K.
**Year**: 2019
**Venue**: NAACL 2019
**Quality Rating**: A

**Abstract**: We introduce a new language representation model called BERT, which stands for Bidirectional Encoder Representations from Transformers...

**Key Contributions**:
- Bidirectional pre-training for language understanding
- Introduced MLM and NSP tasks
- Set new SOTA on 11 NLP tasks

**Citation**:
- arXiv: 1810.04805
- URL: https://arxiv.org/abs/1810.04805

**Citation Count**: 75,000+
**Relevance Score**: High

---

### Quality Distribution
- A-rated: 4 papers
- B-rated: 3 papers
- C-rated: 2 papers
- D-rated (preprints): 1 paper

### Recommended Follow-up Searches
- "efficient transformer attention"
- "linear attention mechanism"
- "vision transformer ViT"
```

---

## Example 2: Medical Literature Search

**User Request**:
```
/academic-search CRISPR gene therapy clinical trials 2023
```

**Expected Workflow**:

1. **Analyze Query**:
   - Primary Keywords: CRISPR, gene therapy, clinical trials
   - Domain: Biomedical, Medicine
   - Time Frame: 2023

2. **Search Execution**:
   - Primary: WebSearch with `site:pubmed.ncbi.nlm.nih.gov`
   - Secondary: `mcp__paper-search__search` for cross-references
   - Tertiary: WebSearch with `site:clinicaltrials.gov` for trial data

3. **Sample Output**:

```markdown
## Academic Search Results: CRISPR Gene Therapy Clinical Trials 2023

**Search Summary**:
- Databases searched: PubMed, Semantic Scholar, ClinicalTrials.gov
- Publication year filter: 2023
- Total papers found: 23

---

### 1. CRISPR-Cas9 Gene Editing for Sickle Cell Disease

**Authors**: Frangoul, H., et al.
**Year**: 2023
**Venue**: New England Journal of Medicine
**Quality Rating**: A

**Abstract**: This study reports on the outcomes of CRISPR-Cas9 gene editing therapy in patients with transfusion-dependent sickle cell disease...

**Key Findings**:
- 93% of patients achieved transfusion independence
- Durable responses at 24-month follow-up
- Acceptable safety profile

**Citation**:
- DOI: 10.1056/NEJMoa2309XXX
- PMID: 37XXXXXX
- URL: https://pubmed.ncbi.nlm.nih.gov/37XXXXXX/

**Citation Count**: 150+
**Relevance Score**: High

---

### Quality Distribution
- A-rated (RCTs, top journals): 5 papers
- B-rated (cohort studies): 8 papers
- C-rated (case reports): 6 papers
- D-rated (preprints): 4 papers
```

---

## Example 3: Integration with Deep Research

When called by `research-executor` during Phase 3:

**Research Topic**: "Impact of Large Language Models on Software Development"

**Academic Search Agent Prompt**:
```
Search for academic papers on:
1. LLM code generation effectiveness
2. AI pair programming productivity studies
3. Security implications of AI-generated code
4. Developer experience with AI coding assistants

Prioritize:
- Peer-reviewed publications (2021-2024)
- Papers with empirical studies
- Highly cited foundational works
```

**Output Integration**:
The academic search results are saved to:
```
RESEARCH/llm-software-development/
├── sources/
│   ├── academic_papers.md      # Full paper list with metadata
│   └── source_quality_table.md # Quality ratings
└── research_notes/
    └── academic_agent_findings.md  # Key findings summary
```

---

## Example 4: Citation Chain Analysis

**User Request**:
```
/academic-search "chain of thought prompting" --include-citations
```

**Additional Steps**:
1. Find the original paper
2. Use `mcp__paper-search__get_citations` to find papers citing it
3. Use `mcp__paper-search__get_references` to find foundational works
4. Build a citation network view

**Sample Citation Chain Output**:

```markdown
## Citation Chain Analysis: Chain of Thought Prompting

### Foundational Paper
- **Chain-of-Thought Prompting Elicits Reasoning in Large Language Models**
  - Wei et al., 2022, NeurIPS
  - Citations: 3,500+

### Key Citing Papers (Selected)
1. Self-Consistency Improves Chain of Thought Reasoning (Wang et al., 2023)
2. Tree of Thoughts: Deliberate Problem Solving (Yao et al., 2023)
3. ReAct: Synergizing Reasoning and Acting (Yao et al., 2023)

### Referenced By This Paper
1. Language Models are Few-Shot Learners (Brown et al., 2020)
2. Scaling Laws for Neural Language Models (Kaplan et al., 2020)

### Research Trajectory
CoT → Self-Consistency → Tree of Thoughts → Graph of Thoughts
```

---

## Tool-Specific Examples

### arXiv MCP Tools

```
# Search for recent papers
mcp__arxiv__search_papers(
  query="large language model reasoning",
  max_results=15,
  categories=["cs.CL", "cs.AI"]
)

# Get paper details
mcp__arxiv__get_paper_details(
  paper_id="2201.11903"
)
```

### Paper Search MCP Tools

```
# Search Semantic Scholar
mcp__paper-search__search(
  query="prompt engineering techniques",
  limit=20,
  year="2023-2024"
)

# Get citations for a paper
mcp__paper-search__get_citations(
  paper_id="649def34f8be52c8b66281af98ae884c09aef38b"
)
```

### WebSearch for Specific Databases

```
# PubMed search
WebSearch(
  query="machine learning drug discovery",
  allowed_domains=["pubmed.ncbi.nlm.nih.gov"]
)

# IEEE search
WebSearch(
  query="neural network hardware acceleration",
  allowed_domains=["ieeexplore.ieee.org"]
)
```
