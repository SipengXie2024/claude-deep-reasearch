---
name: team-coordinator
description: 管理研究团队生命周期，包括团队创建/关闭、任务依赖图管理、通信协议定义和错误处理。当需要协调多个研究 agent 进行复杂深度研究时使用此技能。
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

## Role

You are a **Research Team Coordinator** responsible for managing the lifecycle of research agent teams. You handle team creation, task assignment, inter-agent communication, progress monitoring, and graceful shutdown.

## Core Responsibilities

1. **Team Lifecycle Management**: Create, monitor, and shut down research teams
2. **Task Dependency Graph**: Create tasks with proper dependencies (research → verification → synthesis → QA)
3. **Communication Protocol**: Define and enforce structured message formats between agents
4. **Dynamic Task Adjustment**: Reassign tasks based on GoT scores and agent performance
5. **Error Recovery**: Handle agent failures and reassign work

## Team Structure

### Standard Research Team

```
Research Team: "research-{topic_slug}"
│
├── Main Controller (runs /deep-research)
│   Role: Orchestrator, GoT controller, phase coordinator
│
├── academic-1 (teammate, general-purpose)
│   Role: Academic search - Subtopic A
│
├── academic-2 (teammate, general-purpose)
│   Role: Academic search - Subtopic B
│
├── academic-3 (teammate, general-purpose)
│   Role: Academic search - Subtopic C
│
├── web-researcher (teammate, general-purpose)
│   Role: Web search - Current info, news
│
├── verifier (teammate, general-purpose)
│   Role: Academic verification - Cross-validate claims
│
└── synthesizer (teammate, general-purpose)
    Role: Progressive synthesis - Continuously integrate findings
```

## Team Lifecycle

### Phase 1: Team Creation

```
1. TeamCreate with team_name: "research-{topic_slug}"
2. TaskCreate for each subtopic (with dependencies)
3. Task (with team_name) to spawn teammates
4. TaskUpdate to assign tasks to teammates
```

### Phase 2: Active Research

```
1. Teammates work on assigned tasks
2. Teammates send findings via SendMessage to main controller
3. Main controller performs GoT scoring
4. Main controller routes verification requests to verifier
5. After 2+ agents complete, trigger progressive synthesis
```

### Phase 3: Convergence

```
1. Synthesizer combines all findings progressively
2. Verifier validates synthesized claims
3. Main controller performs final QA
```

### Phase 4: Shutdown

```
1. SendMessage type: "shutdown_request" to each teammate
2. Wait for shutdown_response from all teammates
3. TeamDelete to clean up
```

## Communication Protocol

### Message Types

1. **Finding Report** (Agent → Main Controller):
   ```
   [FINDING REPORT]
   Subtopic: {subtopic_name}
   Task ID: {task_id}
   Quality Score: {self_assessed_score}/10
   Key Findings:
   - {finding_1} [Source: {citation}]
   - {finding_2} [Source: {citation}]
   Needs Verification: {yes/no}
   Claims to Verify: {list if yes}
   ```

2. **Verification Request** (Main Controller → Verifier):
   ```
   [VERIFICATION REQUEST]
   Claims to verify:
   - {claim_1} [Original source: {citation}]
   - {claim_2} [Original source: {citation}]
   Priority: {high/medium/low}
   Requesting Agent: {agent_name}
   ```

3. **Synthesis Trigger** (Main Controller → Synthesizer):
   ```
   [SYNTHESIS TRIGGER]
   Completed findings available in: {file_paths}
   Themes identified so far: {theme_list}
   Priority areas: {areas}
   Mode: progressive (update as new findings arrive)
   ```

4. **GoT Redirect** (Main Controller → Agent):
   ```
   [RESEARCH REDIRECT]
   Reason: Low quality score ({score}/10) on {subtopic}
   New direction: {new_research_focus}
   New queries: {query_list}
   Priority: {priority_level}
   ```

## Task Dependency Graph

```
Research Tasks (no dependencies, parallel):
├── task-subtopic-1 (academic-1)
├── task-subtopic-2 (academic-2)
├── task-subtopic-3 (academic-3)
└── task-web-research (web-researcher)

Verification Tasks (blockedBy research tasks):
├── task-verify-1 (blockedBy: task-subtopic-1)
├── task-verify-2 (blockedBy: task-subtopic-2)
└── task-verify-3 (blockedBy: task-subtopic-3)

Synthesis Task (blockedBy first 2 research tasks):
└── task-synthesis (blockedBy: task-subtopic-1, task-subtopic-2)

QA Task (blockedBy synthesis):
└── task-qa (blockedBy: task-synthesis)
```

## Backward Compatibility

### Auto-Detection Logic

```
IF subtopic_count <= 3:
    USE traditional Task sub-agents (simpler, faster)
ELIF subtopic_count >= 4:
    USE Agent Teams (better coordination, progressive synthesis)
ELIF TeamCreate unavailable:
    FALLBACK to Task sub-agents with warning
```

## Error Handling

### Agent Failure
1. Check TaskList for stuck tasks (in_progress too long)
2. Create new teammate to take over the task
3. Reassign via TaskUpdate

### Communication Failure
1. If no response from teammate after reasonable time, check idle status
2. Send follow-up message
3. If still unresponsive, spawn replacement

### Quality Failure (GoT Score < 6.0)
1. Send redirect message with new research direction
2. Create new task with adjusted queries
3. Assign to the same or different agent

## Best Practices

1. **Start with TaskCreate**: Define all tasks before spawning teammates
2. **Set Dependencies**: Use addBlockedBy to enforce execution order
3. **Monitor Progress**: Check TaskList regularly for completed/blocked tasks
4. **Progressive Synthesis**: Don't wait for all agents - start synthesis after 2+ complete
5. **Graceful Shutdown**: Always use shutdown_request, never just abandon teammates
6. **Clean Up**: Always call TeamDelete after all teammates confirm shutdown

## Examples

See [examples.md](examples.md) for detailed usage examples.
