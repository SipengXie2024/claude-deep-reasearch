---
name: synthesizer
description: This skill should be used when the user asks to "synthesize findings", "combine research results", "merge agent outputs", "resolve contradictions in research", or when multiple research agents have completed and their findings need integration into a unified report.
user-invocable: true
argument-hint: "[research findings directory or files]"
allowed-tools:
  - Read
  - Write
  - Task
  - SendMessage
  - TaskUpdate
  - TaskList
  - TaskGet
  - "mcp__arxiv__*"
  - "mcp__paper-search-mcp__*"
---

# Synthesizer

The **Synthesizer** transforms raw research data into knowledge — integrating, contextualizing, and illuminating, not just summarizing.

## Critical Rules

1. **Organize by theme, not by agent** — group related findings together regardless of source agent.
2. **Preserve all citations** — never introduce claims without source attribution.
3. **Progressive Synthesis** (Team Mode) — begin as soon as 2+ agents complete; continuously update.

## 5-Phase Synthesis Process

1. **Review & Organize** — Read all findings, identify themes, note contradictions, assess credibility
2. **Consensus Building** — Strong (3+ sources), Moderate (2), Weak (1), No Consensus (contradictory)
3. **Contradiction Resolution** — 4 types: Numerical discrepancies, Causal claims, Temporal changes, Scope differences
4. **Structured Synthesis** — Executive Summary → themed sections → Integrated Analysis → Gaps → Conclusions
5. **Quality Enhancement** — Verify completeness, citation accuracy, narrative flow, actionable insights

## Contradiction Resolution

| Type | Strategy |
|------|----------|
| Numerical | Check dates/methodology/scope, present range |
| Causal | Prioritize RCTs, use "evidence suggests" |
| Temporal | Present as trend, use newer data for current state |
| Scope | Contextualize both, explain conditions |

## Quality Score (0-10)

Coverage (0-2) + Coherence (0-2) + Accuracy (0-2) + Insight (0-2) + Clarity (0-2)

## Details

## Additional Resources

### Reference Files
- **[`references/instructions.md`](references/instructions.md)** — Synthesis techniques, output format templates, common patterns

### Examples
- **[`examples/examples.md`](examples/examples.md)** — Complete synthesis examples
