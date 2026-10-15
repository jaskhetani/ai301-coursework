---
name: pr-precheck
description: Grade one candidate pull request against its accepted plan, issue, diff, test evidence, and repository requirements, then decide whether it is ready to submit. Use for a live draft PR before opening it or for one frozen Unit 4 eval package.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: **is this ready to submit?** A PR package consists of a candidate title and description, commit list, branch diff, and test evidence, read against the issue and accepted plan it claims to implement. Do not redesign the change, score multiple packages, or substitute general code-review advice for the binary readiness decision.

## Inputs and modes

Select exactly one mode from the supplied input.

- **Live mode:** read the student's issue URL, `plan.md` including `## Deviations`, draft title and description from `pr_draft.md`, test output from `test_evidence.md`, and the committed branch diff produced by `git diff main...HEAD`. Read the real issue thread, `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and any policy files those sources identify. Grade the complete committed diff, not only files named in the prompt or working-tree changes. A house-chain student uses the supplied house plan and repro pack in place of their own plan and issue reproduction; do not invent missing house evidence.
- **Eval mode:** the supplied bundle is the whole world. Read its repo-facts, issue, thread highlights, plan context, candidate title and description, commits, diff, and test evidence. Fetch nothing, read no local repository files, and run every rubric check with the full verdict rule.

Do not mix modes. Evidence absent from an eval bundle remains absent even if the live project is familiar.

## The scope seam (live mode only)

In live mode, read `scope.md` before gathering any PR evidence. Confirm that the PR targets the exact repository on its `Repo:` line and apply the listed house rules. Refuse to grade an out-of-scope repository. If the line is missing or still contains a placeholder such as `<ORG>/<PATH-REVIEW-REPO>`, stop without grading and tell the student to fill it; never infer or substitute a repository. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and check the outgoing PR title and description against it. Before the final JSON, report each broken voice rule with the relevant draft wording. Voice findings do not alter the verdict unless a row in `rubric.md` independently makes the same behavior a readiness requirement. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read `rubric.md`; it defines the checks, pass conditions, weights, and verdict rule. Read `references/evidence-guide.md`; it identifies the evidence for each failure family in both modes. Execute `procedure.md` exactly as written to gather, compare, grade, and assemble the result.

Refuse to grade if `rubric.md` has no completed check rows or `procedure.md` has no substantive steps. If the procedure is silent about a necessary operation, identify that gap in the readable summary instead of inventing a replacement procedure. Never add an ad hoc check during a run.

## Verdict and output

The verdict is binary: `accept` means ready to submit and `reject` means hold. A readable per-check summary may precede the result. End every completed grading run with the following valid fenced JSON block, in this order, and print nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Name the concrete fact, quote, changed hunk, command result, or repository requirement that decides every check; "looks good" is not evidence.
- Grade the artifacts and observable outcomes, not length, confidence, prose polish, or number of commits.
- Apply each written pass condition literally. If it produces an uncomfortable result, report it; do not silently rewrite the rubric during the run.
- Follow `procedure.md` as written and report a procedural gap rather than improvising around it.
- Preserve the distinction between an honestly bounded shortfall and silent drift. A limitation may pass when the accepted plan or recorded deviation authorizes it and the description states it accurately; hidden extra work, hidden omitted work, or an unsupported claim does not.
- When evidence needed for a required check is absent or cannot be verified, grade it `unclear`. Under the rubric verdict rule, `unclear` fails readiness.