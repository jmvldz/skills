---
name: clean-up-threads
description: Archive clearly completed or abandoned Nuanced project threads in the requested workspaces while preserving anything still active, waiting, or holding unique unfinished work. Use when the user asks to clean up, tidy, or archive unused, old, finished, or stale threads. Invoking it authorizes archiving only. Not for status reports, which never archive.
---

# Clean Up Threads

Invoking this skill authorizes `archive_thread` on threads in scope that are clearly finished or abandoned. It authorizes nothing else.

## Scope

Use the workspaces the user named. If they did not name any, use the current project when the conversation makes it clear. Ask only when several workspaces are plausible. Do not widen the scope on your own.

## Inspect before archiving

1. `list_threads` for each workspace in scope.
2. `thread_status` on each candidate.
3. Rule out a pending continuation for each candidate you might archive. No tool lists a thread's own scheduled continuation, and archiving cancels it. Run `search_threads` for `continuation` in the workspace, then `read_thread` on each candidate. A thread that ever scheduled a continuation is ruled out only when a later message shows that continuation ran or was canceled. If you cannot rule one out, keep the thread.
4. `list_wakeups` and `list_dependencies` with `include_settled: false` to find threads Home is still watching.

Archive a thread only when one of these holds:

- Its work is complete: merged or released as GitHub confirms, or a finished report or answer that needs no further action.
- It is an empty, failed, or abandoned run with nothing worth keeping.

Keep it otherwise, including when it:

- is running, has a background job or watcher, or may have a pending continuation or a plan to check back, even if its last turn finished;
- is waiting on the user, or a Home wake-up or dependency references it;
- has an open PR, unaddressed findings, or a stated next step;
- holds a unique deliverable that still needs action, such as an unreviewed demo or an unmerged branch;
- is idle without proof of completion. Idle alone is not done.

## Archiving

- Call `archive_thread` without `discard_uncommitted_changes`. If it refuses because of uncommitted or unchecked changes, keep the thread and report it. Set `discard_uncommitted_changes` only when the user explicitly agrees for that thread.
- History and branches are kept. If the result says the worktree will be removed after the configured delay, tell the user.
- Do not stop threads, cancel continuations, wake-ups, or dependencies, delete branches or cloud resources, merge, change agent settings or credentials, or set up future automatic archiving.

## Report

Give counts: archived, kept, and refused. Name what is still active or blocked and why. Do not list every archived thread unless asked. Link every thread you name as `[Exact title](nuanced-thread://<workspace_id>/<thread_id>)`.
