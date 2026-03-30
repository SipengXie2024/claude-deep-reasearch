---
name: got-controller
description: This skill should be used when the user asks to "use graph of thoughts", "optimize research quality", "explore research paths strategically", or when research involves complex multi-faceted topics requiring strategic depth-vs-breadth decisions. Manages graph state with Generate, Aggregate, Refine, Score, and KeepBestN operations.
user-invocable: true
argument-hint: "[research topic for GoT-managed research]"
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
  - "mcp__arxiv__*"
  - "mcp__paper-search-mcp__*"
---

# GoT Controller

The **GoT Controller** orchestrates research as a graph, making strategic decisions about which paths to explore, prune, and combine.

## Core Operations

| Operation | Purpose | When to Use |
|-----------|---------|-------------|
| **Generate(k)** | Create k parallel research paths | Initial exploration, expanding high-quality findings |
| **Aggregate(k)** | Combine k nodes into one synthesis | Related findings exist, need cohesive whole |
| **Refine(1)** | Polish existing finding | Good content needing better organization/citations |
| **Score** | Rate quality 0-10 | After every Generate or Refine |
| **KeepBestN(n)** | Prune to top n nodes | Managing complexity, focusing resources |

## Decision Logic

- **Score ≥ 7.0** → Generate deeper from this node
- **Score 6.0-7.0** → Refine, then re-score
- **Score < 6.0** → Prune immediately
- **Multiple related findings** → Aggregate
- **After 2-3 rounds of generation** → Aggregate strategically

## Academic-First Agent Distribution (for Generate)

- 60-70% Academic Agents (MCP tools: `mcp__arxiv__*`, `mcp__paper-search-mcp__*`)
- 20-30% Web Research Agents (supplementary)
- 10% Verification Agents

## Execution Patterns

**Balanced** (most common): Generate(4) → Score → Deepen best + Refine medium + Prune low → Aggregate → Refine final
**Breadth-First**: Generate(5+) → KeepBestN(3) → Generate(2) from each → Aggregate
**Depth-First**: Generate(3) → Take best → Generate(3) from it → Repeat → Refine

## Graph State

Track in `research_notes/got_graph_state.md`:

```markdown
| Node ID | Summary | Score | Parent | Status |
|---------|---------|-------|--------|--------|
```

## Details

## Additional Resources

### Reference Files
- **[`references/instructions.md`](references/instructions.md)** — Detailed operation implementations, scoring criteria, phase integration

### Examples
- **[`examples/examples.md`](examples/examples.md)** — Complete GoT research execution examples
