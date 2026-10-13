---
name: plan-check
description: Grade a plan package (a candidate plan plus a candidate plan comment, read against the issue and its reproduction evidence) and decide whether it is ready to post and build from. Use when checking a draft plan or plan comment before posting, or when grading an eval package bundle.
---

# plan-check: rubric-driven plan grading

You are grading one plan package to answer a single question: is this
ready to post and build from? A package is a candidate plan and a
candidate plan comment, read against the issue they belong to and the
reproduction evidence that plan builds on. You do not answer from gut
feel, and this file no longer tells you how to work: you answer by
executing the student-authored grading procedure in `procedure.md`,
which applies the rubric in `rubric.md` to evidence gathered per
`references/evidence-guide.md`.

## Inputs

One of:

- **Live mode**: the student's own `plan.md` and draft plan comment,
  plus their issue's URL. Gather the issue-side evidence live (via
  `gh`, the GitHub API, or the web; `references/evidence-guide.md`
  says where each signal lives), and take the reproduction evidence
  from the student's posted repro comment on that issue (their week-2
  proof; the plan must follow from it). A student on the house issue
  has no posted repro comment of their own: there, the reproduction
  evidence is the house repro pack as quoted in the drafts, and if the
  drafts quote no repro evidence at all, that absence is what the
  relevant checks grade. Read the drafts the way a
  maintainer on the thread will read the posted comment: the package
  is what the drafts contain and quote, not other files in the
  student's working directory.
- **Eval mode**: a package bundle (a markdown file containing the
  issue context, a repo-facts block, a repro-evidence block, the
  candidate plan, and a candidate plan comment). Use ONLY the bundle
  text as evidence. Do not fetch anything; the bundle is the whole
  world. Eval mode always grades a complete package: every check,
  full verdict rule.

## The scope sets the field (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It names where the student's issue must live and the house rules
that apply there; a house rule changes how evidence is read in that
environment. Refuse to grade a package for an issue outside the scoped
source. If the scope's repo line still carries an unfilled placeholder,
stop without grading and tell the student to get their cohort's scope
file from the instructor; never guess a scope. In eval mode, ignore
`scope.md` entirely.

## The voice guide gates outgoing words (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream, with wrong/right examples. Check the draft
plan comment against those rules and report any rule the draft breaks
in the summary, quoting the rule. The voice guide never changes the
rubric's verdict on its own unless the rubric has a check that reads
it. In eval mode, ignore `voice-guide.md` entirely: voice is personal
and carries no gold labels; the universal communication-quality checks
live in the rubric.

## The rubric is the brain

Read `rubric.md`. It defines:

1. A table of checks. Each row names the check, the evidence to
   gather, the pass condition, and its weight: `required` checks gate
   the verdict; `preferred` checks never change it.
2. A verdict rule: how check results combine into a final verdict.

`references/evidence-guide.md` is the rubric's map: where each kind of
evidence lives in a plan package (and, live, on GitHub), and what good
looks like there.

## The procedure is the hands

**Execute `procedure.md`.** That file must decide the read order, how
each evidence family gets gathered, how a check executes against the
gathered evidence, and how check grades become the verdict. Follow it
exactly. Where it is silent, note the gap in the summary rather than
silently inventing a step.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in, stop and say so. This skill cannot grade without both.

## Verdict and output

The verdict space is binary: `accept` (ready to post and build from)
or `reject` (hold). Emit a fenced JSON block, then nothing else after
it:

```json
{
  "item": "<issue URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

Before the JSON block you may show a short readable summary. The JSON
block must be valid and last because the eval harness parses it.

## After an honest deviation (live mode)

When a build deviates from the posted plan and the student records
what changed and why in `plan.md`, re-run this skill on the updated
package before posting a follow-up. A recorded deviation is honest
work; a deviation visible only in the diff is not.

## Grading discipline

- Name the evidence that decides every check.
- Grade outcomes, not polish or document shape.
- Apply the rubric and procedure exactly; report gaps instead of
  improvising around them.
- Follow the rubric's treatment of `unclear`; absent verifiable
  evidence is not a silent pass.
