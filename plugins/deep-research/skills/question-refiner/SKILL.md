---
name: question-refiner
description: This skill should be used when the user asks to "refine my research question", "help me define the research scope", "create a structured research prompt", "what should I research about [topic]", or provides a vague/broad research topic that needs clarification before execution.
user-invocable: true
argument-hint: "[raw research question or topic]"
allowed-tools:
  - Read
  - Write
  - AskUserQuestion
---

# Question Refiner

The **Question Refiner** crafts structured research prompts through systematic clarification. It does NOT answer the research question itself — only structures it.

## Critical Rules

1. **Ask first, generate later**: Always ask clarifying questions before producing the structured prompt.
2. **Do not skip fields**: Every section of the output template must be filled with concrete details.
3. **Wait for answers**: Do NOT generate the prompt until the user responds to your questions.

## Interaction Flow

1. **Ask 5 categories of clarifying questions**: Core Question, Output Requirements, Scope & Boundaries, Sources & Credibility, Special Requirements.
2. **Wait for user response**. If incomplete, ask follow-ups.
3. **Generate structured prompt** with sections: TASK, CONTEXT/BACKGROUND, SPECIFIC QUESTIONS, KEYWORDS, CONSTRAINTS, OUTPUT FORMAT, FINAL INSTRUCTIONS.
4. **Verify quality** against checklist before delivering.

## Output Template

```markdown
### TASK
[Clear statement of what needs to be researched]

### CONTEXT/BACKGROUND
[Why this research matters, who will use it]

### SPECIFIC QUESTIONS OR SUBTASKS
1. [Question 1]
2. [Question 2]
...

### KEYWORDS
[keyword1, keyword2, ...]

### CONSTRAINTS
- Timeframe: [date range]
- Geography: [regions]
- Source Types: [academic, industry, etc.]
- Length: [word count]

### OUTPUT FORMAT
- [Format details, citation style]

### FINAL INSTRUCTIONS
[Citation requirements: Author, Date, Title, URL/DOI, Pages]
```

## Details

## Additional Resources

### Reference Files
- **[`references/instructions.md`](references/instructions.md)** — Question patterns by research type, special case handling

### Examples
- **[`examples/examples.md`](examples/examples.md)** — Complete question refinement examples
