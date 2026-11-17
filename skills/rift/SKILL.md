---
name: rift
description: Create and switch to a Rift workspace when the user explicitly requests Rift.
---

# Rift

1. Create one worktree with `rift.create`. Use a short task-based name when a useful name is apparent.
2. After creation succeeds, call `opencode.session_move` with the returned directory in a separate tool call.
3. Continue the original task from the moved session.

If creation succeeds but the move fails, report the existing workspace path and blocker. Retry only the move to that path, not creation. Report unavailable Rift tools as a blocker rather than substituting a manual Git worktree.
