---
name: academic-search
description: This skill should be used when the user asks to "search for papers", "find academic literature on [topic]", "look up research papers", "search arXiv/PubMed/Scholar", or needs scholarly references with quality ratings from arXiv, Google Scholar, PubMed, bioRxiv, or medRxiv.
user-invocable: true
argument-hint: "[search query or research topic]"
allowed-tools:
  - WebSearch
  - WebFetch
  - Read
  - Write
  - Task
  - "mcp__paper-search-mcp__*"
  - "mcp__arxiv__*"
---

# Academic Search

The **Academic Search** skill finds, evaluates, and organizes scholarly literature from multiple databases.

## Search Workflow

1. **Understand needs** — Identify keywords, domain, timeframe, paper type
2. **Select sources** — Choose databases based on domain (see table below)
3. **Execute search** — Use MCP tools with domain-appropriate queries
4. **Assess quality** — Rate each paper A-E
5. **Format output** — Standardized metadata per paper

## Database Selection

| Database | Best For | Tool |
|----------|----------|------|
| arXiv | CS, Physics, Math, AI/ML | `mcp__arxiv__search_papers` |
| Google Scholar | Broad, cross-disciplinary | `mcp__paper-search-mcp__search_google_scholar` |
| PubMed | Biomedical, Life Sciences | `mcp__paper-search-mcp__search_pubmed` |
| bioRxiv | Biology preprints | `mcp__paper-search-mcp__search_biorxiv` |
| medRxiv | Medical preprints | `mcp__paper-search-mcp__search_medrxiv` |
| IEEE/ACM | Engineering, CS | WebSearch with `site:` filtering |

## Quality Ratings

| Rating | Criteria |
|--------|----------|
| **A** | Top-tier venue (Nature, NeurIPS, ICML, ACL), high citations, rigorous methodology |
| **B** | Good venue, peer-reviewed, solid methodology |
| **C** | Lower-tier venue, limited citations |
| **D** | Preprint, workshop paper, not peer-reviewed |
| **E** | Blog, technical report, white paper |

## Output Per Paper

Title, Authors, Year, Venue, Quality Rating, Abstract summary, Key Contributions, DOI/arXiv ID/URL, Citation Count, Relevance Score

## Details

## Additional Resources

### Reference Files
- **[`references/instructions.md`](references/instructions.md)** — Detailed tool parameters, arXiv categories, search strategies

### Examples
- **[`examples/examples.md`](examples/examples.md)** — Academic search examples across domains
