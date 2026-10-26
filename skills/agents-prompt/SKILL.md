---
name: "agents-prompt"
description: "Build an agent prompt in the fixed four-block template (Goal with optional Steps, Return Format, Warnings, Context Dump). Use whenever the user asks for a prompt from the template, a prompt, brief or task for an agent or subagent."
---

# Agents Prompt

Single source of truth for prompts built together with the user.

Origin: four blocks from "The Anatomy of an Agent Prompt" (artificialintelligence.co). Headings, the optional Steps section and the standard warnings are the user's own form.

## Asking the user

Every open question goes through the AskUserQuestion tool, one question per topic, never as a list in prose. Max. 4 questions per call, 2–4 options each; always offer "Not relevant / no preference" where it fits. If AskUserQuestion is unavailable, stop and say so instead of asking in plain text.

## Process

1. **Restate.** Summarise, in 2–3 sentences, what the prompt is for and which blocks can already be filled from the conversation or the active repo's CLAUDE.md.
2. **Pre-clarify, only what cannot be derived.** Target agent/model; prompt language; environment (repo, tools, MCP, rights, network); autonomy (read and propose / write / execute); existing CLAUDE.md or system prompt (do not repeat what is already there).
3. **Close the gaps per block** using the guiding questions below. Ask only what cannot be derived. Never invent; unresolved gaps stay as visible `<TODO: …>` markers.
4. **Draft** the prompt in the template below as ONE code block.
5. **Present.** Below the block, one sentence per block stating what was assumed.
6. **Approve** via AskUserQuestion: "Approve" / "Change" / "Deepen with agents-deepener" (only if that skill exists). Do not write the prompt to a file unless the user asks.

## Template

````
# Goal

<One paragraph: the main task, the expected outcome and how success is recognised (prefer checkable: tests green, command output, file exists).>

<If work is delegated: who does what, e.g. "You (the orchestrator) investigate, decide, write the briefs and verify. Implementation is delegated to subagents with `model: …`.">

Scope: <what is in; what is explicitly out; what happens with anything noticed outside scope (report as observation, do not fix).>

## Steps

1. **<Verb>.** <What to do, with what input, what it produces.>
2. **<Verb>** (<read-only / in own context / delegated>). <…>
3. **Plan, then STOP.** <What to present; write nothing until I approve; where a design decision is needed, ask.>
4. **<Verb>.** <…>
5. **Verify.** <Re-run verification yourself; do not rely on reports; review diffs for scope creep and weakened checks.>
6. **Report** in the format below.

# Return Format

<Per recipient if delegated: e.g. "Subagent reports (no prose): …".>

<Final report to me: language, max. length, structure (table columns, sections), order. Where to stop for approval. Assumptions and open points listed separately, not silently filled.>

# Warnings

<Task-specific: what must never be done; irreversible actions (delete, overwrite, push, send); money, rights, sensitive data; what to do when in doubt (stop and ask).>

- Minimal, surgical edits. No refactoring, renaming, reformatting or "while we're at it" changes.
- Never make a check, test or gate pass by weakening or skipping it. Fix the cause.
- If something cannot be proven, say so and name the smallest step that would prove it. Do not ship a guess.
- Do not commit, push or open an MR unless the repo's CLAUDE.md prescribes it or I ask.
- Do not invent criticism or findings. Mark statements as Finding (evidence), Assumption or Opinion. "No change needed" is a valid result.
- For every proposal name its cost and one simpler alternative.
- Ask open questions one at a time with AskUserQuestion; do not guess.

# Context Dump

- <Why it matters / what depends on it.>
- <History: what failed or was tried before.>
- <Preferences and trade-offs.>
- <Environment and conventions: follow the active repo's CLAUDE.md (branch rules, commit delegation, language conventions); mandatory checks with exact commands; known pitfalls.>
- <Who reviews or uses the result.>
````

Rules for the template:
- Headings exactly `# Goal`, `## Steps`, `# Return Format`, `# Warnings`, `# Context Dump`, in this order. No `--` separator.
- `## Steps` is optional: use it for multi-step or delegated tasks, omit it for single-shot tasks. Include a "Plan, then STOP" step whenever the agent may write or execute.
- The standard warnings stay unless clearly irrelevant (e.g. drop commit/MR lines for a non-code task); say in the assumption sentence which were dropped and why.
- Do not repeat in the prompt what the target environment's CLAUDE.md already enforces; reference it instead.
- Remove all `<…>` placeholders before approval; unresolved ones become `<TODO: …>` and are named.

## Guiding questions (ask only if not derivable)

- **Goal:** task in one sentence (verb + object); resulting artifact and location; checkable success criterion; constraints; explicitly out of scope; inputs and where they live.
- **Steps:** which steps, which are read-only, where to stop for approval, what is delegated and to which model.
- **Return Format:** content and order; form (table, list, file); language and length; one or several options and ranking; reasons and uncertainty; stop points; separate list of assumptions.
- **Warnings:** forbidden actions; irreversible actions; money, rights, sensitive data; behaviour under doubt; whether limits are technically enforced or only stated in the prompt.
- **Context Dump:** purpose; history; preferences; team, tools, pipelines, conventions; stakeholders; anything else that changes the approach.