# Test Suite Pruning: a Claude Code skill to delete useless tests

Too many tests? **Test Suite Pruning** is an [Agent Skill](https://docs.claude.com/en/docs/claude-code/skills) for Claude Code that audits a bloated automated test suite, finds the tests that cannot catch a bug, and removes them safely. It works with pytest, Vitest, Jest and Playwright.

Built while cleaning the test suite of [yamete.gg](https://yamete.gg), an AI image generation platform: **1,117 of 7,237 tests removed (15%), with no new failure in any suite that ran.**

## What it does

- **Reduces test suite size** by deciding which genres of test are worth keeping, then deleting the others.
- **Protects what matters**: payments, auth, moderation, data loss, real user actions and contracts between services stay.
- **Finds tests that validate a bug**, the ones that make a defect look intended.
- **Spots brittle tests**: source greps, CSS class checks, UI text assertions, copied constants, snapshot-style pins.
- **Scales with subagents**: a reviewer brief classifies thousands of tests in batches of about 100.
- **Compares every suite with a baseline**, so the damage from a wrong call shows up immediately.

## Why decide by genre instead of test by test?

We tried test by test first. Asked "can this one go?", a careful reader finds a reason to keep almost anything: a second reading kept four candidates out of five and removed 288 tests. Deciding by genre first, with explicit keep rules, removed about 830 more on the same suite.

Then the safety rules earned their place. Some deleted tests were named by a metric catalog, and the guard that checks those names caught it, so they came back.

## Install

Copy the skill into your personal or project skills folder:

```bash
git clone https://github.com/ghazette/test-suite-pruning.git ~/.claude/skills/test-suite-pruning
```

For one project, clone it into `.claude/skills/test-suite-pruning` instead.

## Usage

Ask Claude Code in plain language:

- "There are too many tests, clean them up"
- "Which of these tests are useless?"
- "Is this test worth keeping?"

The skill works on a branch, takes a baseline, agrees the genres with you, then reports what each genre removed and how to undo it.

## What gets deleted

| Keep | Delete |
| --- | --- |
| Money, auth, moderation, data loss | Source file greps |
| Actions: request sent, event emitted, row written | Display-only checks: text, class, attribute |
| Decisions that change what the user gets | Cosmetic formatting, trivial lookups |
| Real fixed bugs, on behavior | Trivial empty cases, copied constants |
| Contracts no type checks | Framework tests, redundant twins, dead skips |

The full list is in [`references/genres.md`](references/genres.md). Adjust it to your project: an internal admin tool and a checkout page do not carry the same risk.

## Limits

Deleting tests is a judgment call, and a score only nominates. Sample each genre before deleting, keep commits revertible, and run the end-to-end tests your environment can run. In the cleanup above, the end-to-end suite was only checked for syntax.

## License

MIT. See [LICENSE](LICENSE).

---

Made by the team behind [yamete.gg](https://yamete.gg).
