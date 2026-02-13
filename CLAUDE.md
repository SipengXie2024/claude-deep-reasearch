# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview


This is a **Claude Code Deep Research Agent** framework that implements a sophisticated multi-agent research system to conduct comprehensive, citation-backed research projects. It replicates and enhances OpenAI's Deep Research and Google Gemini's Deep Research capabilities using Claude Code's native features.

**Core Technology:**
- **Graph of Thoughts (GoT)** Framework - Intelligent research path management with graph-based reasoning
- **7-Phase Deep Research Process** - Structured methodology for quality research
- **Multi-Agent Architecture** - Parallel research agents with specialized roles
- **Citation Validation System** - A-E source quality ratings with chain-of-verification

## Quick Start Commands

### Primary Research Command

```bash
/deep-research [research topic]
```

This single command executes the complete research workflow:
1. Question refinement (asks clarifying questions)
2. Research planning with subtopics
3. Multi-agent parallel research deployment
4. Source triangulation and synthesis
5. Citation validation
6. Output to `RESEARCH/[topic]/` directory

### Step-by-Step Commands

```bash
/refine-question [raw question]     # Generate structured research prompt
/plan-research [structured prompt]   # Create execution plan
/synthesize-findings [directory]     # Combine research outputs
/validate-citations [file]           # Verify citation quality
```

## Architecture

### 7-Phase Research Process

1. **Question Scoping** - Define boundaries, output format, constraints
2. **Retrieval Planning** - Break topic into subtopics, plan agent deployment
3. **Iterative Querying** - Deploy parallel research agents (3-8 agents)
4. **Source Triangulation** - Cross-validate findings across sources
5. **Knowledge Synthesis** - Combine into coherent narrative with citations
6. **Quality Assurance** - Fact-check, validate citations, detect hallucinations
7. **Output & Packaging** - Generate structured research documents

### Skills System

The `.claude/skills/` directory contains modular research capabilities:

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| `question-refiner` | Transform raw questions into structured prompts | Vague topics, undefined scope |
| `research-executor` | Execute full 7-phase research process | Structured research prompts |
| `got-controller` | Manage Graph of Thoughts for complex topics | Multi-faceted research, quality-critical |
| `citation-validator` | Verify citation accuracy and quality | Final reports, publishing preparation |
| `synthesizer` | Combine findings from multiple agents | Contradictory sources, comprehensive reports |
| `team-coordinator` | Manage research team lifecycle | Complex research (4+ subtopics), team coordination |

### Graph of Thoughts Operations

The GoT framework manages research as a graph with these transformations:

- **Generate(k)**: Spawn k parallel research paths from a node
- **Aggregate(k)**: Merge k findings into stronger synthesis
- **Refine(1)**: Improve existing finding without new research
- **Score**: Rate quality (0-10) based on citations, accuracy, completeness
- **KeepBestN(n)**: Prune to top n nodes per level

**Research Patterns:**
- **Balanced**: Generate(4-5) → Score best → Deepen top paths → Aggregate
- **Depth-first**: Generate(3) → Take best → Generate(3) from it → Continue
- **Breadth-first**: Generate(8) → KeepBestN(5) → Generate(2) from each → Aggregate

### Agent Teams Architecture

**⚠️ Academic-First Research**: All research MUST prioritize academic sources via MCP tools.

The framework supports two deployment modes based on research complexity:

**Mode A: Agent Teams (4+ subtopics) — PREFERRED**
```
Phase 3: Team-Based Research
├── TeamCreate: "research-{topic_slug}"
├── TaskCreate × N: Define tasks with dependencies
├── Spawn Teammates:
│   ├── academic-1..N (teammates): Academic search per subtopic
│   ├── web-researcher (teammate): Current info, news
│   ├── verifier (teammate): Cross-validate claims
│   └── synthesizer (teammate): Progressive synthesis
├── Coordination via SendMessage:
│   ├── Finding reports (Agent → Main Controller)
│   ├── Verification requests (Main → Verifier)
│   ├── Synthesis triggers (Main → Synthesizer)
│   └── GoT redirects (Main → Agent, if score < 6.0)
└── Shutdown: shutdown_request → TeamDelete
```

**Mode B: Task Sub-Agents (1-3 subtopics) — BACKWARD COMPATIBLE**
```
Phase 3: Iterative Querying (Academic-First)
├── Academic Research Agents (3-4) [PRIMARY]
├── Web Research Agents (1-2) [SUPPLEMENTARY]
├── Verification Agent (1) [REQUIRED]
└── All launched in single response with run_in_background: true
```

**Auto-Detection**: The system automatically selects Team Mode for 4+ subtopics and Sub-Agent Mode for 1-3 subtopics. Falls back to Sub-Agent Mode if TeamCreate is unavailable.

**MCP Academic Tools Priority**:
1. `mcp__arxiv__search_papers` - arXiv (CS, Physics, Math, AI/ML)
2. `mcp__paper-search-mcp__search_google_scholar` - Broad coverage
3. `mcp__paper-search-mcp__search_pubmed` - Biomedical
4. `mcp__paper-search-mcp__search_biorxiv` - Biology preprints
5. `mcp__paper-search-mcp__search_medrxiv` - Medical preprints
6. `mcp__arxiv__read_paper` - Full paper content

Each agent receives:
- Clear description of research focus
- **Mandatory MCP tool usage instructions**
- Specific search queries
- Expected output format
- Citation requirements
- **Team communication protocol** (Team Mode only): SendMessage for reports, TaskUpdate for status

## Output Structure

Research outputs are created in `RESEARCH/[topic_name]/`:

```
RESEARCH/[topic_name]/
├── README.md                    # Overview and navigation
├── executive_summary.md         # 1-2 page key findings
├── full_report.md               # Complete analysis (20-50 pages)
├── data/
│   └── statistics.md            # Key numbers, facts
├── visuals/
│   └── descriptions.md          # Chart/graph descriptions
├── sources/
│   ├── bibliography.md          # Complete citations
│   └── source_quality_table.md  # A-E ratings
├── research_notes/
│   └── agent_findings_summary.md # Raw agent outputs
└── appendices/
    ├── methodology.md           # Research methods
    └── limitations.md           # Unknowns, gaps
```

## Citation Requirements

**Every factual claim must include:**
1. Author/Organization name
2. Publication date
3. Source title
4. Direct URL/DOI
5. Page numbers (if applicable)

**Source Quality Ratings:**
- **A**: Peer-reviewed RCTs, systematic reviews, meta-analyses
- **B**: Cohort studies, clinical guidelines, reputable analysts
- **C**: Expert opinion, case reports, mechanistic studies
- **D**: Preprints, preliminary research, blogs
- **E**: Anecdotal, theoretical, speculative

## Tool Permissions

The `.claude/settings.local.json` file configures allowed tools:
- **WebSearch**: General web searches
- **mcp__playwright__***: Browser automation for dynamic content
- **Task**: Deploy autonomous research agents or spawn teammates
- **TeamCreate/TeamDelete**: Create and manage research teams
- **SendMessage**: Inter-agent communication within teams
- **TaskCreate/TaskUpdate/TaskList/TaskGet**: Shared task tracking with dependencies
- **Read/Write**: Manage research documents
- **mcp__arxiv__***: arXiv paper search and reading
- **mcp__paper-search-mcp__***: Multi-database academic search

## Development Notes

### When Modifying Skills

Each skill in `.claude/skills/[name]/` contains:
- `SKILL.md`: YAML frontmatter + description
- `instructions.md`: Detailed implementation guidance
- `examples.md`: Usage examples

When creating new skills:
1. Follow the existing structure
2. Include clear YAML frontmatter in SKILL.md
3. Provide comprehensive instructions
4. Add diverse examples

### When Modifying Commands

Commands in `.claude/commands/[name].md` are user-facing shortcuts. Each command:
- Has YAML frontmatter with description, argument hint, allowed tools
- References the appropriate skill for execution
- Should be simple and focused

### Graph State Management

When using GoT Controller for research:
- Maintain graph state throughout execution
- Document operations in `research_notes/got_operations_log.md`
- Save individual nodes to `research_notes/got_nodes/[id].md`
- Use TaskCreate/TaskUpdate to track GoT operations (replaces TodoWrite)
- In Team Mode: shared task list supplements graph state for cross-agent visibility

## Key Documentation

- `CLAUDE.md`: This file - Quick reference for Claude Code
- `CLAUDE2.md`: Complete Graph of Thoughts implementation guide
- `PROJECT_UNDERSTANDING.md`: Detailed architecture and design
- `IMPLEMENTATION_GUIDE.md`: User guide with examples and workflows
- `.claude/skills/*/instructions.md`: Skill-specific instructions

## Important Constraints

- All research outputs go in `RESEARCH/[topic]/` directories
- Break large documents into smaller files to avoid context limits
- Track task completion using TaskCreate/TaskUpdate (shared task tracking)
- Use Agent Teams for complex research (4+ subtopics) or parallel Task deployment for simple research
- Always use shutdown_request + TeamDelete for graceful team cleanup
- Validate citations before finalizing reports
- Never make claims without sources - state "Source needed" if uncertain
