---
name: research-executor
description: This skill should be used when the user asks to "run deep research", "research [topic]", "execute research plan", "start the 7-phase research", or has a structured research prompt ready for execution. Deploys parallel multi-agent teams and produces comprehensive, citation-backed reports.
user-invocable: true
argument-hint: "[structured research prompt]"
allowed-tools:
  - Task
  - TeamCreate
  - TeamDelete
  - SendMessage
  - TaskCreate
  - TaskUpdate
  - TaskList
  - TaskGet
  - WebSearch
  - WebFetch
  - Read
  - Write
  - Skill(academic-search)
  - "mcp__arxiv__*"
  - "mcp__paper-search-mcp__*"
---

# Research Executor

The **Deep Research Executor** conducts comprehensive research using the 7-phase methodology and produces citation-backed reports.

## Critical Rules

1. **Academic-First**: ALL agents MUST prioritize MCP academic tools (`mcp__arxiv__*`, `mcp__paper-search-mcp__*`) before web sources.
2. **Team Mode Mandatory for 4+ subtopics**: Use TeamCreate → TaskCreate → spawn teammates → SendMessage coordination → shutdown_request → TeamDelete. Do NOT use Task sub-agents with `run_in_background` as a shortcut.
3. **Sub-Agent Mode for 1-3 subtopics only**: Launch all Task agents in a single response with `run_in_background: true`.
4. **Every factual claim needs a citation**: Author, Date, Title, URL/DOI, Page numbers (if applicable).

## 7-Phase Process (Summary)

| Phase | Action | Key Decision |
|-------|--------|-------------|
| 1. Question Scoping | Verify structured prompt completeness | Ask for clarification if needed |
| 2. Retrieval Planning | Decompose into 3-7 subtopics, create plan | Present plan for approval |
| 3. Iterative Querying | Deploy agents (Team or Sub-Agent mode) | 60-70% academic, 20-30% web, 10% verification |
| 4. Source Triangulation | Cross-validate findings, A-E quality ratings | Resolve contradictions |
| 5. Knowledge Synthesis | Write sections with inline citations | Theme-based, not agent-based |
| 6. Quality Assurance | Chain-of-Verification for key claims | 2-3 independent sources for critical claims |
| 7. Output & Packaging | Generate `RESEARCH/[topic]/` directory | README, executive summary, full report, sources, appendices |

## Mode Selection

| Subtopics | Mode | Enforcement |
|-----------|------|-------------|
| 1-3 | Task Sub-Agents | Allowed |
| **4+** | **Agent Teams (TeamCreate)** | **REQUIRED** |
| Fallback | Task Sub-Agents | ONLY if TeamCreate fails with error |

## Source Quality Ratings

- **A**: Peer-reviewed RCTs, systematic reviews, meta-analyses
- **B**: Cohort studies, clinical guidelines, reputable analysts
- **C**: Expert opinion, case reports, white papers
- **D**: Preprints, conference abstracts, blogs
- **E**: Anecdotal, theoretical, speculative

## Details

## Additional Resources

### Reference Files
- **[`references/instructions.md`](references/instructions.md)** — Detailed phase actions, agent templates, deployment strategies, output templates

### Examples
- **[`examples/examples.md`](examples/examples.md)** — Complete research execution examples
