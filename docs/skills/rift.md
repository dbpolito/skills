# Rift

The README marks this skill archived and recommends [worktree](worktree.md). Its Rift-specific manifest remains present and describes the behavior below.

## Behavior

When a Rift workspace is requested, create one worktree with `rift.create`, using a short task-based name when apparent. After successful creation, move the session to the returned directory with `opencode.session_move` in a separate call and continue the original task.

Do not create another worktree after moving. Do not substitute a manual Git worktree when Rift was requested. Report a blocker if Rift tools are unavailable.

## Evidence

Static observations: `skills/rift/SKILL.md` and `README.md`. No Rift workspace was created.

## Open questions

- Does “archived” mean only that installation is discouraged, or should invocation be disabled? The manifest still has an active trigger description; this baseline preserves both observations without changing it.
