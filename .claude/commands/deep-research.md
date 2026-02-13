---
description: 对指定主题执行完整的深度研究流程，从问题细化到最终报告生成
argument-hint: [研究主题或问题]
allowed-tools: Task, TeamCreate, TeamDelete, SendMessage, TaskCreate, TaskUpdate, TaskList, TaskGet, WebSearch, WebFetch, Read, Write, Skill(academic-search), mcp__arxiv__*, mcp__paper-search-mcp__*
---

# Deep Research

Execute comprehensive deep research on the given topic using the 7-phase research methodology and Graph of Thoughts framework.

## Topic

$ARGUMENTS

## Research Workflow

This command will execute the following steps:

### Step 1: Question Refinement
Use the **question-refiner** skill to ask clarifying questions and generate a structured research prompt.

### Step 2: Research Planning
Break down the research topic into 3-7 subtopics and create a detailed execution plan.

### Step 3: Team-Based Research (Academic-First)
Deploy a coordinated research team (4+ subtopics) or parallel sub-agents (1-3 subtopics):

**Team Mode (4+ subtopics)**:
1. **TeamCreate**: Create research team "research-{topic_slug}"
2. **TaskCreate**: Define tasks for each subtopic with dependencies
3. **Spawn teammates**: academic-1..N, web-researcher, verifier, synthesizer
4. **Assign tasks** via TaskUpdate
5. **Monitor**: Receive finding reports, GoT score, redirect if needed
6. **Progressive synthesis**: Trigger after 2+ agents complete
7. **Verification loop**: Forward claims to verifier, apply corrections
8. **Shutdown**: shutdown_request to all → TeamDelete

**Sub-Agent Mode (1-3 subtopics, backward compatible)**:
- Launch all Task agents in one response with `run_in_background: true`

**Agent types (both modes)**:
- **Academic Research Agents (3-4) [PRIMARY]**: Use MCP tools (`mcp__arxiv__*`, `mcp__paper-search-mcp__*`)
- **Web Research Agents (1-2) [SUPPLEMENTARY]**: Current information, trends, news
- **Verifier Agent (1) [REQUIRED]**: Cross-validate claims against academic literature
- **Synthesizer Agent (1) [PROGRESSIVE]**: Begin synthesis after 2+ agents complete

### Step 4: Source Triangulation
Compare findings across multiple sources and validate claims using the A-E quality rating system.

### Step 5: Knowledge Synthesis
Use the **synthesizer** skill to combine findings into a coherent report with inline citations.

### Step 6: Citation Validation
Use the **citation-validator** skill to verify all claims have accurate, complete citations.

### Step 7: Output Generation
Generate structured research outputs in the `RESEARCH/[topic]/` directory:
- README.md
- executive_summary.md
- full_report.md
- data/
- visuals/
- sources/
- research_notes/
- appendices/

## Citation Requirements

Ensure every factual claim includes:
1. Author/Organization name
2. Publication date
3. Source title
4. Direct URL/DOI
5. Page numbers (if applicable)

Begin the deep research process now.
