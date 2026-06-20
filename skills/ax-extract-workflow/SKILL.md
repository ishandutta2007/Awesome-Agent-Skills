---
name: ax-extract-workflow
description: Reconstructs the workflow behind a shipped artifact from ax session evidence, commits, and PR context. Use when asking what made a feature, fix, demo, or PR work, or when turning a past success into a repeatable recipe.
---

# ax Extract Workflow

## Overview

Shipping something once is not the same as knowing how to repeat it. This skill reconstructs the ordered workflow behind a past artifact: the framing decisions, implementation path, review gates, verification evidence, and user steering points that made it land.

Use [ax](https://github.com/Necmttn/ax) as the primary evidence source when it is installed, because it can connect sessions, turns, skills, tool calls, commits, and cost data. If ax is unavailable, use git history, PR comments, issue threads, CI logs, and local notes, but label the reconstruction as lower confidence.

## When to Use

- The user asks "what made this work?", "how did we ship this?", or "extract the workflow behind this PR"
- A feature, demo, refactor, migration, or incident fix worked and the team wants the reusable recipe
- You need to explain which skills or process steps actually mattered, not just summarize the final diff
- You are preparing a retrospective, playbook, onboarding note, or repeatable implementation guide

**NOT for:**
- Generic activity summaries or "what happened today" reports
- Root-causing a live bug; use `debugging-and-error-recovery` first
- Writing a new implementation plan from scratch; use `spec-driven-development` and `planning-and-task-breakdown`

## Process

### 1. Resolve the anchor

Identify the artifact the user wants to reconstruct:

| Anchor | Evidence to gather |
|---|---|
| Commit SHA | `ax sessions near <sha>` plus `git show <sha>` |
| PR URL or number | PR description, comments, review threads, checks, and commits |
| Date or release | `ax sessions around <date>` plus release notes or tags |
| Topic or feature name | `ax recall "<topic>"` to find candidate sessions and commits |

If the anchor is ambiguous, ask the user to choose before narrating.

### 2. Build the evidence window

Collect the smallest window that explains the artifact:

1. Start from the anchor session, PR, or commit.
2. Include predecessor sessions that introduced the idea, plan, or failed attempt.
3. Include follow-up sessions that repaired tests, changed direction, or finished verification.
4. Exclude unrelated nearby work, even if it happened on the same day.

When using ax, prefer:

```bash
ax sessions near <sha> --json
ax sessions show <id> --json
ax sessions show <id> --by-role
ax recall "<topic>" --sources=turn,commit,skill --scope=here
```

### 3. Extract the ordered workflow

Turn the evidence into an ordered arc:

1. **Framing:** the initial question, constraint, or user correction that shaped the work.
2. **Planning:** the spec, decomposition, or decision map that prevented thrash.
3. **Execution:** the implementation slices, dispatches, or edits that moved the artifact forward.
4. **Verification:** tests, checks, reviews, screenshots, logs, or CI evidence.
5. **Repair:** failed checks, regressions, or review feedback and how they were closed.
6. **Packaging:** docs, PR body, release note, or final artifact.

Each step must cite evidence: session id, commit SHA, PR comment, test output, or file path.

### 4. Name the reusable decisions

Pull out 2-5 decisions that changed the outcome. Good decisions are specific:

- "Scoped Codex ingest by cwd before normalizing other providers"
- "Moved visual judgment to a subagent to keep screenshots out of main context"
- "Ran the repo validator before opening the PR"

Avoid fake lessons like "communicate clearly" or "write better tests." If the lesson could apply to any project, it is not extracted enough.

### 5. Write the recipe

Return three sections:

1. **Workflow Arc:** numbered steps with evidence references.
2. **Key Decisions:** what changed the path and why it mattered.
3. **Repeatable Recipe:** the shortest version someone can follow next time.

Keep it factual. Do not turn uncertainty into a confident story.

## Common Rationalizations

| Rationalization | Why It Is Wrong |
|---|---|
| "The final commit tells the story." | The commit omits failed paths, user steering, verification churn, and subagent work. |
| "I can infer the process from the diff." | Diffs show what changed, not why the sequence worked. Use session, PR, and test evidence. |
| "Nearby sessions are probably relevant." | Time proximity is weak evidence. Include only sessions tied to the artifact by files, topic, commit, or explicit mention. |
| "A polished narrative is enough." | The output must be auditable. Every important claim needs a source. |

## Red Flags

- The answer reads like a project summary instead of an ordered workflow
- No session ids, commit SHAs, PR URLs, checks, or file paths are cited
- It ignores failed attempts or repair loops
- It overstates confidence when ax data is missing or incomplete
- The reusable recipe is too vague to guide the next run

## Verification

Before finishing, confirm:

- The artifact anchor is explicit and unambiguous
- Every workflow step has evidence
- The story includes both successful actions and meaningful repair loops
- The repeatable recipe names concrete steps, tools, and checks
- Any missing ax data or uncertain inference is labeled clearly
