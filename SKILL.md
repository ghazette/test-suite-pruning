---
name: test-suite-pruning
description: Audit and shrink a bloated automated test suite by deciding what kind of test is worth keeping, then deleting the rest. Rates tests by what would break unnoticed, sorts them into genres (source greps, display checks, cosmetic formatting, trivial empty cases, registry shape), keeps what protects money, auth, moderation, data and real behavior, and flags tests that validate a bug. Works with pytest, Vitest, Jest and Playwright. Use when the user says there are too many tests, wants to clean, prune, reduce or audit tests, speed up a slow suite, remove useless or brittle tests, or asks whether a test is worth keeping, even without saying "pruning".
---

# Test suite pruning

A test earns its place by catching a bug nobody would otherwise see. This skill finds the tests that cannot, and deletes them by genre.

## Why decide by genre

Judging test by test fails. Asked "can this one go?", a careful reader finds a reason to keep almost anything: on a real 7,237-test suite, a case-by-case second reading kept four candidates out of five. The questions that work are "is this kind of test useful here?" and "does this one fall under an exception?". Decide the genres first, then classify.

## Workflow

1. **Baseline.** Run every suite before touching anything and save the failures. Without this list, you cannot tell your damage from what was already red. A suite that cannot load (a missing dependency, a file outside the mount) hides tests: fix the environment for the run or say what went unmeasured.
2. **Work on a branch**, with commits you can revert by lot. Stage explicit paths: the tree may hold someone else's work.
3. **Read a sample.** Pull 50 to 100 low-value tests and read their bodies. Adjust the genres in `references/genres.md` to this project: an internal admin tool and a checkout page do not carry the same risk.
4. **Agree the genres** with the owner when they are available. A genre deleted here is a product decision.
5. **Classify every candidate.** Above a few hundred tests, split into batches of about 100 for subagents, using `references/reviewer-brief.md`. Reject any batch that judged from test names: reruns changed about a third of the verdicts.
6. **Audit the result.** Sample each genre, read the reasons, and read every flag a reviewer raised. Rescue by hand what the rule got wrong.
7. **Apply a safety net.** Keep any candidate whose path or name touches money, auth, permissions, moderation, minors, data deletion or migrations, unless you read it yourself.
8. **Delete with a script** that parses the code (AST in Python, bracket matching in TypeScript). Also handle emptied classes and `describe` blocks, `describe.each(...)(...)` double calls, files left with no test, and unused imports.
9. **Compare with the baseline.** Failures must match exactly. A new failure is a finding: read it before anything else.
10. **Report** the before and after counts, what each genre removed, what you rescued, what could not run, and the commands to undo.

## The rules that keep this safe

- **Never prune a test because it is red.** A failing test goes through the same decision as the others, and a red one that was already red gets reported, not removed.
- **Look for guards that name tests.** Registries, coverage checks and catalogs can reference tests by name or by marker. Deleting those guards first hides the damage you do next: keep them until the end, then read what they report. Their shape looks useless, and they may enforce a documented rule.
- **Check claims in comments.** A docstring saying "no guard exists here" can be wrong. Read the code before fixing or reporting.
- **Search under several names** before stating a route or feature does not exist.
- **Deleting a test is not fixing its bug.** Report any product defect you meet, separately.

## A test that validates a bug

Never leave a test that asserts a defect as the expected result. It makes the defect look intended. When a test, comment or name says "quirk", "pins the current behaviour" or "known issue":

1. Read the code and verify the claim.
2. If it is a defect, fix it and assert the right result, or report it.
3. If it is a trade-off someone chose and wrote down, keep the test with its reason.

## Rating a test

Rate what the test checks, never its file name. Ask what would break, unnoticed, if it disappeared.

| Score | Meaning |
| --- | --- |
| 90-100 | Money, auth and permissions, moderation, data loss, a whole flow down |
| 60-89 | Real business logic, or a real fixed bug that could return |
| 30-59 | Useful, limited damage, unlikely edge case |
| 1-29 | Copy of a value, framework, redundant, render only |
| 0 | No real assertion, dead, or validates a bug |

A score nominates candidates. The genres decide. Report critical tests that skip by default (no database, no environment variable): they protect nothing until someone runs them.

## Writing new tests

Before adding a test, name the bug it would catch. No bug to name, no test. A feature needs its refusals, its branches and one smoke path, far short of one test per visible element.
