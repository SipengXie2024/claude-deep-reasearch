# Deep Research Implementation with Graph of Thoughts

## Overview

Graph of Thoughts (GoT) implementation for deep research using Task agents. This document focuses on GoT-specific concepts and execution patterns. For project-level configuration (commands, output structure, citation standards), see `CLAUDE.md`.

## Understanding Graph of Thoughts

Graph of Thoughts is a reasoning framework where:
- **Thoughts = Nodes**: Each research finding or synthesis is a node
- **Edges = Dependencies**: Connect parent thoughts to children
- **Transformations**: Operations that create (Generate), merge (Aggregate), or improve (Refine) thoughts
- **Scoring**: Every thought is evaluated 0-10 for quality
- **Pruning**: Low-scoring branches are abandoned
- **Frontier**: Active nodes available for expansion

The system explores multiple research paths in parallel, scores them, and finds optimal solutions through graph traversal.

## The 7-Phase Deep Research Process

> All outputs go to `RESEARCH/[project_name]/`. Break large documents into smaller files to avoid context limits. Track task completion using TaskCreate/TaskUpdate.

### Phase 1: Question Scoping
- Clarify the research question with the user
- Define output format and success criteria
- Identify constraints and desired tone

### Phase 2: Retrieval Planning
- Break main question into subtopics
- Generate specific search queries
- Select appropriate data sources
- Use GoT to model the research as a graph of operations

### Phase 3: Iterative Querying
- Execute searches systematically
- Apply GoT operations for complex reasoning
- Use multiple search modalities (academic MCP tools, web search, web fetch)

### Phase 4: Source Triangulation
- Compare findings across multiple sources
- Validate claims with cross-references
- Use GoT scoring functions to evaluate information quality

### Phase 5: Knowledge Synthesis
- Structure content logically with inline citations
- Use GoT to optimize information organization

### Phase 6: Quality Assurance
- Check for hallucinations and errors
- Apply Chain-of-Verification techniques
- Use GoT ground truth operations for validation

### Phase 7: Output & Packaging
- Format for optimal readability
- Include executive summary and proper bibliography

## Core Concepts

### Graph Structure
- Each research finding is a node with a unique ID
- Nodes have scores (0-10) indicating quality
- Edges connect parent thoughts to child thoughts
- The frontier contains active nodes for expansion

### Transformation Operations
- **Generate(k)**: Create k new thoughts from a parent
- **Aggregate(k)**: Merge k thoughts into one stronger thought
- **Refine(1)**: Improve a thought without adding new content
- **Score**: Evaluate thought quality
- **KeepBestN(n)**: Prune to keep only top n nodes per level

### Research Quality Metrics
- Citation density and accuracy
- Source credibility
- Claim verification
- Comprehensiveness
- Logical coherence

## Implementation Tools

### Core Tools:
1. **WebSearch**: Built-in web search
2. **WebFetch**: Extract and analyze content from URLs
3. **Read/Write**: Manage research documents
4. **Task**: Spawn autonomous agents or teammates
5. **TeamCreate/TeamDelete**: Manage coordinated research teams
6. **SendMessage**: Inter-agent communication within teams
7. **TaskCreate/TaskUpdate/TaskList/TaskGet**: Shared task tracking

### MCP Academic Tools (Priority Order):
1. **mcp__arxiv__search_papers** — arXiv (CS, Physics, Math, AI/ML)
2. **mcp__paper-search-mcp__search_google_scholar** — Broad academic coverage
3. **mcp__paper-search-mcp__search_pubmed** — Biomedical
4. **mcp__paper-search-mcp__search_biorxiv** — Biology preprints
5. **mcp__paper-search-mcp__search_medrxiv** — Medical preprints
6. **mcp__arxiv__read_paper** — Full paper content
7. **mcp__playwright__*** — Browser automation for dynamic web content

## GoT Research Strategy

The system implements GoT using Task agents as transformation operations. A controller agent maintains the graph state and deploys specialized agents.

### Multi-Agent Deployment

#### Step 1: Create Research Plan
Break down the research question into 3-8 subtopics.

#### Step 2: Launch Parallel Agents
Use multiple Task invocations in a single response. Each agent receives:
- Clear description of research focus
- Mandatory MCP tool usage instructions
- Expected output format and citation requirements

#### Step 3: Coordinate Results
- Compile findings, identify overlaps/contradictions
- Synthesize into coherent narrative with source attribution

### Best Practices
1. **Clear Task Boundaries**: Each agent has a distinct focus
2. **Parallel Execution**: Launch all agents in one response
3. **Quality Control**: Always include a verification agent
4. **Academic-First**: Prioritize MCP academic tools over web search

## GoT Agent Prompt Templates

### Research Agent
```
Execute GoT research for: [specific aspect] of [main topic]

1. Research using available tools:
   - mcp__arxiv__search_papers for academic papers
   - mcp__paper-search-mcp__search_google_scholar for broad coverage
   - WebSearch for general web sources
   - WebFetch for extracting content from URLs

2. GoT process: Generate queries → Search → Score sources → KeepBestN →
   Summarize with citations → Validate claims → Aggregate

3. Save results to RESEARCH/[project_name]/[aspect].md

Return scored and validated research summary.
```

### Cross-Validation Agent
```
Validate research findings:

1. Extract all factual claims
2. Cross-reference using mcp__arxiv__search_papers, WebSearch, WebFetch
3. Score each claim's validity (0-10)
4. Keep only validated claims (score >= 7)

Return: claim validity scores, contradictions, source reliability ratings
```

### Synthesis Agent
```
Synthesize final research report:

1. Collect all research findings from agents
2. Score quality of each section (0-10)
3. Aggregate into coherent narrative preserving all citations
4. Validate all claims have verifiable sources

Output: executive summary, integrated findings, citation list, confidence scores
```

## Research Quality Checklist

- [ ] Every claim has a verifiable source
- [ ] Multiple sources corroborate key findings
- [ ] Contradictions are acknowledged and explained
- [ ] Sources are recent and authoritative
- [ ] No hallucinations or unsupported claims
- [ ] Proper citation format throughout

## Advanced Research Methodologies

### Chain-of-Density (CoD) Summarization
1. First pass: Extract key points (low density)
2. Second pass: Add supporting details and context
3. Third pass: Compress while preserving all critical information
4. Final pass: Maximum density with all essential facts and citations

### Chain-of-Verification (CoVe)
1. Generate initial research findings
2. Create verification questions for each claim
3. Search for evidence to answer verification questions
4. Revise findings based on verification results

### ReAct Pattern (Reason + Act)
1. **Reason**: Analyze what information is needed
2. **Act**: Execute search or retrieval action
3. **Observe**: Process the results
4. **Reason**: Determine if more information needed
5. **Repeat**: Continue until sufficient evidence gathered

### Multi-Agent Orchestration

**Team Mode (4+ subtopics) — PREFERRED**:
- **TeamCreate**: Create team "research-{topic_slug}"
- **TaskCreate**: Define tasks with dependencies
- **Spawn Teammates**: academic-1..N, web-researcher, verifier, synthesizer
- **SendMessage**: Inter-agent communication
- **Progressive Synthesis**: Synthesizer starts after 2+ agents complete
- **GoT Integration**: Controller scores findings, redirects low-quality agents
- **Graceful Shutdown**: shutdown_request → TeamDelete

**Sub-Agent Mode (1-3 subtopics) — Backward Compatible**:
- Deploy specialized agents in parallel via Task tool
- Include at least one verification agent

## Citation & Quality Standards

> See `CLAUDE.md` for full citation requirements, source quality ratings (A-E), and output structure.

### Agent Citation Instructions
Include in all agent prompts:
```
"For every factual claim, provide:
1. Direct quote or specific data point
2. Author/organization name
3. Publication year
4. Full title
5. Direct URL/DOI
6. Confidence rating (High/Medium/Low)
Never make claims without sources. State 'Source needed' if uncertain."
```

## Cost Optimization

- Use `model: "haiku"` for quick, straightforward sub-agent tasks
- Use `model: "sonnet"` for standard research agents
- Reserve default (Opus) for complex synthesis and final reports
- Cache intermediate results by saving to `research_notes/`
- Batch similar operations via parallel Task deployment

## GoT Execution Example

### "Deep research CRISPR gene editing safety"

**Iteration 1 — Initialize and Explore**:
1. Controller creates root node
2. Generate(3) deploys 3 agents: evidence, safety concerns, regulations
3. Results scored: 6.8, 8.2, 7.5

**Iteration 2 — Deepen Best Paths**:
1. n3 (8.2): Generate(3) for deeper exploration
2. n2 (7.5): Generate(2)
3. n4 (6.8): Refine(1) to improve
4. Best result scores 9.1

**Iteration 3 — Aggregate**:
1. Aggregate(3) merges best thoughts → Score: 9.3

**Iteration 4 — Final Polish**:
1. Refine(1) → Final score: 9.5 → Output as research report

## Automatic GoT Execution

### Graph State Schema
```json
{
  "nodes": {
    "n1": {"text": "...", "score": 0, "type": "root", "depth": 0}
  },
  "edges": [],
  "frontier": ["n1"],
  "budget": {"tokens_used": 0, "max_tokens": 50000}
}
```

### Execution Loop
```
repeat until DONE {
    1. Select top-3 frontier thoughts by score
    2. Choose transformation per thought:
       - depth < 2: Generate(3)
       - score < 7: Refine(1)
       - multiple good paths: Aggregate(k)
    3. Deploy transformation agents
    4. Update graph with new nodes, edges, scores
    5. Prune: KeepBestN(5) per depth level
    6. Exit when max_score > 9 or depth > 4
}
```

### Graph Traversal Strategy
1. **Early Depth (0-2)**: Aggressive Generate(3) to explore search space
2. **Mid Depth (2-3)**: Mix of Generate for promising paths + Refine for weak nodes
3. **Late Depth (3-4)**: Aggregate best branches + final Refine
4. **Pruning**: Keep only top 5 nodes per depth level
5. **Termination**: When best node scores 9+ or depth exceeds 4

Save graph state to `RESEARCH/[topic]/research_notes/got_operations_log.md` after each iteration.
