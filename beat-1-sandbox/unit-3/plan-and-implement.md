# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

jaskhetani

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69#issuecomment-6154103423

I reproduced the raw-array crash in my own checkout and posted the evidence above: `json.loads` returns a list, `parse_review_output` passes it to the dict-only `_parse_json_output`, and `.items()` raises `AttributeError` at `output_parser.py:68`. The JSON-object control still parses successfully.

My plan is to guard the parsed value at both JSON entry points in `parse_review_output`—the fenced branch and the raw branch—so only dictionaries enter `_parse_json_output`. A decoded array will continue to the existing `_parse_plaintext_output(raw)` fallback instead of being given new structured-array semantics. I will keep the change to `rag/generator/output_parser.py` and `tests/unit/test_output_parser.py`.

For coverage, I will remove the strict `xfail` from `test_json_array_fallback`, strengthen it to assert the `general_feedback` fallback and preserved raw content, and add the same regression case for an array inside a JSON fence. I will rerun my direct raw-array reproduction, add the fenced-array check, keep the JSON-object input as a control, run the complete output-parser test file, and run the repository's required Python checks. The observable result is that both array inputs return one fallback `FeedbackSection` without an exception while the object still uses structured JSON parsing.

jacho15 proposed the same two-call-site guard earlier in this thread. This plan follows that bounded direction but is based on my own reproduction and will be built only on my fork, consistent with the course's shared-issue rules. I am not planning to redesign `_parse_json_output`, define list-item mapping semantics, or clean up other seeded defects.

I used OpenAI Codex to help inspect the code and draft and review this plan. The reproduction evidence is from my own checkout, and I will run and report the implementation tests myself.

---

## Your branch

**Branch**

`fix/69-json-array-fallback`

**Evidence**

### Before the change

Direct raw-array reproduction from my Unit 2 report:

```bash
.venv/Scripts/python - <<'PYCODE'
import json
from rag.generator.output_parser import parse_review_output

raw = json.dumps(["First feedback item", "Second feedback item"])
print(parse_review_output(raw))
PYCODE
echo "exit code: $?"
```

```text
Traceback (most recent call last):
  File "<stdin>", line 5, in <module>
  File "...\rag\generator\output_parser.py", line 48, in parse_review_output
    return _parse_json_output(data)
  File "...\rag\generator\output_parser.py", line 68, in _parse_json_output
    for key, value in data.items():
                      ^^^^^^^^^^
AttributeError: 'list' object has no attribute 'items'
exit code: 1
```

The focused test with its xfail marker ignored showed the same failure:

```bash
.venv/Scripts/python -m pytest \
  "tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback" \
  -v --runxfail
echo "exit code: $?"
```

```text
tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback FAILED [100%]
data = ['First feedback item', 'Second feedback item']
>       for key, value in data.items():
                          ^^^^^^^^^^
E       AttributeError: 'list' object has no attribute 'items'
rag\generator\output_parser.py:68: AttributeError
1 failed in 1.91s
exit code: 1
```

### After the change

I reran the raw-array input against commit `deeacee`:

```bash
PYTHONUTF8=1 .venv/Scripts/python - <<'PYCODE'
import json
from rag.generator.output_parser import parse_review_output

raw = json.dumps(["First feedback item", "Second feedback item"])
print(parse_review_output(raw))
PYCODE
echo "exit code: $?"
```

```text
2026-10-13 16:29:36 [info     ] plaintext_output_parsed        content_length=47
[FeedbackSection(section_name='general_feedback', content='["First feedback item", "Second feedback item"]', confidence=0.7, suggestions=[])]
exit code: 0
```

I also exercised the separately guarded fenced-JSON path:

````bash
PYTHONUTF8=1 .venv/Scripts/python - <<'PYCODE'
import json
from rag.generator.output_parser import parse_review_output

raw = "```json\n" + json.dumps(
    ["First feedback item", "Second feedback item"]
) + "\n```"
print(parse_review_output(raw))
PYCODE
echo "exit code: $?"
````

```text
2026-10-13 16:30:05 [warning  ] raw_json_parsing_failed
2026-10-13 16:30:05 [info     ] plaintext_output_parsed        content_length=59
[FeedbackSection(section_name='general_feedback', content='```json\n["First feedback item", "Second feedback item"]\n```', confidence=0.7, suggestions=[])]
exit code: 0
```

The JSON-object control remained on the structured path:

```bash
.venv/Scripts/python -c "import json; from rag.generator.output_parser import parse_review_output; raw=json.dumps({'summary':'Works'}); print(parse_review_output(raw))"
```

```text
2026-10-13 16:36:37 [info     ] json_output_parsed             section_count=1
[FeedbackSection(section_name='summary', content='Works', confidence=0.85, suggestions=[])]
exit code: 0
```

The focused parser suite passed with the strict xfail removed and the fenced
regression added:

```bash
PYTHONUTF8=1 .venv/Scripts/python -m pytest tests/unit/test_output_parser.py -v
```

```text
collected 20 items
tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback PASSED
tests/unit/test_output_parser.py::TestOutputParser::test_json_array_in_code_fence_fallback PASSED
============================= 20 passed in 4.67s ==============================
```

The complete unit suite also passed with the repository's expected xfails:

```bash
PYTHONUTF8=1 make test-unit
```

```text
================ 377 passed, 52 xfailed, 2 warnings in 42.70s =================
```

`make check` completed Ruff and Black, then stopped in an installed dependency
stub rather than either changed file:

```bash
PYTHONUTF8=1 make check
```

```text
.venv/Scripts/ruff check .
All checks passed!
.venv/Scripts/black .
1 file reformatted, 109 files left unchanged.
.venv/Scripts/mypy api/ core/ ingestion/ rag/ agent/ safety/
.venv\Lib\site-packages\numpy\__init__.pyi:737: error: Type statement is only supported in Python 3.12 and greater  [syntax]
Found 1 error in 1 file (errors prevented further checking)
make: *** [typecheck] Error 2
```

Focused checks on both changed files passed, including mypy on the changed
production file:

```text
ruff: All checks passed!
black: 2 files would be left unchanged.
mypy rag/generator/output_parser.py: Success: no issues found in 1 source file
git diff --check: exit code 0
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 19/20 scored items agreed with the gold labels. The run passed the
   18/20 bar and matched at least one package in every category. This is the
   final run saved in `eval-run.txt`.

**Package analysis**

I analyzed `pkg-04`. My rubric decided `reject`, and the gold label was also
`reject`. The candidate proposed a documentation-only workaround even though
the thread had isolated the implementation area in `src/tui/light_windows.go`,
provided a patched test binary, and requested testing. Under
`thread-and-conventions`, a plan must engage explicit maintainer direction, and
under `scope-bounded` and `approach-executable`, its proposed change must address
the reproduced issue at the supported layer. The candidate did neither, so the
rubric correctly held the plan.

**Check rationale**

> | tests-decisive | The plan's test commands, inputs, controls, and expected post-fix observations read against the repro evidence's trigger and artifacts | The test plan reruns the reproduced trigger through the real changed path and names the observable result that distinguishes fixed from still broken. It adds or updates a focused regression check when practical and preserves a relevant control; a generic full-suite run or “should feel right” is insufficient. | required |

I wrote this check to require a direct connection between the reproduced input,
the code path being changed, and a visible post-fix result. I rejected a weaker
version that would pass a plan merely for naming a full test suite, because a
green broad suite can omit the issue trigger and therefore cannot demonstrate
that the reported behavior changed. The focused regression and control clauses
also keep a plan from fixing the issue by silently breaking nearby supported
behavior.

**Trade-offs**

Making `thread-and-conventions` required can hold an otherwise strong technical
plan because its public comment violates a repository-specific rule. That is an
intentional cost: “ready to post and build” includes being ready to post. The
run also exposed the limit of that check in `pkg-20`: my skill accepted the
technically excellent Ghostty plan while the gold label rejected it because the
comment omitted Ghostty's mandatory AI-use disclosure. I kept the 19/20 run
instead of changing the rubric after it passed, because every category already
had a match and a wording change could flip correct thread-convention cases.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
