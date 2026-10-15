# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/216

**Branch**

`fix/69-json-array-fallback`

## Eval iterations

**Run history**

1. Full run: `17/20` scored items agreed. `pkg-05`, `pkg-11`, and `pkg-19` were false rejects on `test-evidence-decisive`.
2. Targeted run: `4/4` scored items agreed. The run covered `pkg-05`, `pkg-11`, `pkg-19`, and the `pkg-07` canary; `calib-04` also agreed but was not scored.
3. Full run: `17/20` scored items agreed. The earlier false rejects were corrected, but `pkg-02`, `pkg-13`, and `pkg-16` became false rejects.
4. Final full run: `20/20` scored items agreed. Category results were `clear-accept 7/7`, `not-tested 4/4`, `silent-drift 4/4`, `standards-wall 2/2`, and `unreviewable 3/3`. This is the run saved in `eval-run.txt`.

**Package analysis**

For `pkg-05`, my final rubric returned `accept`, matching the gold label `accept`. The package's description names issue #18848, explains the merge-by-name regression and the planned `(name, mode, modifier, keycode)` identity, and accurately matches the two implementation commits and the bounded two-file diff. Its evidence directly compares the issue reproduction before and after: one `atuin` row before and both the `ctrl-r` and `up` rows after. It also reports the separately promised same-key warning result, `cargo test -p nu-protocol` with 312 passing tests, and clean format and clippy checks. The required template content and issue linkage are present, with no repository AI policy to satisfy. Therefore all four required checks pass.

**Check rationale**

Current `test-evidence-decisive` pass condition, quoted exactly from `tools/pr-precheck/rubric.md`:

> The evidence establishes the issue-defining changed path with a concrete broken-versus-fixed observation. Each separately promised acceptance target with a distinct trigger or expected behavior has an explicit result. Repeated applications of the same mechanism across analogous call sites do not require separate reproductions unless the plan promises separate cases; representative regression coverage plus the specific broader-suite result may establish them. Named controls and applicable repository checks have specific reported outcomes, but supporting results need not include full transcripts. Generic assurances, a suite name without a result, evidence only from an unchanged control, or omission of a separately promised path fails. A reported check failure may pass only when its result is shown, the description discloses it, and the evidence establishes that it is unrelated to the changed behavior; failure on the changed path fails.

The first version was too strict about requiring separate runtime proof for every analogous call site, which falsely rejected the otherwise complete `pkg-05`, `pkg-11`, and `pkg-19`. I revised the check to distinguish a genuinely separate promised behavior from repeated uses of the same mechanism. Representative regression coverage and a concrete broader-suite result can now establish analogous sites, but the rule still rejects a missing distinct trigger, generic “tests pass” language, or a failure on the changed path. I also made the disclosed-failure rule explicit so an unrelated environment or dependency failure is evaluated honestly instead of being automatically accepted or rejected.

**Trade-offs**

Allowing representative coverage for analogous call sites gives up some per-site certainty and could miss a site-specific wiring problem. Requiring every analogous path separately, however, produced three false rejects even where the same mechanism and broader suite were decisively covered. I re-ran the changed cases `pkg-05`, `pkg-11`, and `pkg-19` with `pkg-07` as a negative canary and `calib-04` as calibration; all agreed. The next full run exposed false rejects in `pkg-02`, `pkg-13`, and `pkg-16`, so I tightened the evidence/description interaction and ran the complete set again. The final `20/20` run, including every negative category, is the reason I know the relaxation did not turn the rubric into an accept-by-default check.

---

Related paths: `eval-run.txt` in this directory; the skill files in `tools/pr-precheck/`.
