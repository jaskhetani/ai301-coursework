# Contribution: Add "constant placement" style guidance to documentation

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Jas Khetani
**GitHub Username:** jaskhetani
**Issue:** https://github.com/Lind-Project/lind-wasm/issues/375
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

I found this issue on page 2 of the AI301 Issue Selection site after parsing twelve saved pages containing 1,200 unique issues. My scout rejected the seemingly easiest alternatives because they already had claim comments, open pull requests, maintainer reservations, or contribution restrictions. It accepted Lind #375 as straightforward: the repository is active, the issue had no assignee or comments, no pull request was implementing it, and the requested Markdown guidance was still absent from `docs/contribute/styleguide.md`.

This issue fits my low-risk strategy because the maintainer already supplied the three rules that need to be documented, so the contribution is bounded without being a copy-paste change. I hope to learn how an established Rust project organizes contributor guidance and reviews a small documentation proposal. The scout was useful at detecting stale issue labels, but its broad search surfaced many false positives; live checks of the complete comment thread and linked pull requests were essential. I claimed the issue on November 5, 2026: https://github.com/Lind-Project/lind-wasm/issues/375#issuecomment-6667018036

**Toolkit**

- Tool: issue-scout, using my low-risk language profile and required availability, activity, AI-policy, and scope checks.
- What it produced: An accept verdict with a straightforward difficulty note for Lind #375 after confirming the exact documentation gap.
- Difficulties: Course-site issues frequently remained listed after being claimed, superseded, or covered by an open pull request.
- What changed: I treated live comments and linked pull requests as decisive and rejected every stale or contested candidate before choosing this one.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

I forked and cloned `Lind-Project/lind-wasm` on Microsoft Windows 11 Pro 10.0.26100 with Git 2.51.0.windows.1 and Python 3.13.7. I checked upstream commit `908cb5a93b511f1db62e20aab4975f4401f7c6d6`. A full checkout produced case-collision changes from vendored Linux headers on NTFS, so I limited the fresh clone to `docs/` and `.github/` with Git sparse checkout; this does not affect the documentation file under review.

**Steps to Reproduce**

1. From the clone root, run `git grep -n -i -E "constant|crate-wide|project-wide|file-local" -- docs/contribute/styleguide.md`.
2. Run `git show HEAD:docs/contribute/styleguide.md` to display the complete current source page.
3. The targeted search produces no output, and the page contains only the default Rust style link and two `cargo fmt` commands. It has no guidance for local, crate-wide, or project-wide constants.

**Reproduction Evidence**

- **Commit showing reproduction:** None; the gap was verified against upstream commit `908cb5a93b511f1db62e20aab4975f4401f7c6d6` without changing the repository.
- **Screenshots/logs:** The zero-match `git grep` result and complete eight-line `styleguide.md` source are recorded in `repro-draft.md`.
- **My findings:** The documentation gap is current and reproducible. `docs/contribute/styleguide.md` is the smallest likely location because `CONTRIBUTING.md` already links to it, while a separate page would also require a `mkdocs.yml` navigation change.

**Toolkit**

- Tool: repro-check, used on the public claim and a rerunnable documentation-gap report.
- What it produced: An accept verdict on all six required checks, including direct evidence that the guidance is absent at the pinned commit.
- Difficulties: Its first accepted draft used a PowerShell-only read command and called a condensed excerpt complete.
- What changed: I replaced that command with shell-neutral `git show`, restored the exact Markdown link and code fence, and added explicit Expected and Actual evidence.

---

## Solution Approach

**Analysis**

The missing guidance is a documentation gap rather than a runtime defect. `CONTRIBUTING.md` directs contributors to `docs/contribute/styleguide.md`, but that page currently contains only the default Rust style link and two formatting commands. Issue #375 and its linked discussion provide the three constant scopes that the page must distinguish.

**Proposed Solution**

Add one short constant-placement section to the existing style guide, using the terminology confirmed by the issue discussion. Keep the change documentation-only: do not move constants, alter Rust code, create a new page, or modify navigation unless the maintainer asks for a different location.

**Implementation Plan**

1. Give Gemini/Codex the issue thread, linked discussion, current style-guide page, and repository contribution rules. Use the AI tools to compare the sources, identify ambiguities, propose a concise outline, and flag wording that claims more than the maintainers specified.
2. I will choose the final wording and type the documentation change in `docs/contribute/styleguide.md`, understanding and approving every changed line. Gemini/Codex may explain Rust terminology and review the diff, but it will not independently decide project policy or submit generated work without my review.
3. Give Gemini/Codex the completed diff and repository commands so it can suggest focused validation checks and look for scope drift, broken Markdown, or missing disclosure. I will run the checks locally, inspect their output, and decide whether the change is ready to commit and submit.
4. If I switch to an OpenChemistry backup, keep the same boundary required by its AI policy: AI may inspect, explain, plan, suggest tests, and review; I will select the issue slice, implement it, run it, and verify every changed line.

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**November 5, 2026**

I selected and claimed Lind #375 after the scout accepted it as a straightforward documentation task. I forked the repository, created a docs-only sparse checkout to avoid Windows case-collision noise from vendored Linux headers, recorded the repository rules in the excluded `CLAUDE.local.md`, and reproduced the missing guidance at upstream commit `908cb5a`.

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
