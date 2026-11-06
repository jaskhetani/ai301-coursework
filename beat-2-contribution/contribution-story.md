# Contribution: Link Qt field labels to their input widgets

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Jas Khetani
**GitHub Username:** jaskhetani
**Issue:** https://github.com/OpenChemistry/avogadrolibs/issues/3184
**Pull Request:** https://github.com/OpenChemistry/avogadrolibs/pull/3310

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built a low-risk scout with a traceable, bug-first search strategy**

I built GitHub searches for small Python, Rust, and Go bugs, then ranked unassigned and recently updated results ahead of older or discussion-heavy work. My first scan filled all five slots with easy bugs from one repository, but its contribution policy banned AI-assisted work, so the scout correctly rejected every candidate. I added a one-result-per-repository cap and excluded linked PRs during free search, then direct-graded the remaining low-risk options until five passed every required check. The scout also caught that an issue I reserved on the course site already had an upstream claim and open PR, so I rejected it instead of duplicating someone else's work. After claiming Lind #375, I added issue-age and measured maintainer-responsiveness checks; the older Lind issue failed the stricter regrade, so I identified two fresh, actively maintained course-site backups without making competing public claims.

**Unit 6: Drafted a bounded public claim and cached repository-specific instructions**

I drafted a claim that named my first step without promising an outcome, but a later tooling audit found that the official voice-guard hook had not actually been installed. I corrected that gap before opening the PR by installing the official guard and checking the public PR draft. For the initial Lind candidate, I created a local instruction file with its DCO, title, documentation-check, and disclosure rules; after switching issues, I followed Avogadro's checked-in contribution instructions instead.

**Unit 7: Gated the surface-dialog patch before opening its pull request**

I installed the official ship-gate and voice-guard tools, then wrote a local manifest with focused XML and Qt `uic` checks for the two changed files. Windows launcher differences initially broke the hooks, and the first plan-check lacked permission to inspect GitHub, so I repaired the local launch commands and reran it with read-only repository access. The plan-check accepted all six rubric checks, ship-gate passed its repository and PR checks, and Claude Code opened PR #3310 with the validated draft.

**Unit 8: Installed a repository-calibrated PR monitor and chose to wait**

I wrote a one-page playbook from my voice rules and five recent outside-contributor PRs, whose first maintainer activity ranged from immediate to 30.6 hours, and set a conservative four-day nudge threshold. The monitor read PR #3310's checks, review state, thread, and quiet time, then drafted a private wait note because the PR had been open for less than a day. Its first full run exposed another Windows `claude` launcher mismatch, so I corrected the local launcher and reran successfully; after reviewing the draft, I posted nothing because the threshold was not met.

---

## Why I Chose This Issue

I selected OpenChemistry/avogadrolibs #3184 after tightening my scout to account for issue age and maintainer responsiveness. I withdrew my older Lind claim and publicly claimed only the unclaimed `qtplugins/surfaces/` slice: `surfacedialog.ui` and `orbitalwidget.ui`. The repository is active, the issue is maintainer-authored and recently updated, and the requested change is a small, testable Qt accessibility improvement. My claim is https://github.com/OpenChemistry/avogadrolibs/issues/3184#issuecomment-6671133419.

This slice fits my low-risk strategy because it changes only existing Qt Designer metadata and has a clear observable result: a label mnemonic should focus its associated input. The main judgment call is choosing the correct primary widget on rows with multiple controls, so I will verify each mapping instead of applying buddies mechanically.

**Toolkit**

- Tool: issue-scout, using availability, activity, AI-policy, scope, issue-age, and maintainer-responsiveness checks.
- What it produced: A low-risk recommendation for the unclaimed surfaces slice of avogadrolibs #3184.
- Difficulties: Course-site issues frequently remained listed after being claimed, superseded, or covered by an open pull request.
- What changed: I rechecked live GitHub state, withdrew the older Lind claim, and kept only one public active claim.

---

## Understanding the Issue

**Problem Description**

Eight visible field-label candidates in the claimed Qt Designer files do not identify their input widgets through Qt's `buddy` property. Activating a label mnemonic therefore cannot transfer keyboard focus to the intended control.

**Expected Behavior**

Each label that names one field should have a buddy pointing to that field's primary input widget. Descriptive or group labels should remain unlinked.

**Current Behavior**

The files are valid and compile through Qt `uic`, but all seven labels in `surfacedialog.ui` and the one label in `orbitalwidget.ui` currently have no buddy metadata.

**Affected Components**

Only `avogadro/qtplugins/surfaces/surfacedialog.ui` and `avogadro/qtplugins/surfaces/orbitalwidget.ui` are in my claimed slice. No C++ behavior, translations, build files, or unrelated dialogs are in scope.

**Toolkit**

- Tool: Codex repository inspection, constrained to OpenChemistry's permitted AI levels 0-1.
- What it produced: An inventory of eight missing buddies and a candidate label-to-control map for my review.
- Difficulties: Four rows contain secondary controls, so proximity alone cannot prove the intended buddy.
- What changed: I limited the plan to each row's primary field and reserved final mapping approval and implementation for myself.

---

## Reproduction Process

**Environment Setup**

I forked `OpenChemistry/avogadrolibs` to `jaskhetani/avogadrolibs` and made a partial clone at upstream commit `cb0e94852889da9f7288bd1e58241e69990935a7`. Because the host lacked Qt build tools, I built the local Docker image `ai301-avogadro-qt:24.04` with Ubuntu 24.04 and the repository's Qt 6/toolchain dependencies. Its recipe is locally ignored and will not enter the contribution.

**Steps to Reproduce**

1. Mount the unmodified clone read-only at `/workspace` in the container.
2. Run `xmllint --noout` on both claimed `.ui` files.
3. Run Qt 6 `uic` on each file, writing generated headers only to `/tmp`.
4. Inspect the source and confirm that its seven plus one labels lack `buddy` properties.

**Reproduction Evidence**

- **Commit showing reproduction:** None; the baseline was verified at upstream commit `cb0e94852889da9f7288bd1e58241e69990935a7` without changing the clone.
- **Screenshots/logs:** `repro-draft.md` records the passing XML/`uic` baseline and generated-header line counts.
- **My findings:** The source is structurally valid, isolating the gap to missing accessibility metadata. Runtime keyboard-focus behavior remains a post-change check.

**Toolkit**

- Tool: Docker plus `xmllint` and Qt 6 `uic`, with Codex assembling the read-only command.
- What it produced: A repeatable passing baseline for both target files and evidence that all eight candidate labels lack buddies.
- Difficulties: Docker Desktop was initially stopped and the CI-aligned dependency image is large.
- What changed: I started Docker Desktop and kept the recipe local and Git-ignored so it cannot pollute either repository.

---

## Solution Approach

**Analysis**

This is a Qt Designer metadata fix. A `QLabel` buddy names the input widget that should receive focus when the user activates the label's mnemonic. Existing dialogs establish the local pattern; no C++ logic is required.

**Proposed Solution**

Add only appropriate `buddy` properties in the two claimed files. Candidate primary mappings are Surface to `surfaceCombo`, Color by to `propertyCombo`, Colormap to `colormapCombo`, Resolution to `resolutionCombo`, Isosurface Value to `isosurfaceDoubleSpinBox`, Smoothing to `smoothingCombo`, Frame to `stepValue`, and Quality to `combo_quality`. I will confirm each before typing it and skip any group label.

**Implementation Plan**

1. Gemini/Codex may inspect the issue, repository instructions, and UI files; explain buddies; suggest mappings and tests; and review my eventual diff.
2. I will decide the mappings and personally type and understand every XML edit, as required by OpenChemistry's AI policy.
3. I will run the checks and inspect the generated wiring. AI will not author the contribution edits.

Deviation (November 6, 2026): A full Avogadro build and native screen-reader test were not practical in the headless standalone container. I used the focused XML, Qt `uic`, generated-header, and diff checks instead and disclosed that limitation in the PR.

**Toolkit**

- Tool: Claude plan-check, using the implementation plan, issue context, repository instructions, and proposed validation evidence.
- What it produced: An accepted result across all six required plan checks after reviewing the exact two-file scope and eight mappings.
- Difficulties: Its first run could not read the GitHub issue because the session lacked read-only GitHub access.
- What changed: I reran it with bounded read-only access, documented the full-build limitation, and kept the PR disclosure explicit.

---

## Testing Strategy

I reran `xmllint --noout` and Qt 6 `uic` for both changed files in the pinned Docker image; both checks passed. The generated headers contained all eight expected `setBuddy(...)` calls, and the final diff contained only 24 added XML lines across the two intended files. A full application build and native keyboard-focus or screen-reader test were not completed, so the PR states that limitation instead of treating the static checks as complete accessibility proof.

**Toolkit**

- Tool: Dockerized `xmllint`, Qt 6 `uic`, generated-header inspection, and Git diff review, executed and verified by me.
- What it produced: Passing XML and UI compilation checks plus eight exact `setBuddy(...)` calls in generated headers.
- Difficulties: A headless container cannot prove live keyboard-focus or screen-reader behavior.
- What changed: I disclosed the missing runtime check in the PR and retained focused static evidence instead of claiming broader validation.

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**November 5, 2026**

I selected and claimed Lind #375 after the scout accepted it as a straightforward documentation task. I forked the repository, created a docs-only sparse checkout to avoid Windows case-collision noise from vendored Linux headers, recorded the repository rules in the excluded `CLAUDE.local.md`, and reproduced the missing guidance at upstream commit `908cb5a`.

After adding issue-age and maintainer-responsiveness checks, I regraded Lind #375 and withdrew because it failed both new safeguards. I then claimed the unclaimed `qtplugins/surfaces/` slice of OpenChemistry/avogadrolibs #3184. The public withdrawal is https://github.com/Lind-Project/lind-wasm/issues/375#issuecomment-6671132875 and the replacement claim is https://github.com/OpenChemistry/avogadrolibs/issues/3184#issuecomment-6671133419. I kept OpenChemistry #3100 reserved only on the course site as a backup, not as a second public claim.

**November 6, 2026**

I personally added eight `buddy` properties to the two claimed Qt Designer files, then verified valid XML, successful Qt `uic` generation, all eight generated `setBuddy(...)` calls, and the complete two-file diff. I committed the change as `fe4a58db13c6a63e9376a4c8c2aa496ccdfef8a4` with DCO sign-off and pushed `fix/surface-label-buddies` to my fork.

Before opening the PR, I restored the official course voice guard and ship gate, wrote the local plan, manifest, evidence, and PR draft, and corrected their Windows launcher compatibility. Claude's plan-check accepted the plan, ship-gate passed, and Claude Code opened OpenChemistry/avogadrolibs PR #3310.

**Toolkit**

- Tool: Qt Designer XML editing by me, Docker validation, Git/DCO, Claude plan-check, voice guard, and ship-gate.
- What it produced: A signed two-file commit, focused validation evidence, an accepted plan, and a passing PR gate.
- Difficulties: The course hooks assumed Unix command names on Windows, and the standalone clone could not support a practical full application test.
- What changed: I fixed only the local tool launchers, kept all gate artifacts Git-ignored, and documented the test boundary in the PR.

---

## Pull Request

**PR Link:** https://github.com/OpenChemistry/avogadrolibs/pull/3310

**PR Description:** The PR states that it addresses only the `qtplugins/surfaces/` portion of #3184, lists the eight label-to-input mappings, records the passing XML and Qt-generated wiring checks, and discloses both the missing full runtime test and the use of Codex and Claude course tools.

**Maintainer Feedback:**

- November 6, 2026: No maintainer feedback yet. The DCO and WIP checks passed; broader CI and review are pending.

**Status:** Awaiting review

**Toolkit**

- Tool: Claude voice guard, plan-check, and ship-gate, followed by Claude Code running the exact `gh pr create` command.
- What it produced: A voice-compliant PR draft, an accepted six-check plan review, a passing gate, and live PR #3310.
- Difficulties: The first automated plan review was incomplete because live issue access was denied, and Claude's noninteractive PR command required one bounded permission override.
- What changed: I reran the review with read-only GitHub access and authorized only the already-reviewed PR creation command.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned how Qt Designer's `buddy` property becomes generated `QLabel::setBuddy(...)` wiring, how to validate `.ui` files with `xmllint` and Qt `uic`, and how to use a Docker environment for reproducible repository checks. I also practiced DCO sign-off, tightly scoped public claims, and evidence-based PR gating.

**Challenges Overcome**

The course issue list changed faster than its displayed claims, the first selected repository prohibited the intended AI workflow, and an older backup had weak maintainer responsiveness. I solved those problems by checking live GitHub state, tightening the scout criteria, withdrawing the stale claim, and choosing a small slice in an active repository. On Windows, I also repaired local course-tool launcher assumptions without adding those changes to the contribution.

**What I'd Do Differently Next Time**

I would install and test every required course gate before selecting or implementing an issue. I would also include issue age, maintainer response time, AI policy, and competing-PR checks in the scout from the first scan instead of adding them after a claim.

---

## Resources Used

- [OpenChemistry/avogadrolibs issue #3184](https://github.com/OpenChemistry/avogadrolibs/issues/3184), which defined the accessibility gap and the repository slices.
- [OpenChemistry contribution guide](https://github.com/OpenChemistry/avogadrolibs/blob/master/CONTRIBUTING.md), which supplied the DCO and contribution requirements.
- [Merged spectra label-buddy PR #3260](https://github.com/OpenChemistry/avogadrolibs/pull/3260), which demonstrated the accepted local Qt Designer pattern.
