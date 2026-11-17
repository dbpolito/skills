---
name: worktree
description: Create a worktree using the project's configured strategy for a generic worktree or isolated checkout request. For explicit Rift requests, use the Rift skill.
---

# Worktree

1. Create exactly one worktree per request with `opencode.worktree_create` and a short, task-based `name`. It may be used as a subdomain, so keep it DNS-safe and within 63 characters including any added prefixes and suffixes. Let the project's configured strategy handle creation.
2. After creation succeeds, call `opencode.session_move` with the returned directory in a separate tool call.
3. Continue the original task from the moved session.

If creation succeeds but the move fails, report the existing worktree path and blocker. Retry only the move to that path, not creation. If the tools are unavailable, report the blocker.
