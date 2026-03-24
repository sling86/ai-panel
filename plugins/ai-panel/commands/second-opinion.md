Get second opinions from Gemini and GPT on: $ARGUMENTS

---

## Instructions

You have access to two other AI models via CLI. Consult both for their perspective on the user's question.

### Step 1 — Call both models in parallel

Run these two Bash commands simultaneously (in a single message), with timeout protection:

**Gemini:**

```bash
timeout 30 gemini -p "$ARGUMENTS Be concise, 3-5 key points."
```

**GPT (via Copilot):**

```bash
timeout 30 copilot -p "$ARGUMENTS Be concise, 3-5 key points." --model gpt-5.2 --allow-all-tools
```

### Step 2 — Clean the output

- **Gemini**: Remove any `Loaded cached credentials.` prefix line.
- **GPT**: Remove trailing usage stats (everything from the blank line before `Total usage est:` onwards).
- **Timeout**: If either model timed out (exit code 124), note it and proceed with whichever responded.

### Step 3 — Present the results

Format your response as:

**Gemini says:**

> <cleaned Gemini response>

**GPT says:**

> <cleaned GPT response>

**My take:**
Synthesise the responses. Note where the models agree, where they disagree, and give your own recommendation weighing all perspectives. Be direct about which approach you'd choose and why.
