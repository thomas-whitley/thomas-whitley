---
name: interrogate
description: Adversarial review. Several subagents try to break a change or design, and only findings proven by a real run are reported. Use when asked to interrogate, stress test, challenge or "tear apart" a change.
disable-model-invocation: true
argument-hint: "[scope: files, PR, or blank for the branch diff] [number of adversaries, default 3]"
---

# Interrogate

Adversaries try to break a change or a design. A finding counts only when a run proves it. Report the findings; do not fix anything.

## 1. Frame

- **Scope:** what I name. Otherwise, the branch diff from its merge base with the default branch, plus uncommitted changes.
- **Intent:** restate in two or three sentences what the change is meant to do, from my message, the commits and any PR description. If the intent is unclear, ask me before going further.

## 2. Fan out

Spawn the adversaries in parallel with the Agent tool: 3 unless I gave a number, each `general-purpose` with `isolation: "worktree"`, so their repro scripts never touch my working tree. If the scope includes uncommitted changes, tell each one to apply them first (give them the `git diff` output or a patch file path).

Give each one a different angle so they do not all find the same thing, for example: edge-case inputs and boundaries; concurrency, ordering and failure paths (timeouts, partial writes, retries); and callers and contracts (who else uses this, what assumptions broke). Then give each the scope, the intent and this brief:

> Break this change. Prove each finding with a run against the real code: a failing test, a repro script, or command output that shows the wrong behaviour. For a design or plan, run a prototype or probe the current code. Keep repros in your own worktree. Report each finding with a title, the exact command, the output it printed, and the output the intent requires. Leave out anything you did not run, including style, naming, structure, and risks that only matter after a future change. No findings is a valid result.

## 3. Verify

Rerun each finding's command yourself. Keep it only when the output shows the failure and that failure breaks the intent. Drop the rest. Merge findings that share a root cause. Keep every verified finding from every adversary, not just one list.

## 4. Report

- **Intent:** the restatement.
- **Findings:** for each verified finding, the command, its output, how it breaks the intent, and which adversaries found it. Say "no verified findings" if none survived.
- **Dropped:** for each adversary, how many findings were dropped and why.
