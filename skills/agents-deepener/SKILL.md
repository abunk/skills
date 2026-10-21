---
name: "agents-deepener"
description: "Deepen a prompt, skill source text or other instructions with Felix Rieseberg's four questions (expert, raw material, methods, independent verification), review it, return an approved version."
disable-model-invocation: true
---

# Agents Deepener

Makes ONE finished task description (prompt built with the agent prompt template: Goal, Return Format, Warnings, Context Dump; skill source text; or other instructions) *deeper, not bigger*, reviews it, and returns it for approval.

Source: Felix Rieseberg, "How to be more ambitious: A tutorial for humans with models" (felixrieseberg.com, 2026-10-13). The four questions paraphrase his prompt; only its opening ("Before rewriting it: (1) say what a top professional in this field would do here that I'm not asking for;") is verbatim. Steps marked [own] are additions. He gives no measurements; present the benefit as his experience.

## Rules

- Chat with the user in German; rewritten text in English, whatever the input language.
- Deliver every result in chat; saving files is the user's step.
- Put every question and open point to the user via AskUserQuestion.
- Out of scope: creating the initial text, changing the wiki.

## Input

Arrives as `<Input type="prompt|skill-source|text">…</Input>`; `<Prompt>…</Prompt>` counts as `type="prompt"`.
- Missing or empty → ask for it, stop.
- No type → infer: template blocks present → `prompt`; workflow instructions meant to become a skill → `skill-source`; else `text`. Unclear → ask.
- `prompt` without the template blocks → say so in one line, ask whether to continue. Building the blocks is the user's step.

## Step 1 — Deepen in a fresh context [own]

Spawn a general-purpose subagent so this conversation cannot bias the rewrite. Pass it ONLY the block below, with TYPE and INPUT inserted. {TYPE_NOTE} = "The text is the source from which a reusable skill will later be built." for `skill-source`, else empty. No Agent tool → do it yourself and tell the user the context was not fresh.

```
I want a deeper version of this task description, not a bigger one. {TYPE_NOTE}

<input type="{TYPE}">
{INPUT}
</input>

Before rewriting it, answer four questions:
1. What would a top professional in this field do here that the text is not asking for?
2. What is the rawest material (original data, primary sources) this professional would start from, instead of already processed information?
3. Which specific methods and tools would this professional use?
4. What, besides you and me, would show that the result is correct?

Then rewrite the original text so it asks for exactly these things.

Rules:
- Output only the four answers, the rewritten text and the list of changes. Do NOT carry out the task described in the text.
- Keep the original's structure (for prompts: Goal, Return Format, Warnings, Context Dump); extend it.
- Answers in German, rewritten text in English.
- Name only sources, datasets and tools you can point to; mark anything unverifiable or inaccessible as an assumption.
- Deeper, not bigger: targeted changes, no full rewrite, at most ~30% longer than the original.

Return format:
## Antworten
1. … 2. … 3. … 4. …
## Überarbeiteter Text
<text>…</text>
## Änderungen
- <change> (<section>): question <1–4>
```

## Step 2 — Review [own]

Check the subagent's output yourself. Label every point Befund (belegt), Annahme or Meinung. Done when every check below has a result.

- Q1: real depth or just more scope?
- Q2/Q3: do the named data, sources and tools exist, and can the executing agent use them? Verify by cheap search; else Annahme.
- Q4: is the check independent of model and author, and executable?
- Original vs. rewrite, section by section: structure kept, nothing requested lost, length within ~30%, no new assumptions or scope creep.
- New mandatory steps: fit every case the original allows, proportionate, safe to carry out.

Raise only real findings; "passt so" is valid. Give each proposed correction its cost and a simpler alternative.

## Step 3 — Present, approval gate

Show: (1) the four answers, (2) the rewritten text, (3) findings with proposed corrections plus a short "Bleibt so" list. Ask which corrections to apply; continue only after approval.

## Step 4 — Finalize

Apply only approved corrections; show the final text in a code block. Offer, acting only on the user's choice:
- (a) Default, all types: the user takes it and uses it themselves, e.g. in a new session.
- (b) `prompt`: run it now in a fresh subagent.
- (c) [own] `prompt`: A/B: run original and final in two fresh subagents; mention the extra cost.
- (d) [own] `skill-source`: hand the final text to the skill-creator skill, passing the Q4 answers as proposed eval cases.

## Step 5 — Independent check (only after b or c) [own]

Spawn a separate subagent that sees ONLY the result(s) and the Q4 acceptance criteria, never the creation process. Report its verdict per criterion. For (c), judge both results against the same criteria and state plainly if the original was as good.
