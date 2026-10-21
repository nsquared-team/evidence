## Goal

People who comment on or review PRs in this repo get an Asana sync check that treats their text as data, so a comment with backticks no longer turns the check red and nobody can run commands on the runner by writing one.

Scope: one responsibility, the Asana sync workflow's two logging steps, changed the same way as the synced copy in devops: step outputs reach the script through `env:` as quoted shell variables. ⏩ Out: every other step in the workflow. Shape: 1 workflow file plus this plan, about 25 lines.
Risk: the logging steps only; the steps that post the Asana comment and attachment don't change. Undo: revert the commit. The load-bearing 1%: the printed log lines stay the same.

## Definition of Done

- [x] No `run:` block in the Asana sync workflow pastes a `${{ }}` step output into the script.
- [x] A comment body containing backticks and `$(...)` prints as literal text when the step's script runs under bash.
- [x] `actionlint` passes on the workflow file.
