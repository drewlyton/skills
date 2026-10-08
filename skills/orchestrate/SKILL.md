---
name: orchestrate
description: "Take a GitHub issue to a finished PR, dispatching subagents for research, implementation, and review."
disable-model-invocation: true
argument-hint: "<github issue number>"
---

# Orchestrate

You are the **orchestrator**. Your goal is to implement `gh issue <number>` and ship it as a PR. Cut a fresh branch off `origin/main` first, then complete the issue by understanding the problem, working iteratively, breaking the work down, and **dispatching subagents** for research, implementation, review, and anything else the work needs.

You coordinate; subagents hold the code. Your context stays on the plan — the issue, the task graph, and the pointers that connect them.

## Branch

`git fetch origin && git checkout -b feat/issue-<N>-<slug> origin/main`. Stay on an existing branch if one for this issue already exists.

## Vocabulary

- **Task graph** — the issue broken into pieces, with the blocking relationships between them.
- **Frontier** — every task whose blockers are done. Work the frontier, then recompute it.
- **Context pointer** — a reference to out-of-context material (the issue, a research note, a commit, a file) handed to a subagent instead of restated. Briefs stay sparse.
- **Dispatch** — hand a task to a subagent with a brief and the condition that says it's done.

## Subagents

Dispatch one when it buys something: parallelism, a clean context, or an independent pair of eyes. Do the work directly when it doesn't.

- **Research** — for what you don't know: external docs, APIs, library behaviour, the issue's history. Call the Skill tool with "research"; it writes notes you can point later subagents at.
- **Implementer** — for a frontier task. Give each its own branch (a worktree keeps them out of each other's way), a brief built from context pointers, and a completion criterion. Run them in the background so several stay in flight, and have each call the Skill tool with "tdd".
- **Reviewer** — an independent pass over finished work. Call the Skill tool with "code-review".

## Loop

Read the issue (`gh issue view <N> --comments`), follow its links, and read the code it touches until you can state the problem and the plan. Then work the frontier: dispatch, merge, recompute, repeat until the graph is empty. Run the project's checks — typecheck, then the touched tests, then the full suite — as you go and once at the end.

## Ship

Once the acceptance criteria are met and the checks are green, review the branch (Skill: "code-review") and fix what it raises. Then push and open a PR: author the body with Skill "pr", use `gh pr create --base main --head <branch>`, and `Closes #<N>`. Surface anything the issue left undecided in the PR body rather than quietly resolving it.
