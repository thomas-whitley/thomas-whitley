---
name: fix-bug
description: Use when asked to fix a bug, regression, crash, wrong output, flaky behaviour or anything that "used to work". Reproduce it, find the root cause with evidence, make the smallest fix, and prove the original repro now passes.
---

# Fix a bug

Every line you ship must trace back to runtime evidence. A change that "might help" is a hypothesis, not a fix, and does not ship. When evidence disproves a hypothesis, revert what it added.

## 1. Reproduce it yourself

Reproduce the bug on the surface where it was reported: the same app screen, endpoint, query, command or test. Drive it yourself (run the app, call the API, run the script) rather than asking me to.

If it will not reproduce directly, build the trigger: tighten the conditions, synthesise the input, or add logging until it fires. Ask me only when something specific stops you reaching it (credentials, hardware, production-only data), and say what that is, after getting as far as you can.

Write down the exact repro command or steps and the wrong output. You will rerun it in step 4.

## 2. Find the cause by elimination

1. List the candidate causes. Seed the list by reading the code path, and by checking recent history for regressions (`git log -p` / `git log -S` on the affected code, `git bisect` when there is a known good commit).
2. Each pass, run the check that rules out the most candidates at once. Get runtime evidence (logs, a debugger, a print of the actual state), not a reading of the code.
3. Repeat until one cause is left. Confirm the mechanism with evidence before fixing.

Apply the root-cause rules in `CLAUDE.md`: is the bad state legitimate, or is a guard hiding where it came from? Look for the same fault elsewhere.

## 3. Fix

Make the smallest change the evidence justifies. If the fix crosses function or module boundaries, outline the plan to me in a few lines first.

If the bug has a cheap local test path, follow the `tdd` skill: write the failing test, watch it fail for the right reason, then fix. Skip that when the test would need heavy mocks, slow end-to-end setup or production-only state.

## 4. Verify on the same surface

Rerun the exact repro from step 1. It must now give the right output. A pass on a different surface, or an "inconclusive" result, is not a pass: say so. Unit tests show that a branch behaves; they do not prove the bug is gone. Run the nearby existing tests too.

## 5. Commit in order

When committing, put the failing test (or repro script) in its own commit before the fix, so the history shows it failing and then passing.

## Reply

- What was broken
- The root cause, and the evidence for it
- The fix
- How you verified it, with the failing-then-passing repro output pasted verbatim
