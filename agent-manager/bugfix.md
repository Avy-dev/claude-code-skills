---
name: bugfix
description: "Structured 7-step bug fix workflow: reproduce, diagnose, plan, implement, test, verify, update context. Dispatches to domain agents for multi-domain bugs."
user_invocable: true
---

# /bugfix — Structured Bug Fix Workflow

You are executing a structured bug fix. Follow these 7 steps in order. Never skip diagnosis — state the root cause before writing code.

**Bug description:** $ARGUMENTS

---

## Step 1: Reproduce
- Identify the bug location from the description
- Find the relevant file(s) and read the affected code
- Confirm you can trace the failure path

## Step 2: Diagnose
- Trace the data flow through the affected code path
- Check MEMORY.md "Lessons Learned" for known patterns that match this bug
- Check session-context.md for recent changes that may have introduced it
- Identify the root cause — do NOT proceed until you can state it in one sentence

## Step 3: Plan Fix
- State the root cause in 1 sentence
- Outline the fix in 3-5 bullets: what changes, where, why
- If the fix spans multiple domains (backend + frontend, DB + API, etc.), identify which domain agents to dispatch to and in what order
- Display the plan inline before implementing

## Step 4: Implement
- Make the smallest correct change that fixes the root cause
- If multi-domain, dispatch to the appropriate domain agent(s):
  - Backend before frontend
  - Database before API
  - API before UI
- Do not refactor surrounding code unless it is part of the root cause

## Step 5: Test
- Run relevant existing tests
- If no test covers this bug, write a minimal regression test
- Report test results

## Step 6: Verify
- Re-read the changed code to confirm correctness
- Check for regressions: did the fix break any adjacent behavior?
- Check for the patterns in MEMORY.md Lessons Learned (XSS, caching, field contracts, etc.)

## Step 7: Update Context
- If this bug reveals a new pattern or lesson, add it to session-context.md
- If the pattern is broadly applicable, suggest adding it to MEMORY.md Lessons Learned
- Summarize: root cause (1 line), fix (1 line), files changed

---

## Rules
- Never skip Step 2 (Diagnose). Jumping to a fix without understanding the cause leads to incomplete fixes.
- Fix backend before frontend when the bug spans both systems.
- If the bug is in a domain with a specialist agent (UI, DB, API, etc.), dispatch to that agent for Step 4.
- Do not commit or push — leave changes for user review.
