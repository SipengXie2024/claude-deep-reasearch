---
name: citation-validator
description: This skill should be used when the user asks to "validate citations", "check my sources", "verify references", "audit research quality", or before finalizing, reviewing, or publishing any research report. Ensures every factual claim has a verifiable source with A-E quality rating.
user-invocable: true
argument-hint: "[research report file or directory]"
allowed-tools:
  - WebSearch
  - WebFetch
  - Read
  - Write
  - "mcp__arxiv__*"
  - "mcp__paper-search-mcp__*"
---

# Citation Validator

The **Citation Validator** is the last line of defense against misinformation and hallucinations in research reports.

## Citation Requirements

Every citation MUST include:
1. **Author/Organization**
2. **Publication Date** (YYYY)
3. **Source Title**
4. **URL/DOI**
5. **Page Numbers** (if applicable)

## Source Quality Ratings

- **A**: Peer-reviewed journals, meta-analyses, RCTs, government regulatory bodies
- **B**: Cohort studies, clinical guidelines, reputable analysts (Gartner, Forrester)
- **C**: Expert opinion, case reports, white papers, reputable news
- **D**: Preprints, conference abstracts, blogs
- **E**: Anonymous, biased, outdated, broken links

## 7-Step Validation Process

1. **Claim Detection** — Identify all factual claims (statistics, dates, specs, market data, cause-effect)
2. **Citation Presence** — Verify each claim has a citation
3. **Completeness** — Check all 5 required elements exist
4. **Quality Assessment** — Assign A-E rating
5. **Accuracy Verification** — Use WebSearch/WebFetch/MCP tools to verify source supports the claim
6. **Hallucination Detection** — Flag: missing citations, dead URLs, unsupported claims, generic sources
7. **Chain-of-Verification** — For critical claims (medical/legal/financial): verify with 2-3 independent sources

## Success Criteria

- 100% of factual claims have citations
- 100% of citations are complete
- 95%+ of citations are accurate
- No unexplained hallucinations
- Overall quality score ≥ 8/10

## Details

## Additional Resources

### Reference Files
- **[`references/instructions.md`](references/instructions.md)** — Detailed scoring formulas, domain-specific considerations, common problem patterns

### Examples
- **[`examples/examples.md`](examples/examples.md)** — Citation validation examples
