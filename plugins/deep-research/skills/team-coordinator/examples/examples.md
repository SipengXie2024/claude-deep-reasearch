# Team Coordinator Skill - Examples

## Example 1: Standard Research Team (4+ Subtopics)

**Topic**: "Impact of Large Language Models on Scientific Discovery"

### Step 1: Team Creation

```
TeamCreate:
  team_name: "research-llm-scientific-discovery"
  description: "Deep research on LLM impact on scientific discovery"
```

### Step 2: Task Creation

```
TaskCreate: "Research LLM applications in drug discovery"
  → task #1, owner: pending

TaskCreate: "Research LLM applications in materials science"
  → task #2, owner: pending

TaskCreate: "Research LLM applications in protein folding"
  → task #3, owner: pending

TaskCreate: "Research LLM limitations and risks in science"
  → task #4, owner: pending

TaskCreate: "Web research on recent LLM science breakthroughs"
  → task #5, owner: pending

TaskCreate: "Verify key claims from research agents"
  → task #6, addBlockedBy: [#1, #2]

TaskCreate: "Progressive synthesis of findings"
  → task #7, addBlockedBy: [#1, #2]

TaskCreate: "Final QA review"
  → task #8, addBlockedBy: [#7]
```

### Step 3: Spawn Teammates

```
Task (team_name: "research-llm-scientific-discovery"):
  name: "academic-1", prompt: "Research LLM in drug discovery..."
  name: "academic-2", prompt: "Research LLM in materials science..."
  name: "academic-3", prompt: "Research LLM in protein folding..."
  name: "academic-4", prompt: "Research LLM risks in science..."
  name: "web-researcher", prompt: "Research recent breakthroughs..."
  name: "verifier", prompt: "Verify claims..."
  name: "synthesizer", prompt: "Progressive synthesis..."
```

### Step 4: Assign Tasks

```
TaskUpdate: #1, owner: "academic-1"
TaskUpdate: #2, owner: "academic-2"
TaskUpdate: #3, owner: "academic-3"
TaskUpdate: #4, owner: "academic-4"
TaskUpdate: #5, owner: "web-researcher"
TaskUpdate: #6, owner: "verifier"
TaskUpdate: #7, owner: "synthesizer"
```

### Step 5: Monitor and Coordinate

```
# academic-1 completes first
← Message from academic-1: "[FINDING REPORT] Drug discovery findings..."
→ GoT Score: 8.2 - Accept
→ Save to RESEARCH/llm-scientific-discovery/research_notes/academic-1_findings.md
→ TaskUpdate: #1, status: completed

# academic-2 completes second
← Message from academic-2: "[FINDING REPORT] Materials science findings..."
→ GoT Score: 7.5 - Accept
→ Save findings
→ TaskUpdate: #2, status: completed

# 2+ complete → trigger synthesis
→ SendMessage to synthesizer: "[SYNTHESIS TRIGGER] 2 findings ready..."

# Verification now unblocked
→ SendMessage to verifier: "[VERIFICATION REQUEST] Verify claims..."
```

### Step 6: Shutdown

```
→ SendMessage type: "shutdown_request" to all teammates
← shutdown_response: approve from all
→ TeamDelete
```

---

## Example 2: Simple Research (≤3 Subtopics, Backward Compatible)

**Topic**: "Current State of Quantum Error Correction"

Since there are only 2-3 subtopics, use traditional Task sub-agents:

```
# No TeamCreate needed
# Launch all agents in ONE response using Task tool

Task: "Research quantum error correction theory..." (run_in_background: true)
Task: "Research practical implementations..." (run_in_background: true)
Task: "Research recent breakthroughs..." (run_in_background: true)

# Collect results with TaskOutput
# Synthesize directly
```

---

## Example 3: GoT-Driven Redirect

```
# Agent reports low-quality findings
← Message from academic-3: "[FINDING REPORT] Score: 5.5/10..."

# Main controller assesses and redirects
→ SendMessage to academic-3:
  "[RESEARCH REDIRECT]
   Reason: Low quality score (5.5/10) on protein folding
   New direction: Focus specifically on AlphaFold and ESMFold papers
   New queries: 'AlphaFold protein structure prediction', 'ESMFold language model'
   Priority: high"

# Create new task for the redirected research
→ TaskCreate: "Revised research on AlphaFold/ESMFold"
→ TaskUpdate: assign to academic-3
```

---

## Example 4: Verification Feedback Loop

```
# Synthesizer sends draft
← Message from synthesizer: "Draft synthesis ready, key claims to verify..."

# Route to verifier
→ SendMessage to verifier:
  "[VERIFICATION REQUEST]
   Claims:
   1. 'LLMs have discovered 3 new drug candidates' [Source: Nature, 2024]
   2. 'AlphaFold predictions are 95% accurate' [Source: Science, 2023]
   Priority: high"

# Verifier responds
← Message from verifier:
  "Claim 1: PARTIALLY VERIFIED - Found 2 candidates, not 3
   Claim 2: VERIFIED - Multiple sources confirm"

# Route correction to synthesizer
→ SendMessage to synthesizer:
  "[SYNTHESIS UPDATE] Correction: Update drug candidate count from 3 to 2..."
```
