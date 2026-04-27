<!--
  This file is the single task queue for Claude Code.

  RULES:
  - Always OVERWRITE this file entirely. Never append.
  - One task at a time.
  - Running /prompt will execute whatever is currently in this file.

  Paste a new prompt below, replacing everything including this comment block if desired.
-->

# [ACTION_PREFIX]: [Brief description of the task]

## Context

[Why this change is needed. What problem does it solve?]

## Step 1 — [Action]

[Specific instructions with file paths]

## Step 2 — [Action]

[Code examples if needed]

## Step N — TypeScript check, commit, push

```bash
npx tsc --noEmit
git add .
git commit -m "[ACTION_PREFIX]: [brief description]"
git push
```

## Report back

[What Claude Code should confirm when done]
