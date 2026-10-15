# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives.** In an eval bundle, compare the Plan context's promised outcome, scope, exclusions, named files, test changes, and deviations with the Candidate PR's title, description, commits, and every diff hunk. In live mode, compare `plan.md` including `## Deviations` with `git diff main...HEAD`, the branch commits, and `pr_draft.md`.

**What good looks like.** Every changed behavior and file is authorized by the plan or a specific recorded deviation, and every promised material deliverable is present or explicitly deferred. The description matches both the implemented work and the stated boundary. Extra configuration, refactors, cleanup, or features are silent drift even when adjacent to the fix; claiming an absent planned change is drift in the opposite direction. A narrower result is acceptable when the plan or deviation and the description both disclose the same bounded shortfall.

## Test evidence (harness category: not-tested)

**Where it lives.** In an eval bundle, map the Plan context's reproduction, commands, controls, and expected results to the Candidate PR's Test evidence and test-related diff hunks. Also read repo-facts for required checks. In live mode, use the Unit 2 reproduction, the plan's test section, `test_evidence.md`, changed tests in the branch diff, and the repository's required check commands.

**What good looks like.** Evidence names the issue-triggering command or input, shows the broken observation and the branch's corrected observation, and exercises the code path changed by the diff. It covers each material failure mode the plan promises, includes named controls where relevant, and reports concrete repository-check outcomes. "Tested locally," "works now," a generic suite name, or a passing unchanged control does not establish the fix. An unrelated infrastructure failure may remain when its full result and cause are disclosed; a failure or omission on the changed path does not pass.

## Diff quality (harness category: unreviewable)

**Where it lives.** In an eval bundle, inspect the entire unified diff and use the commit list as supporting context. In live mode, inspect `git diff main...HEAD` and the branch's commits; uncommitted scratch files are not part of the proposed PR but should not be used to fill a committed-diff gap.

**What good looks like.** The planned behavior change and regression coverage are easy to isolate, with only necessary mechanical support. Debug prints, commented-out attempts, dead helpers, stale TODOs, duplicated code, unrelated feature or refactor hunks, and broad formatting or import churn are review debris. A terse or messy commit title is not by itself a blocker when the resulting diff is clean and bounded.

## Standards and comms (harness category: standards-wall)

**Where it lives.** In an eval bundle, read the Repo facts for PR-template asks, contribution requirements, changelog rules, and AI-use policy, plus explicit Thread highlights. Compare them with the Candidate PR title, description, checklist statements, diff, and test evidence. In live mode, read `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, referenced policy files, and issue-thread directions; compare them with `pr_draft.md`, the diff, and `test_evidence.md`. Read `voice-guide.md` separately for live-only voice feedback.

**What good looks like.** Every mandatory section contains real, truthful content; the issue is linked in the required form; checked boxes agree with evidence; required tests and changelog or release-note files are present; maintainer directions are addressed; and a required AI disclosure names the tool and extent. Boilerplate, unchecked mandatory work, a claimed but absent standards file, or silence under an explicit disclosure policy fails. Do not impose a disclosure or template requirement that the repository does not state. Description claims matching the diff belong to plan fidelity, not this family.