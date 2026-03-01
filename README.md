# Claude Code Deep Research Plugin

A sophisticated multi-agent deep research framework for Claude Code. Implements OpenAI Deep Research and Google Gemini Deep Research capabilities using Claude Code's native plugin system.

## Features

- **Graph of Thoughts (GoT) Framework** - Intelligent research path management with graph-based reasoning
- **7-Phase Deep Research Process** - Structured methodology for quality research
- **Multi-Agent Architecture** - Parallel research agents with specialized roles (3-8 agents)
- **Agent Teams Coordination** - Real-time team communication, progressive synthesis, dynamic task adjustment
- **Academic-First Search** - MCP-powered academic paper search (arXiv, Google Scholar, PubMed, bioRxiv, medRxiv)
- **Citation Validation System** - A-E source quality ratings with chain-of-verification
- **Structured Output** - Executive summaries, full reports, bibliography, methodology documentation

## Installation

### Method 1: From GitHub (Recommended)

在 Claude Code 中直接运行：

```bash
/plugin install github:SipengXie2024/claude-deep-reasearch
```

插件会自动安装，包括所有 skills、commands 和 MCP 服务器配置。

### Method 2: Local Install

```bash
# 克隆仓库
git clone https://github.com/SipengXie2024/claude-deep-reasearch.git

# 在 Claude Code 中安装本地插件
/plugin install file:///absolute/path/to/claude-deep-reasearch/plugins/deep-research
```

### Prerequisites

- Claude Code CLI v1.0.33+
- `uvx` (用于运行 arXiv MCP 服务器)
- MCP 服务器随插件自动安装（配置在插件的 `.mcp.json` 中）：
  - `arxiv` - arXiv 论文搜索与阅读
  - `paper-search-mcp` - Google Scholar、PubMed、bioRxiv、medRxiv

### Permission Setup

安装后，在 Claude Code 中首次使用时会提示授权。也可以预先在项目或用户设置中添加权限：

```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "WebFetch(domain:arxiv.org)",
      "WebFetch(domain:semanticscholar.org)",
      "WebFetch(domain:pubmed.ncbi.nlm.nih.gov)",
      "WebFetch(domain:scholar.google.com)",
      "mcp__arxiv__*",
      "mcp__paper-search-mcp__*",
      "TeamCreate",
      "TeamDelete",
      "SendMessage",
      "TaskCreate",
      "TaskUpdate",
      "TaskList",
      "TaskGet"
    ]
  }
}
```

## Commands

After installation, all commands are namespaced under `deep-research`:

| Command | Usage | Description |
|---------|-------|-------------|
| `/deep-research:research` | `/deep-research:research [topic]` | Execute complete research workflow |
| `/deep-research:refine-question` | `/deep-research:refine-question [question]` | Refine into structured prompt |
| `/deep-research:plan-research` | `/deep-research:plan-research [prompt]` | Create execution plan |
| `/deep-research:synthesize-findings` | `/deep-research:synthesize-findings [dir]` | Combine research outputs |
| `/deep-research:validate-citations` | `/deep-research:validate-citations [file]` | Verify citation quality |

## Quick Start

### One-Command Research

```bash
/deep-research:research AI applications in clinical diagnosis
```

This single command will:
1. Ask clarifying questions to refine your research needs
2. Create a structured research plan with subtopic decomposition
3. Deploy multiple parallel research agents (academic-first)
4. Cross-validate findings via source triangulation
5. Synthesize into a comprehensive report with inline citations
6. Validate all citations for accuracy
7. Output results to `RESEARCH/[topic]/` directory

### Step-by-Step Workflow

```bash
# Step 1: Refine your question
/deep-research:refine-question Should I use WebAssembly for my project?

# Step 2: Plan the research
/deep-research:plan-research [structured prompt from step 1]

# Step 3: Execute research (uses the refined prompt)
/deep-research:research [your topic]

# Step 4: Synthesize (if needed)
/deep-research:synthesize-findings RESEARCH/[topic]/research_notes/

# Step 5: Validate citations
/deep-research:validate-citations RESEARCH/[topic]/full_report.md
```

## Plugin Structure

```
plugins/deep-research/             # Plugin root (installed by /plugin install)
├── .claude-plugin/
│   └── plugin.json                # Plugin manifest (includes mcpServers reference)
├── .mcp.json                      # MCP server configurations (arxiv, paper-search-mcp)
├── skills/                        # Agent Skills (model-invoked)
│   ├── research-executor/         # 7-phase research execution
│   ├── question-refiner/          # Question refinement
│   ├── got-controller/            # Graph of Thoughts controller
│   ├── citation-validator/        # Citation validation
│   ├── synthesizer/               # Research synthesis
│   ├── academic-search/           # Academic paper search
│   └── team-coordinator/          # Agent team lifecycle
└── commands/                      # User commands (slash commands)
    ├── research.md                # Main research command
    ├── refine-question.md         # Question refinement
    ├── plan-research.md           # Research planning
    ├── synthesize-findings.md     # Findings synthesis
    └── validate-citations.md      # Citation validation
```

## Skills Reference

| Skill | Purpose | Invocation |
|-------|---------|------------|
| `research-executor` | Execute full 7-phase research | Auto-invoked by commands |
| `question-refiner` | Transform raw questions into structured prompts | Auto-invoked or manual |
| `got-controller` | Manage Graph of Thoughts for complex topics | Auto-invoked for quality optimization |
| `citation-validator` | Verify citation accuracy and quality | Auto-invoked or `/deep-research:validate-citations` |
| `synthesizer` | Combine findings from multiple agents | Auto-invoked during synthesis phase |
| `academic-search` | Multi-database academic paper search | Auto-invoked by research agents |
| `team-coordinator` | Manage research team lifecycle | Auto-invoked for 4+ subtopics |

## Research Output Structure

Each research project creates structured output in `RESEARCH/[topic]/`:

```
RESEARCH/[topic_name]/
├── README.md                    # Overview and navigation
├── executive_summary.md         # 1-2 page key findings
├── full_report.md               # Complete analysis (20-50 pages)
├── data/
│   └── statistics.md            # Key numbers and facts
├── sources/
│   ├── bibliography.md          # Complete citations
│   └── source_quality_table.md  # A-E quality ratings
├── research_notes/
│   └── agent_findings_summary.md
└── appendices/
    ├── methodology.md
    └── limitations.md
```

## Citation Quality Ratings

| Rating | Quality | Examples |
|--------|---------|---------|
| **A** | Top Tier | Peer-reviewed RCTs, systematic reviews, meta-analyses |
| **B** | High | Cohort studies, clinical guidelines, reputable analysts |
| **C** | Moderate | Expert opinion, case reports, mechanistic studies |
| **D** | Preliminary | Preprints, workshop papers, blogs |
| **E** | Speculative | Anecdotal, theoretical |

## Graph of Thoughts Operations

| Operation | Purpose |
|-----------|---------|
| **Generate(k)** | Spawn k parallel research paths |
| **Aggregate(k)** | Merge k findings into synthesis |
| **Refine(1)** | Improve existing finding quality |
| **Score** | Rate quality 0-10 |
| **KeepBestN(n)** | Prune to top n nodes |

## Agent Teams Architecture

For complex research (4+ subtopics), the framework automatically deploys coordinated Agent Teams:

```
Research Team: "research-{topic}"
├── academic-1..N   [PRIMARY]     Academic search per subtopic
├── web-researcher  [SUPPLEMENTARY] Current info, news, trends
├── verifier        [REQUIRED]     Cross-validate claims
└── synthesizer     [PROGRESSIVE]  Begin synthesis after 2+ agents complete
```

## Examples

### Market Research
```bash
/deep-research:research AI in healthcare market, clinical diagnosis focus, global scope, 2022-2024
```

### Technical Assessment
```bash
/deep-research:research WebAssembly vs JavaScript performance benchmarks
```

### Academic Literature Review
```bash
/deep-research:research Transformer architectures, peer-reviewed only, 2017-present
```

## Documentation

| Document | Description |
|----------|-------------|
| [CLAUDE.md](CLAUDE.md) | Project instructions for Claude Code |
| [CLAUDE2.md](CLAUDE2.md) | Graph of Thoughts implementation guide |
| [PROJECT_UNDERSTANDING.md](PROJECT_UNDERSTANDING.md) | Architecture and design rationale |
| [IMPLEMENTATION_GUIDE.md](IMPLEMENTATION_GUIDE.md) | User guide with examples |

## Contributing

1. Fork the repository
2. Follow the skill structure in `skills/`
3. Include `SKILL.md`, `instructions.md`, `examples.md` for new skills
4. Test with diverse research topics
5. Submit a PR

## License

MIT

## Acknowledgments

- Graph of Thoughts framework inspired by [SPCL, ETH Zurich](https://github.com/spcl/graph-of-thoughts)
- Built with [Claude Code](https://claude.ai/code)
- Academic search powered by [arXiv MCP Server](https://github.com/blazickjp/arxiv-mcp-server) and [Paper Search MCP](https://github.com/openags/paper-search-mcp)
