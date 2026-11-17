# Rift

The README marks this skill archived and recommends [worktree](worktree.md). Its Rift-specific manifest remains present and describes the behavior below.

## Behavior

When Rift is explicitly requested, create one worktree with `rift.create`, using a short task-based name when apparent. After successful creation, move the session to the returned directory with `opencode.session_move` in a separate call and continue the original task.

If the session move fails, report the existing workspace path and blocker. Retry only the move to that path. If Rift tools are unavailable, report a blocker rather than substituting a manual Git worktree.

## Evidence

Static observations: `skills/rift/SKILL.md` and `README.md`. No Rift workspace was created.
