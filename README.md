# Skills

## Reference

### Engineering

**User-invoked**

- **[agents-deepener](./skills/agents-deepener/SKILL.md)**: Deepen a prompt, skill source text or other instructions with Felix Rieseberg's four questions (expert, raw material, methods, independent verification), review it, return an approved version.

**Model-invoked**

- **[agents-prompt](./skills/agents-prompt/SKILL.md)**: Build an agent prompt in the fixed four-block template (Goal with optional Steps, Return Format, Warnings, Context Dump). Use whenever the user asks for a prompt from the template, a prompt, brief or task for an agent or subagent.

### System prompts

Customizing for [omp.sh](https://github.com/can1357/oh-my-pi), not a skill.

- **[omp-system-prompt](./prompts/system/omp-system-prompt.md)** (WIP): A collaboration-focused system prompt (honesty, understand before acting, smallest sensible change, reversibility, evidence). Adopting it replaces the instruction block of the system prompt that omp ships with. The file is a work in progress and its wording may still change.

**What gets replaced.** As a plain-text `SYSTEM.md`, the file replaces omp's default instruction template: its role text, tool inventory and tool policy, and workflow rules. omp still adds discovered skills, rules and the generated `<project-context>` footer. If you still need the default tool policy or workflow, you have to carry that guidance over yourself; selectively inheriting parts of the default prompt is not supported. To only add instructions on top of the default prompt, use `APPEND_SYSTEM.md` instead.

**How to use it.** Copy the file to one of these locations:

```sh
# Project-local (takes precedence)
mkdir -p .omp && cp prompts/system/omp-system-prompt.md .omp/SYSTEM.md

# Global (fallback)
mkdir -p ~/.omp/agent && cp prompts/system/omp-system-prompt.md ~/.omp/agent/SYSTEM.md
```

Alternatively, pass it for a single run: `omp --system-prompt prompts/system/omp-system-prompt.md`. An explicit flag wins over both files. Restart omp after changes and use `/dump` in a session to inspect the prompt that is actually sent.

Further options (Handlebars templates via `SYSTEM_TEMPLATE.md`, `APPEND_SYSTEM.md`, `PERSONALITY.md`, `TITLE_SYSTEM.md`) are described in the [omp documentation](https://github.com/can1357/oh-my-pi/blob/main/docs/system-prompt-customization.md).
