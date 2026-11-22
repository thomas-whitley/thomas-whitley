# Working rules

These apply to every task. The skills in `skills/` build on them.

## Prove it works

Before saying a task is done, check the real thing: run the feature, read the actual value, inspect the diff. Proxies do not count: "it compiles", a passing unrelated test, cached output, file timestamps, or a subagent saying it worked. When a check fails, suspect the way you observed it before you blame the system.

Script the check when you can, so it can be rerun. Show its output to me.

## Fix root causes

Fix the fault, not the place where it surfaced. The root cause is the first place the system did something it should not, or failed to handle a state it should handle.

- Reproduce first. Keep asking "why" until the answer is a wrong decision in the code. When stuck, add logging and read the actual state; do not guess.
- Ask whether the state that triggered the symptom is legitimate. If it is, handle it where the code broke. If it should never exist, a check there hides the bug, so find what produced it. Guards, retries, swallowed errors, clamped values and discarded state are the usual ways bugs get hidden.
- Fix the pattern, not just the instance. Look for other places with the same fault.
- If finding the cause took a long dig, suggest the cheap, always-on signal (a log line, an assertion, a metric) that would have shown it straight away.

## Smallest change

- Make the smallest change that solves the problem. Fewer lines beat tidy-looking boilerplate.
- Do not ship anything that only "might help". A change with no evidence behind it is a guess. When evidence disproves a guess, revert what it added.
- Do not repeat the same decision in several places. Put it behind one source of truth.
