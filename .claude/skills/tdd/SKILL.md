---
name: tdd
description: Use when asked for TDD, a failing test or a regression test, or from the fix-bug skill when the bug has an obvious, cheap local test path. Skip when the test path is unclear, expensive or integration-heavy.
---

# Failing test first

When a bug has a clear, cheap test path, make the broken behaviour executable before changing production code. The goal is one focused regression test that fails before the fix and passes after it.

Do not force a test when it would be impractical. If it would need broad harness setup, brittle mocks, slow end-to-end infrastructure, production-only state, vague repro steps or a lot of unrelated fixture changes, skip the new test and use the closest useful check instead.

## Steps

1. **Understand the bug.** The intended behaviour, the current behaviour, the code path, and the smallest observable repro.
2. **Pick the narrowest check.** Prefer the kind of test the project already uses for that code path. If there is no practical path, do not build one from scratch just to follow this skill.
3. **Write the failing test.** The smallest test that would have caught the bug. It encodes the intended behaviour, not the current implementation.
4. **Run it before fixing.** Confirm it fails, and for the intended reason. If it passes, or fails for something unrelated, fix the test first.
5. **Fix the bug** with the smallest production change that gives the intended behaviour and keeps nearby contracts.
6. **Rerun the test** and confirm it passes. Run the neighbouring tests too.

## When a failing test is impractical

Use the closest repeatable check: a small script, a repro command, browser automation, a snapshot comparison, a log assertion or a focused integration check.

No new test is better than a bad test. A bad test mostly tests mocks, copies implementation details, depends on timing or global state, needs expensive infrastructure for a small fix, or would be deleted right after proving the fix.

## Guardrails

- Never change a test just to match a wrong implementation.
- Do not weaken existing assertions unless the expected behaviour really changed, and say why.
- Keep the test about this bug. No unrelated coverage or fixture churn.
- For a flaky bug, make the test deterministic and note what signal it locks down.
- If the bug shows a wider class of failures, land the focused test first, then suggest the wider coverage.

## Report

- The failing-before test or check, and the failure it printed.
- The passing-after run, and any nearby tests you ran.
- If you could not show it failing first, say why and what check you used instead.
