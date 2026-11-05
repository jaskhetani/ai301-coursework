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
**Pull Request:** [GitHub PR link, once opened]

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

**Unit 6: Guarded the public claim and cached repository-specific instructions**

The voice guard checked the claim that I posted through Claude Code, and the accepted comment named my first step without promising an outcome. I created a 31-line `CLAUDE.local.md` with the DCO, PR-title, documentation-check, and AI-disclosure rules, then excluded it through `.git/info/exclude`. A full Windows checkout appeared dirty because vendored Linux headers differ only by capitalization, so I changed the fresh clone to a docs-only sparse checkout and recorded that limitation in the memory file.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

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
3. I will run the checks, inspect generated wiring, and manually test keyboard focus. AI will not author or submit the contribution.

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

Re-run `xmllint --noout` and Qt 6 `uic` for both changed files in the pinned Docker image. Confirm the diff contains only intended `buddy` properties and generated headers contain matching `setBuddy(...)` calls. Finally, build/run the affected UI and activate each mnemonic to verify focus moves to the intended enabled control. Static checks prove valid syntax and generated wiring; the manual check proves user-visible behavior.

**Toolkit**

- Tool: Dockerized Qt 6 validation planned by Codex and executed/verified by me.
- What it produced: A passing pre-change XML/`uic` baseline and a precise post-change checklist.
- Difficulties: A headless container cannot prove live keyboard-focus or screen-reader behavior.
- What changed: I added a required manual application check instead of treating generated code as complete accessibility proof.

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**November 5, 2026**

I selected and claimed Lind #375 after the scout accepted it as a straightforward documentation task. I forked the repository, created a docs-only sparse checkout to avoid Windows case-collision noise from vendored Linux headers, recorded the repository rules in the excluded `CLAUDE.local.md`, and reproduced the missing guidance at upstream commit `908cb5a`.

After adding issue-age and maintainer-responsiveness checks, I regraded Lind #375 and withdrew because it failed both new safeguards. I then claimed the unclaimed `qtplugins/surfaces/` slice of OpenChemistry/avogadrolibs #3184. The public withdrawal is https://github.com/Lind-Project/lind-wasm/issues/375#issuecomment-6671132875 and the replacement claim is https://github.com/OpenChemistry/avogadrolibs/issues/3184#issuecomment-6671133419. I kept OpenChemistry #3100 reserved only on the course site as a backup, not as a second public claim.

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
