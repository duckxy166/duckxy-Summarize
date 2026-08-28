---
name: duckxy-summarize
description: Summarize the current Codex task with evidence-backed status, completed work, changes, validation, and handoff notes. Use only when the user explicitly invokes $duckxy-summarize; do not use for general document or meeting summaries.
---

# Duckxy Summary

Produce a concise summary of the work performed in the current task. Treat any text after the invocation as a requested focus, while retaining the core summary.

## Build the evidence set

- Start with the current task conversation, including tool results, decisions, and stated acceptance criteria.
- Use issue details already present in the task or supplied by the user when they clarify the goal or outcome.
- When a repository is available, use read-only inspection of relevant changed files, `git status`, staged and unstaged diffs, and commits demonstrably made during this task.
- Do not attribute every dirty-tree change to the task. Include a change only when the conversation, tool history, or an established baseline supports that attribution; label uncertain attribution instead of guessing.
- Report only tests and checks that actually ran, with their observed result. Do not run new tests merely to make the summary look complete.
- If repository, issue, or test evidence is unavailable, summarize from the available task context and state the evidence gap.
- Redact secrets, credentials, tokens, personal data, and other sensitive values.

This is a read-only reporting skill. Do not edit workspace files, create commits or branches, or post or update an issue tracker. A separate explicit user request is required for any such action.

## Determine status

Choose one status and support it with evidence:

- **เสร็จ** / **Complete**: the requested outcome is achieved, required validation is satisfied, and no required work remains.
- **เสร็จบางส่วน** / **Partial**: meaningful work is complete, but required work, validation, or a decision remains.
- **ติดขัด** / **Blocked**: progress cannot continue without user input, authorization, or an external state change.

Never claim completion, a passing test, or a user-visible behavior without evidence. Distinguish completed work from proposed, attempted, reverted, failed, or remaining work.

## Write the summary

Respond in the user's language; default to Thai when it cannot be inferred. Keep the result compact and use these headings, translated when appropriate:

1. **สถานะ** — the selected status and a one-sentence reason.
2. **งานนี้ทำอะไร** — the task goal and current outcome.
3. **ทำอะไรไปแล้ว** — the significant actions completed during this task.
4. **เปลี่ยนอะไรไปบ้าง** — observable behavior first, then important files, APIs, schemas, configuration, or dependencies only when relevant. Exclude unrelated pre-existing changes.
5. **ตรวจสอบอะไรแล้ว** — tests, builds, linting, manual checks, or review actually performed and their results; say clearly when validation did not run or is incomplete.
6. **ฉันต้องรู้อะไรบ้าง** — user impact, breaking changes, required manual steps, risks, limitations, unresolved decisions, and remaining work. State that there is nothing notable only when the evidence supports it.

Prefer outcome-oriented bullets over a chronological transcript or exhaustive diff. Include exact commands, file paths, issue identifiers, or commit hashes only when they materially help the user verify or continue the work.
