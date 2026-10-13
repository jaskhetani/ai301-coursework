# Plan: handle top-level JSON arrays without crashing

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69

Reproduction: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69#issuecomment-5903402421

## Diagnosis

My Unit 2 reproduction passed a raw top-level JSON array to
`parse_review_output` and captured this path:

> `parse_review_output` passed the parsed list to `_parse_json_output` at
> line 48. `_parse_json_output` then called `data.items()` at line 68 and
> raised `AttributeError: 'list' object has no attribute 'items'`.

The JSON-object control completed successfully and returned one
`FeedbackSection`, so JSON decoding itself is not the failure. The array is
valid JSON, which means `json.loads` succeeds and the existing
`JSONDecodeError` handler cannot send it to the plaintext fallback. The parsed
list instead reaches `_parse_json_output`, whose `dict` annotation and
unconditional `.items()` loop show that it accepts dictionaries only.

Code inspection shows the same unconditional call after both `json.loads`
sites: fenced JSON at line 41 and raw JSON at line 48. Other contributors on
the issue have also reproduced the fenced path. My own captured traceback
directly establishes the raw path; the fenced path will therefore receive a
separate regression test rather than being treated as already verified by my
Unit 2 run.

## Scope

In scope:

- In `parse_review_output`, allow only parsed dictionaries to enter
  `_parse_json_output` at both the fenced-JSON and raw-JSON call sites.
- Let a successfully decoded non-dictionary value continue to the existing
  `_parse_plaintext_output(raw)` fallback instead of raising.
- Remove the strict `xfail` marker from the issue's existing raw-array test and
  make its expected fallback result explicit.
- Add focused coverage for the same array shape inside a JSON code fence.

Out of scope:

- Defining list elements as new structured feedback sections or changing the
  `FeedbackSection` data model.
- Redesigning `_parse_json_output`, changing malformed-JSON behavior, or adding
  general schema validation.
- Cleaning up the deliberately retained `sections` accumulator or any other
  seeded defect.

The type guard also makes JSON scalars use the existing fallback, because they
are non-dictionaries for the same reason as arrays. I will not add broader
scalar semantics or unrelated cases to this issue.

## Files

- `rag/generator/output_parser.py`
- `tests/unit/test_output_parser.py`

## Approach

1. After decoding fenced JSON, check `isinstance(data, dict)`. Return
   `_parse_json_output(data)` only for a dictionary; otherwise continue through
   the function to its existing plaintext fallback.
2. Apply the same check after decoding raw JSON. This keeps the two current JSON
   entry paths consistent without broad exception handling.
3. Do not catch `AttributeError`: that would hide unrelated defects inside
   `_parse_json_output` and would not express the actual input contract.
4. Remove `@pytest.mark.xfail(strict=True, ...)` from
   `test_json_array_fallback`. Strengthen the test to assert one
   `general_feedback` section containing the original raw array text.
5. Add a fenced-array test that asserts the same no-crash fallback behavior for
   the other call site. Keep the existing JSON-object tests unchanged as the
   control for structured parsing.

This follows jacho15's earlier two-call-site guard direction on the issue. I am
building it from my own reproduction in my fork, as the course repository's
shared-issue rules allow, rather than claiming their evidence as mine.

## Test plan

Before the change, my Unit 2 evidence records:

- The focused test normally reports `XFAIL` and exits 0.
- The same test with `--runxfail` fails at
  `rag/generator/output_parser.py:68` with
  `AttributeError: 'list' object has no attribute 'items'` and exits 1.
- A raw JSON-object control returns a `FeedbackSection` and exits 0.

After the change:

1. Re-run the same direct raw-array command from the reproduction. Expect exit
   code 0 and one `general_feedback` section whose content preserves the input
   array text.
2. Run the equivalent fenced-array input. Expect exit code 0 and one
   `general_feedback` section rather than the line-41-to-line-68 crash.
3. Re-run the raw JSON-object control unchanged. Expect structured JSON parsing
   to continue returning the `summary` section with exit code 0.
4. Run
   `.venv/Scripts/python -m pytest tests/unit/test_output_parser.py -v`.
   Expect both array tests to pass normally, no `XPASS(strict)`, and all other
   parser tests to remain green.
5. Run the repository checks required by `docs/CONTRIBUTING.md`: `make check`
   and `make test-unit` from Git Bash. If a failure is unrelated to this two-file
   change, record the full output rather than hiding or widening the fix.

## Risks and unknowns

- The issue requires a fallback but does not define array-to-section semantics.
  Reusing `_parse_plaintext_output` preserves the original response and avoids
  inventing an API contract; the focused assertions make that choice visible.
- For a fenced array, plaintext fallback preserves the full original response,
  including its fence. That matches the current fallback's contract of storing
  `raw`; the new fenced regression will make the behavior explicit.
- The same two-site guard has already been proposed on the shared issue. If the
  upstream branch changes before the Unit 4 PR, I will rebase and reassess the
  plan rather than expand this change to compete with unrelated work.

## Deviations

The implementation followed the posted plan. I added dictionary guards at both JSON entry points, removed the strict xfail, strengthened the raw-array test, and added the fenced-array regression test.

The planned project-wide `make check` did not complete because mypy stopped in the installed NumPy stub with `Type statement is only supported in Python 3.12 and greater`; the repository config targets Python 3.11. This is outside the two changed files, so I did not change dependencies or widen the fix. Ruff and Black passed, focused mypy on `rag/generator/output_parser.py` passed, all 20 output-parser tests passed, and the full unit suite completed with 377 passed and 52 expected failures.
