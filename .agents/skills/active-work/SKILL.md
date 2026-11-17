---
name: active-work
description: Give a current snapshot of active and unfinished work across Nuanced project threads, covering running turns, threads waiting on the user, background watchers and scheduled continuations, Home wake-ups and dependencies, and finished turns with unresolved follow-up. Use when the user asks what is going on, what is in flight, the status of active work, or what is still open. Read-only. For what changed since an earlier point in the conversation, use catch-up instead.
---

# Active Work

Report the state of work right now: who owns it, what is done, what is next, and what is actually blocked.

## Gather

1. `list_workspaces`, then `list_threads` for the relevant workspaces; omit `workspace_id` when the user asked about everything.
2. Candidates are every running or waiting thread, plus every idle thread, regardless of age, whose last result is missing or does not clearly say the work is done. A last result mentioning an open PR, CI, a watcher, a scheduled continuation, a demo awaiting review, or a next step makes it a candidate. Idle status alone does not mean the work is finished.
3. `thread_status` on each candidate for live activity, background tasks, the last outcome, the worktree, and GitHub PR and release state. Use `read_thread` when the last result is missing or status leaves the next step unclear. No tool lists a thread's own scheduled continuation, so rely on what its recent messages say.
4. `read_pending_prompt` for threads waiting on the user, so you can say what they need.
5. `list_wakeups` and `list_dependencies` with `include_settled: false` for Home's own pending checks.

Gather once. Do not poll, schedule wake-ups, send messages, or resume threads just to produce the report.

## Report

Group by state, omitting empty groups:

- **Needs you:** pending questions, approvals, failures, and decisions, each with the decision stated plainly.
- **Running:** live turns, and finished turns with active watchers or continuations.
- **Waiting on something else:** CI, review, a dependency, or a scheduled time.
- **Done but not shipped:** open PRs, unreleased merges, and demos awaiting review.

For each item, give the thread and its owner, what is done, and what is next. Keep implemented, validated, demoed, merged, released, and installed distinct, and claim merged or released only when `thread_status` confirms it. Link every thread you name as `[Exact title](nuanced-thread://<workspace_id>/<thread_id>)`.

## Boundaries

A status request authorizes reading only. Do not answer questions, stop, archive, merge, release, or change settings; recommend the action instead.
