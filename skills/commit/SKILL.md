---
name: commit
description: Create well-formed git commits whose messages accurately summarize every committed change.
disable-model-invocation: true
---

Commit your changes, structured into logical commits, running every git command yourself. Strictly follow Conventional Commits 1.0.0 for writing commit messages.

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

**Hands off the repo.** Run git commands only; don't alter any file in the repo unless the user's request says to, and don't create commit-message temp files. The working tree wins, exactly as it stands. Commit it as-is and report any surprises to the user. Ask the user to clarify their intent when the change set is ambiguous.

## Conventional Commits 1.0.0

```text
<type>[(<scope>)][!]: <description>

[body]

[footer(s)]
```

### Specification

1. Start with a noun as the type, optionally add a scope and `!`, then write `: `.
2. Use `feat` for new features.
3. Use `fix` for bug fixes.
4. An optional scope is a parenthesized noun naming a codebase area.
5. Immediately after `: `, briefly summarize the changes.
6. Begin an optional body after a blank line.
7. The body can contain any number of free-form paragraphs.
8. Begin optional footers after a blank line. Each takes a token, `: ` or ` #`, and a value.
9. Hyphenate multiword tokens except `BREAKING CHANGE`.
10. Values can span lines. The next valid token and separator end the value.
11. Every breaking change needs a marker in the subject prefix, a footer, or both.
12. The footer form is `BREAKING CHANGE: <description>`, with an uppercase token.
13. The subject form puts `!` immediately before `:`. If the footer is omitted, describe the break in the subject.
14. Other types are permitted.
15. Parsers treat the format case-insensitively except for the uppercase breaking-change token.
16. `BREAKING-CHANGE` is equivalent to `BREAKING CHANGE` as a footer token.

### Examples

```text
docs: clarify the configuration search order
```

```text
feat(cli)!: require an explicit output directory
```

```text
fix(cache): invalidate entries when a workspace closes

The cache now clears entries when the last editor closes the workspace.

Reviewed-by: Morgan
Refs #418
```

```text
refactor(api): simplify pagination responses

BREAKING CHANGE: replace the nextPage field with a nextCursor string
```

For SemVer, breaking changes signal MAJOR, `feat` signals MINOR, and `fix` signals PATCH. Other types have no implicit version effect.

Adapted from the [Conventional Commits 1.0.0 specification](https://www.conventionalcommits.org/en/v1.0.0/) under [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/).
