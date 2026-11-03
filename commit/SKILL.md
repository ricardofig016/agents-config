---
name: commit
description: Create well-formed git commits whose messages accurately summarize every committed change.
disable-model-invocation: true
---

Commit all local changes, structured into logical commits, running every git command yourself. Strictly follow the Conventional Commits specification (https://www.conventionalcommits.org/en/v1.0.0/) for writing commit messages.

## Behavior

1. **Find the repository that owns the changes.** Run `git rev-parse --show-toplevel` in the working directory. If it fails, search the workspace for nested Git roots, then probe directories the request touches and their parents. If more than one repo matches, ask which one.

2. **Inspect the entire change set before choosing a message.** Run `git status --short`, then inspect the content with `git diff`, `git diff --cached`, and a summary such as `git diff HEAD --stat`. Treat tracked, staged, unstaged, deleted, renamed, and untracked files as part of the change set, and ground the message's scope in what the patches show.
3. **Read the actual patches.** For every changed path, identify what behavior, presentation, configuration, generated artifact, or documentation changed. Trace a generated file back to its source change and include that fact in the scope analysis. `git diff HEAD -- <paths>` (or equivalent) covers staged and unstaged content in one view.
4. **Structure the changes into commits.** Default to a single `git add -A` plus one commit; split into multiple commits when the changes span clearly unrelated concerns, such as separate features, fixes, or areas. When splitting, stage each group's paths separately with `git add <paths>` and commit each group in turn.
5. **Default to a title-only message.** Title-only is the norm. A body is for large commits whose changes cannot be summarized in the title. Bodies are discouraged.
6. **Pass the message to Git over stdin.** Build it in PowerShell memory as an array of lines joined with LF, then pipe it to `git commit -F -`. Keep the array in the multiline form shown; do not condense it into an inline array. Do not use `-m`:
   ```powershell
   $message = @(
     'feat(scope): description'
   ) -join "`n"
   $message | git commit -F -
   ```
7. **Verify every commit after it lands.** Run `git status --short` and confirm the intended paths are committed. Repeat the check before retrying a commit command that exited nonzero.

Run the commits immediately; the commit request is the authorization. The user's request names the git operations to run: commits by default, plus push, tag, reset, or rebase when the user names them. To repair a malformed message, such as a stray BOM or a truncated subject, amend the message of an unpushed commit you just created, regenerating the full message, and let the amend cover that message alone.

## Rules

**Hands off the repo.** Run git commands only; don't alter any file in the repo unless the user's request says to, and don't create commit-message temp files. The working tree wins, exactly as it stands. Commit it as-is and report any surprises to the user. Use the `ask_user_question` tool to clarify the user's intent when the change set is ambiguous.
