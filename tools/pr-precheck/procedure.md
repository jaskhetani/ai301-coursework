# Procedure: how this tool grades a PR package

## Read order

1. Determine the mode. In live mode, read `scope.md`, confirm the target repository, record its house rules, and then read `voice-guide.md`. In eval mode, skip both files and treat the bundle as the only source.
2. Read the repository facts and standards before the candidate PR. In eval mode, record the repo-facts block's template, contribution, changelog, testing, and AI-policy requirements. In live mode, read the PR template, `docs/CONTRIBUTING.md`, referenced policy files, and explicit maintainer directions in the issue thread.
3. Read the issue and thread. Record the issue number, requested observable outcome, reproduced trigger, controls, and maintainer constraints without yet accepting the PR's claims.
4. Read the accepted plan before the candidate diff. Record its promised outcome, scope boundary, excluded work, files, mechanism, regression coverage, commands, expected observations, and every recorded deviation. This creates the baseline against which drift is judged.
5. Read the candidate's title, description, and commits. List each material claim about behavior, files, tests, standards, limitations, and fidelity to the plan.
6. Read the entire unified diff, then the test evidence. Do not stop after finding the planned fix. Reading the evidence last prevents a confident description or passing suite from hiding a contradictory hunk or wrong-path test.

## Evidence gathering

1. Build a plan-to-PR ledger. For every changed file and meaningful hunk, record the plan item or deviation that authorizes it. Separately list each planned production, test, documentation, or release-note deliverable and whether the diff contains it. Pair every material title or description claim with the diff hunk, plan statement, test result, or repository fact that verifies it; record contradictions, overclaims, omitted limitations, and claims about absent work.
2. Mark every accepted-plan limit and recorded deviation, then compare it with the description. Treat a limit as authorized when the plan deliberately excludes or defers it and the description states the same boundary. Do not turn a broader issue outcome into an unstated requirement after the accepted plan has bounded the PR.
3. Build a test matrix from the plan's test plan: issue trigger, broken observation, fixed observation, separately promised acceptance targets, changed-path regression test, named controls, and repository checks. Count an acceptance target as separate only when the plan gives it a distinct trigger or expected behavior; multiple analogous call sites receiving the same mechanism are implementation coverage, not automatically separate reproductions. Require concrete broken-versus-fixed evidence for the issue-defining changed path. For a supporting control, regression test, or broader check, accept a specific reported input or scope and outcome even without a full terminal transcript; the test code in the diff may corroborate that report. Leave generic claims such as "tested" or "all tests pass" without a named scope or result unfilled.
4. Classify every nonessential-looking hunk. Record debug statements, commented experiments, dead helpers, stale TODOs, duplicated blocks, broad formatting or import churn, unrelated refactors, and drive-by features. Distinguish minimal mechanical support from avoidable debris by tracing whether the planned implementation requires it.
5. Build a standards checklist from stated repository requirements only. Mark required title form, issue linkage, template sections, checkboxes, tests, changelog or release note, maintainer direction, and AI disclosure as satisfied, violated, or unverifiable, citing the candidate artifact for each mark.
6. In live mode, separately compare the title and description with `voice-guide.md` and retain any violations for the readable summary; do not add them to a rubric grade unless a rubric pass condition also covers the same fact.

## Check execution

1. Grade `plan-fidelity` from the plan-to-PR ledger. Inspect both drift directions and every description claim. Treat an accepted-plan limit or recorded, accurately described deviation as authorization, not as automatic failure; fail only undisclosed extra work, undisclosed omitted work, or a material contradiction.
2. Grade `test-evidence-decisive` from the test matrix. Trace the issue-defining input through the changed path to the broken-versus-fixed observation. Then verify each genuinely separate acceptance target and the specific outcomes of named controls and checks. Do not demand one reproduction per analogous call site or reject a specific supporting result merely because it is terse. A broad suite cannot replace a missing issue-path result, and an unchanged control cannot stand in for the failing path.
3. Grade `diff-reviewable` from the complete diff classification. Judge the review cost imposed by actual debris and unrelated hunks, not prose quality or commit count by itself.
4. Grade `repository-standards` from the standards checklist. Enforce only requirements actually stated in repo facts or live repository sources, but enforce every mandatory one, including AI disclosure.
5. Assign each check exactly one of `pass`, `fail`, or `unclear` and one decisive evidence line. Use `fail` when present evidence violates the pass condition. Use `unclear` only when necessary evidence is absent or unverifiable. Do not infer missing commands, files, policy compliance, or intent.

## Verdict assembly

1. Confirm that every rubric row has one grade and a concrete evidence line.
2. Apply the verdict rule mechanically: any required `fail` or `unclear` yields `reject`; only all required passes yield `accept`. Preferred checks never gate submission.
3. In the readable summary, lead with the first failing required check in rubric order, then identify any additional failures. For an accepted package, name the strongest plan-to-diff and test-evidence facts. In live mode, add voice-guide findings separately.
4. Emit the fixed JSON with the PR URL or bundle id, checks in rubric order, and binary verdict. Confirm that it parses and print nothing after the closing fence.
