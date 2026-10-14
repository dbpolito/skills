# Worktree

Ask to work in a new worktree or isolated checkout. Requires OpenCode's `opencode.worktree_create` and `opencode.session_move` tools.

## Behavior

Create exactly one worktree per request with a short task-based name. Let the project's configured strategy handle creation. After creation succeeds, move the current session to the returned directory in a separate tool call, then continue the original task there.

Report a blocker if the tools are unavailable. The README notes that the configured strategy can use Rift; the skill itself does not select a specific backend.

## Evidence

Static observations: `skills/worktree/SKILL.md` and `README.md`. No worktree or session movement was executed.

## Open questions

- The manifest does not specify recovery when creation succeeds but session movement fails. Retrying the entire request could create another worktree; recovery behavior needs clarification before being treated as a requirement.
