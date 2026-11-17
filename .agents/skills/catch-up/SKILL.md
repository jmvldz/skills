---
name: catch-up
description: Summarize what materially changed across Nuanced project threads since a conversational checkpoint, by default the user's previous substantive message to Home. Use when the user asks to catch them up, what happened or changed since their last message, while they were away, since a time, or since a named event. Read-only. For a snapshot of what is running or unfinished right now regardless of when it changed, use active-work instead.
---

# Catch Up

Report the delta since a checkpoint: what was delivered, what newly blocks or needs a decision, and whose status changed.

## Checkpoint

- Use the checkpoint the user names (a time, an event, "since this morning"). Otherwise use their previous substantive message in this conversation. Automatic thread updates, wake-ups, and dependency notifications are not checkpoints.
- If the conversation was reset or does not reach back far enough, say which checkpoint you used instead and what history you could not see. Do not invent an earlier baseline.

## Gather

1. Start from what this conversation already shows after the checkpoint: thread updates, wake-ups, and results Home received.
2. Use `list_workspaces` and `list_threads` to find threads whose activity or status could have changed since the checkpoint. Include threads archived in the window with `search_threads` when relevant.
3. Use `read_thread` only on threads that likely changed, with a modest `max_chars`.
4. Before saying anything merged or released, confirm it with `thread_status` (GitHub-backed). A finished turn is not a merge. Say "implemented", "validated", "merged", "released", or "installed" only as far as the evidence goes.

## Filter

- Collapse repeated watcher, CI-poll, wake-up, and "no change" updates into the one state they ended at, or drop them if nothing changed.
- Leave out threads with no material change.
- Prefer one line per change over a chronological log.

## Report

Open with the checkpoint used. Then, omitting empty sections:

- **Delivered:** merged, released, or completed work, each with its PR or release link.
- **Needs you:** new decisions, pending questions, approvals, and failures.
- **Changed status:** started, stalled, or newly waiting work.

Link every thread you name as `[Exact title](nuanced-thread://<workspace_id>/<thread_id>)`.

## Boundaries

A catch-up request authorizes reading only. Do not resume, message, answer, stop, archive, merge, release, or schedule anything because of what the report finds; recommend the next step instead.
