---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

## Branch

- **Start on a fresh branch when on the default branch** (`main` / `master` / whatever the repo's protected branch is — ask if it's not obvious). Cut one before any work lands. Branch name is `feat/<short-kebab-slug>` or, if the spec names an issue number, `feat/issue-<N>-<short-slug>`.
- **Stay on the existing branch if the user already named it** (e.g., when this skill is invoked from `implement-spec` or after a handoff). Re-cutting breaks the user's setup.

## Loop

Use /tdd where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Small commits, each green. Drive-by-fixes and adjacent refactors don't belong in this branch — call them out in the PR body instead.

## Review

Once the AC is met and tests are green, use /code-review to review the work before pushing. Fix what it raises; then ship.

## Ship

1. **Push the branch.** `git push -u origin <branch>` (force-with-lease only if rewriting published history).
2. **Open a PR.** Use `/pr` to author the body from the spec / issue, then `gh pr create --base <default-branch> --head <branch>`. Reference any closing issues (`Closes #N`) when the spec names them.
3. **Surface anything the spec didn't decide** in the PR body or a follow-up comment — don't quietly resolve it.

If `gh` isn't authenticated, the repo isn't on GitHub, or the push fails: stop, surface the failure, and ask. Don't paper over it.
