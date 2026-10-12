---
name: worktree
description: Use when asked to work in a new worktree or isolated checkout.
---

# Worktree

1. Create exactly one worktree per request with `opencode.worktree_create` and a short task-based `name`. Let the project's configured strategy handle creation.
2. After creation succeeds, call `opencode.session_move` with the returned directory in a separate tool call.
3. Continue the original task from the moved session.

If the tools are unavailable, report the blocker.
