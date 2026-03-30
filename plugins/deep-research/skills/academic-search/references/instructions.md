# Academic Search — Detailed Instructions

## Step 1: Understand Research Needs

Analyze the user's query to identify:
- **Primary Keywords**: Core concepts and terms
- **Domain/Field**: Computer Science, Medicine, Physics, etc.
- **Time Frame**: Recent papers, historical, specific year range
- **Paper Type**: Survey, empirical study, theoretical, applied

## Step 2: Select Data Sources

Choose databases based on the research domain:

| Database | Best For | Access Method |
|----------|----------|---------------|
| **arXiv** | CS, Physics, Math, AI/ML | `mcp__arxiv__*` tools |
| **Google Scholar** | Broad coverage, cross-disciplinary | `mcp__paper-search-mcp__search_google_scholar` |
| **PubMed** | Biomedical, Life Sciences | `mcp__paper-search-mcp__search_pubmed` |
| **bioRxiv** | Biology preprints | `mcp__paper-search-mcp__search_biorxiv` |
| **medRxiv** | Medical preprints | `mcp__paper-search-mcp__search_medrxiv` |
| **IEEE Xplore** | Engineering, Electronics | WebSearch with `site:ieeexplore.ieee.org` |
| **ACM Digital Library** | Computer Science | WebSearch with `site:dl.acm.org` |

## Step 3: Execute Search

### arXiv Papers (PRIMARY)

```
mcp__arxiv__search_papers:
  query: "<search terms>"
  max_results: 15-20
  categories: ["cs.AI", "cs.CL", "cs.LG", "cs.CV"]  # adjust per topic
  sort_by: "relevance"
```

### Google Scholar (broad coverage)

```
mcp__paper-search-mcp__search_google_scholar:
  query: "<search terms>"
  max_results: 10
```

### PubMed (biomedical)

```
mcp__paper-search-mcp__search_pubmed:
  query: "<search terms>"
  max_results: 10
```

### bioRxiv / medRxiv (preprints)

```
mcp__paper-search-mcp__search_biorxiv:
  query: "<search terms>"
  max_results: 10
```

### Deep Paper Reading

```
mcp__arxiv__read_paper:
  paper_id: "2301.12345"  # arXiv ID
  # Returns full paper content in markdown format
```

### Other Databases (FALLBACK)

Use WebSearch with domain filtering:
```
WebSearch with allowed_domains: ["ieeexplore.ieee.org"]
WebSearch with allowed_domains: ["dl.acm.org"]
WebSearch with allowed_domains: ["nature.com", "science.org"]
```

## Step 4: Quality Assessment

Rate each paper using the academic quality scale:

| Rating | Criteria |
|--------|----------|
| **A** | Top-tier venue (Nature, Science, NeurIPS, ICML, ACL, CVPR), high citations, rigorous methodology |
| **B** | Good venue, peer-reviewed, solid methodology, moderate citations |
| **C** | Peer-reviewed but lower-tier venue, limited citations |
| **D** | Preprint (arXiv, bioRxiv), workshop paper, not peer-reviewed |
| **E** | Blog post, technical report, white paper |

**Quality Indicators**:
- Citation count relative to paper age
- Venue impact factor / reputation
- Author h-index and institutional affiliation
- Methodology rigor
- Reproducibility (code/data availability)

## Step 5: Format Output

For each relevant paper, provide standardized metadata:

```markdown
### [Paper Title]

**Authors**: Author1, Author2, Author3
**Year**: YYYY
**Venue**: Conference/Journal Name
**Quality Rating**: A/B/C/D/E

**Abstract**: [Brief summary]

**Key Contributions**:
- Contribution 1
- Contribution 2

**Citation**:
- DOI: [if available]
- arXiv: [arXiv ID if applicable]
- URL: [direct link]

**Citation Count**: N citations
**Relevance Score**: High/Medium/Low
```

## Tool Reference

### mcp__arxiv__* Tools (PRIMARY)

- `mcp__arxiv__search_papers`: Search arXiv papers with advanced filtering
- `mcp__arxiv__download_paper`: Download paper PDF
- `mcp__arxiv__read_paper`: Read full paper content in markdown format
- `mcp__arxiv__list_papers`: List available downloaded papers

### arXiv Categories

- `cs.AI` — Artificial Intelligence
- `cs.CL` — Computation and Language (NLP)
- `cs.CV` — Computer Vision
- `cs.LG` — Machine Learning
- `cs.MA` — Multi-Agent Systems
- `cs.NE` — Neural and Evolutionary Computing
- `stat.ML` — Machine Learning (Statistics)

### mcp__paper-search-mcp__* Tools

- `mcp__paper-search-mcp__search_arxiv`: Alternative arXiv search
- `mcp__paper-search-mcp__search_google_scholar`: Search Google Scholar
- `mcp__paper-search-mcp__search_pubmed`: Search PubMed (biomedical)
- `mcp__paper-search-mcp__search_biorxiv`: Search bioRxiv preprints
- `mcp__paper-search-mcp__search_medrxiv`: Search medRxiv preprints
- `mcp__paper-search-mcp__read_arxiv_paper`: Read arXiv paper content
- `mcp__paper-search-mcp__download_arxiv`: Download arXiv PDF

## Search Output Summary

When completing a search, provide:

1. **Search Summary**: Query used, databases searched, total results
2. **Top Papers**: 5-15 most relevant papers with full metadata
3. **Search Recommendations**: Related queries, additional databases to try
4. **Quality Distribution**: How many A/B/C/D/E papers found

## Integration with Deep Research

When called by `research-executor`:
- Focus on papers directly relevant to the research question
- Prioritize recent papers (last 5 years) unless historical context needed
- Include seminal/foundational papers if highly cited
- Provide citation-ready formatted references

## Important Reminders

- Always verify paper URLs are accessible
- Note if papers are behind paywalls
- Distinguish between preprints and peer-reviewed publications
- Check for retractions or corrections on important papers
- Provide enough metadata for proper academic citation
