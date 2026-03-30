# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Claude Code Deep Research Plugin** — a multi-agent research system that produces comprehensive, citation-backed research reports. All implementation details live in the skill files under `plugins/deep-research/skills/`.

**Core Technology:** Graph of Thoughts (GoT) framework, 7-phase research process, multi-agent teams, A-E citation quality ratings.

## Quick Start

```bash
/deep-research:research [research topic]         # Full research workflow
/deep-research:refine-question [raw question]    # Generate structured prompt
/deep-research:plan-research [structured prompt] # Create execution plan
/deep-research:synthesize-findings [directory]   # Combine research outputs
/deep-research:validate-citations [file]         # Verify citation quality
```

## Skills

| Skill | Purpose |
|-------|---------|
| `question-refiner` | Transform raw questions into structured prompts |
| `research-executor` | Execute full 7-phase research process |
| `got-controller` | Manage Graph of Thoughts for complex topics |
| `citation-validator` | Verify citation accuracy and quality |
| `synthesizer` | Combine findings from multiple agents |
| `academic-search` | Search academic databases (arXiv, Scholar, PubMed, etc.) |
| `team-coordinator` | Manage research team lifecycle (internal) |

Each skill has `SKILL.md` (concise entry point), `instructions.md` (detailed guidance), and `examples.md`.

## Hard Constraints

- **Academic-First**: All research agents MUST prioritize MCP academic tools before web sources.
- **Team Mode for 4+ subtopics**: MUST use TeamCreate. Do NOT use Task sub-agents as a shortcut.
- **Citation on every claim**: Author, Date, Title, URL/DOI, Pages (if applicable).
- **Output directory**: `RESEARCH/[topic]/` (in `.gitignore`, local only).
- **Graceful shutdown**: Always use shutdown_request + TeamDelete.

## Environment

### MCP Servers (`.mcp.json` in `plugins/deep-research/`)
- **arxiv**: `uvx arxiv-mcp-server` (storage: `/tmp/arxiv-papers`)
- **paper-search-mcp**: `server.smithery.ai` (Google Scholar, PubMed, bioRxiv, medRxiv)

## Plugin Structure

```
plugins/deep-research/
├── .claude-plugin/plugin.json    # Plugin manifest
├── skills/                       # 7 skills (SKILL.md + instructions.md + examples.md)
├── commands/                     # 5 slash commands
├── .mcp.json                     # MCP server configs
```

## Development Notes

### Skill File Convention
- `SKILL.md`: Concise — role, critical rules, decision logic, references to instructions.md
- `instructions.md`: Detailed — full process steps, templates, tool parameters
- `examples.md`: Usage examples

### Key Documentation
- `CLAUDE2.md`: GoT implementation guide (concepts, agent templates, execution patterns)
- `PROJECT_UNDERSTANDING.md`: Architecture and design rationale
- `IMPLEMENTATION_GUIDE.md`: User guide with examples
