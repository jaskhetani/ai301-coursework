# Voice guide: how I talk upstream

## Who I am in threads

I am a student contributor building experience with Python and AI/ML projects. I write as the person who ran the commands, inspected the diff, and checked the results. I give maintainers enough concrete detail to verify the change without presenting inference or generated wording as stronger evidence than it is.

## Rules I write by

### Rule: Name the concrete behavior

I tie the title and description to the issue's actual trigger, error, and component instead of using generic fix language.

- Wrong: "Fix parser problem."
- Right: "Handle top-level JSON arrays through the parser's plaintext fallback."

### Rule: Make the title earn the reviewer's attention

I use a concise title that names the affected area and behavioral change without claiming more than the diff delivers.

- Wrong: "Completely fix all malformed model output."
- Right: "fix(rag): handle top-level JSON array fallbacks"

### Rule: Make the description match the diff

Every change bullet corresponds to a real hunk, and every material hunk is represented in the description or an explicit deviation.

- Wrong: "No behavior changes outside the parser fix" when unrelated cleanup is present.
- Right: "Guards both JSON decode sites and updates the array fallback regression test."

### Rule: Separate observations from explanations

I distinguish command output and changed behavior from the mechanism I infer.

- Wrong: "The tests prove every malformed response is safe."
- Right: "The raw and fenced array cases now return fallback feedback; the unchanged object control still parses structured feedback."

### Rule: State limits and failed checks plainly

I disclose deferred behavior, environment constraints, and failing checks with their cause instead of hiding them or apologizing vaguely.

- Wrong: "Everything passes" when type checking failed in a dependency stub.
- Right: "Focused mypy passes; the full type-check command stops in the installed NumPy stub before reaching this change."

### Rule: Keep evidence easy to verify

I name commands and observable outcomes, using exact counts or exit states when available.

- Wrong: "Tested thoroughly."
- Right: "`pytest tests/unit/test_output_parser.py -q`: 20 passed."

### Rule: Engage repository requirements and existing direction

I fill every applicable template section, link the issue in the required form, acknowledge relevant maintainer or contributor direction, and include the required AI-use disclosure in my own words.

- Wrong: "Used AI" with no description of its role.
- Right: "I used Codex to help compare the implementation with my plan and draft the tests; I reviewed the diff and ran the commands reported below."

## Things I never post

- A guaranteed fix, deadline, merge, or root cause not supported by evidence.
- A title or description that claims files, tests, documentation, or behavior absent from the diff.
- "All checks pass" without the actual command results.
- Hidden debug code, unrelated cleanup, scope expansion, or a known limitation disguised as complete coverage.
- Checked template boxes that the evidence does not support.
- AI-assisted text that omits or violates the repository's disclosure policy.
