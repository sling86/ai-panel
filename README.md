# AI Panel

Claude Code plugin for consulting other AI models. Get second opinions from Gemini and GPT on architecture decisions, code approaches, and trade-offs.

## Commands

| Command | Purpose |
|---------|---------|
| `/ai-panel:ask <model> <question>` | Ask a single model (gemini/g or gpt/c) |
| `/ai-panel:second-opinion <question>` | Get parallel opinions from both models |

## Skill

The `ai-panel` skill auto-triggers during architecture discussions and trade-off decisions, suggesting you get a second opinion from the panel.

## Requirements

- `gemini` CLI installed and authenticated
- `copilot` CLI installed and authenticated
- Both available on PATH

## Installation

```bash
claude plugin marketplace add sling86/ai-panel
claude plugin install ai-panel
```

## Examples

```bash
/ai-panel:ask gemini What auth libraries work best with SvelteKit?
/ai-panel:ask gpt Should I use a union type or an enum here?
/ai-panel:second-opinion Should we use SSE or WebSockets for real-time updates?
```
