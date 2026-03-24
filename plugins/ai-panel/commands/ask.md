Ask a single AI model for input: $ARGUMENTS

---

## Instructions

The user wants a quick opinion from one specific model. Parse $ARGUMENTS to determine which model and what question.

### Routing

If $ARGUMENTS starts with `gemini` or `g`:

- Strip the model prefix and use the rest as the question.
- Run: `timeout 30 gemini -p "<question> Be concise, 3-5 key points."`
- Clean: remove `Loaded cached credentials.` prefix line.
- Present as: **Gemini says:** followed by the cleaned response, then your own brief take.

If $ARGUMENTS starts with `gpt`, `copilot`, or `c`:

- Strip the model prefix and use the rest as the question.
- Run: `timeout 30 copilot -p "<question> Be concise, 3-5 key points." --model gpt-5.2 --allow-all-tools`
- Clean: remove trailing usage stats block.
- Present as: **GPT says:** followed by the cleaned response, then your own brief take.

If no model prefix is recognised:

- Ask the user which model they want, or default to both (use the `/second-opinion` pattern).

### Examples

- `/ask gemini What auth libraries work best with SvelteKit?`
- `/ask gpt Should I use a union type or an enum here?`
- `/ask g Is Drizzle ORM production-ready for PostgreSQL?`
- `/ask c Best way to handle optimistic updates in a form?`
