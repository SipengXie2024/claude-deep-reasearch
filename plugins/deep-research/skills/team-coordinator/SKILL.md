---
name: team-coordinator
description: This skill should be used internally when the research-executor needs to "create a research team", "coordinate agent teams", "manage team lifecycle", or when 4+ subtopics require TeamCreate-based coordination with task dependencies and structured messaging.
user-invocable: false
allowed-tools:
  - TeamCreate
  - TeamDelete
  - SendMessage
  - TaskCreate
  - TaskUpdate
  - TaskList
  - TaskGet
  - Task
  - Read
  - Write
---

# Team Coordinator

The **Team Coordinator** manages the lifecycle of research agent teams.

## Team Structure

```
research-{topic_slug}/
├── Main Controller (orchestrator, GoT controller)
├── academic-1..N (academic search per subtopic)
├── web-researcher (current info, news)
├── verifier (cross-validate claims)
└── synthesizer (progressive synthesis)
```

## Lifecycle

1. **Creation**: TeamCreate → TaskCreate (with dependencies) → Task (with team_name) to spawn → TaskUpdate to assign
2. **Active Research**: Teammates send findings via SendMessage → GoT scoring → route verification → trigger synthesis after 2+
3. **Convergence**: Progressive synthesis → verification of synthesized claims → final QA
4. **Shutdown**: shutdown_request to each teammate → wait for responses → TeamDelete

## Mode Selection

```
subtopics <= 3  →  Task sub-agents (simpler, faster)
subtopics >= 4  →  Agent Teams (REQUIRED)
TeamCreate fails →  Fallback to Task sub-agents
```

## Message Protocol

| Message | Direction | Header |
|---------|-----------|--------|
| Finding Report | Agent → Controller | `[FINDING REPORT]` |
| Verification Request | Controller → Verifier | `[VERIFICATION REQUEST]` |
| Synthesis Trigger | Controller → Synthesizer | `[SYNTHESIS TRIGGER]` |
| GoT Redirect | Controller → Agent | `[RESEARCH REDIRECT]` |

## Error Recovery

- **Agent failure**: Check TaskList for stuck tasks → spawn replacement → reassign via TaskUpdate
- **Quality failure** (score < 6.0): Send redirect with new direction → create adjusted task

## Details

## Additional Resources

### Reference Files
- **[`references/instructions.md`](references/instructions.md)** — Message format details, task dependency templates, error handling procedures

### Examples
- **[`examples/examples.md`](examples/examples.md)** — Team coordination examples
