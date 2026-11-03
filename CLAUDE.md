# Skills repository

Skills (`skills/<name>/SKILL.md`) and system prompts (`prompts/system/`) for coding agents. There is no build and no test suite; the checks under Prove are the gate.

## Isolate and parallel agents
- Reviews, explanations and investigations need no worktree, commit or PR.
- Every change gets its own worktree and branch, one per task and agent, started from `origin/main`. Never commit to `main`; leave other checkouts untouched.
- Before starting, look at open PRs and uncommitted work; ask only on conflict, dependency or unclear ownership.
- The stash stack is shared across worktrees: set work aside with a WIP commit.
- `README.md` lists every skill, so parallel skill branches collide there; rebase on `origin/main` before the PR and keep both entries.
- Force-push only with `--force-with-lease` on your own branch. On unresolvable conflicts, stop and report.

## Conventions
- A skill is `skills/<name>/SKILL.md` with frontmatter `name` and `description`; `disable-model-invocation: true` marks a user-invoked skill.
- Each skill is listed in `README.md` under User-invoked or Model-invoked, with the frontmatter description.
- A skill or prompt derived from outside material names its source and marks what is our own.
- Write or edit skills and this file with `writing-for-agents`.

## Texts for humans
Applies to every text a person reads (commit message, PR title and body, README entry, code comment, final reply). Edit only text you wrote or changed.
- Name the fact. Cut puffery ("pivotal", "testament", "showcase"), promotional words, vague attributions ("experts say") and generic conclusions.
- Write "is" or "has" for "serves as" or "boasts", "use" for "utilize" or "leverage", the actor in active voice, and the mechanism or number instead of the feeling ("fails the build", not "stays close at hand").
- Cut chatbot phrases ("Certainly!", "I hope this helps"), stacked hedges and filler ("it is important to note").
- Say the point directly: no "not just X, but Y", no forced triples, no synonym cycling. Separate thoughts with periods or commas, never em dashes. Sentence-case headings, no decorative emojis, bold only where it carries meaning.
- Every claim names the check from Prove that backs it.
- Before committing or sending, reread each text once and ask what makes it obviously AI-written; fix what you find.

## Prove
- `git diff --check` exits 0.
- For each changed skill: frontmatter parses, relative links resolve, README entry matches the frontmatter.
- Report what ran and what did not. Prompt wording has no runtime check; say so.

## Ship
- Scope the diff to the task, rebase on `origin/main`, open a draft PR, name its URL.
- Report the actual state, including failing or unrun checks.
- Merge only on explicit instruction; keep the worktree until merge or close.
