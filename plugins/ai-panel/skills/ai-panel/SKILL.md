---
name: ai-panel
description: Consults other AI models (Gemini, GPT) for second opinions on architecture, approach, and trade-offs. Use during planning or when a decision benefits from multiple perspectives.
allowed-tools: Bash(gemini:*), Bash(copilot:*), Bash(timeout:*)
---

# AI Panel — Second Opinions from Other Models

## When to Use

- **Plan mode**: Get alternative perspectives on architecture, technology choices, or trade-offs before committing to an approach.
- **Implementation**: Sanity-check a specific decision (e.g. library choice, data model, algorithm).
- **Debugging**: Ask for fresh eyes on a tricky problem.

**When NOT to use**: Trivial decisions, obvious fixes, or when you're already highly confident. Don't waste premium requests on things that don't benefit from diversity of thought.

## Available Models

| Model             | Command                               | Strengths                                                |
| ----------------- | ------------------------------------- | -------------------------------------------------------- |
| Google Gemini     | `gemini -p "prompt"`                  | Web-grounded knowledge, long context, research questions |
| GPT (via Copilot) | `copilot -p "prompt" --model gpt-5.2` | Strong reasoning, code generation, structured output     |

**Important**: Copilot defaults to Claude Sonnet — always pass `--model gpt-5.2` to get a genuinely different perspective. The point of the panel is model diversity.

## How to Call

Always call both models **in parallel** (two Bash calls in a single message) and wrap in `timeout` to prevent hangs.

```bash
# Gemini (30s timeout)
timeout 30 gemini -p "Context: ... Question: ... Be concise, 3-5 key points."

# GPT via Copilot (30s timeout)
timeout 30 copilot -p "Context: ... Question: ... Be concise, 3-5 key points." --model gpt-5.2 --allow-all-tools
```

### Prompt Guidelines

- **Always end with**: "Be concise, 3-5 key points." — both models ramble without this.
- Provide enough context for a useful answer (tech stack, constraints, goals).
- Ask a specific question — not "what do you think?" but "which approach is better for X given Y?"
- Keep prompts under ~300 words. Longer prompts cause Gemini to hang.
- Include the most relevant constraint or trade-off to focus the response.

## Model Routing — When to Use Which

### Ask both (default for `/second-opinion`)

Architecture decisions, technology choices, approach comparison — anything where genuine disagreement is valuable.

### Gemini only

- Questions about recent technologies, APIs, or documentation (web-grounded)
- Broad "what are the options" research questions
- When you need a non-code perspective

### GPT only

- Code-specific questions (algorithm choice, data structure, implementation pattern)
- When you need structured/formatted output
- Quick sanity checks on a specific approach

### Ask neither

- You're already confident and the decision is straightforward
- The question is project-specific (only you have the context)
- The question is about style/preference rather than correctness

## Output Handling

### Gemini

Strip the `Loaded cached credentials.` prefix line if present. The useful response starts after it.

### Copilot (GPT)

Strip the trailing usage stats block. Everything from the blank line before `Total usage est:` onwards should be removed. Only keep the actual response content.

### Timeout

If a model times out (30s), note it in the output and proceed with whichever model responded. Don't retry — the user can re-run if needed.

## Presenting Results

Format the panel's output clearly:

```
### Gemini
<cleaned response>

### GPT
<cleaned response>

### Synthesis
- **Agreement**: Where both models align
- **Disagreement**: Where they differ and why
- **Recommendation**: Your own assessment, weighing all perspectives
```

Always give your own recommendation last. The panel informs your judgement — it doesn't replace it.
