# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

[Issue #69 — Output parser crashes on a top-level JSON array fallback](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69)

**Verdict output**

**Top tier — quick Python bugs in the core AI/ML pipeline (best fit):**
1. **#69** rag output-parser crash on JSON array — good-first-issue, tier-1, RAG core

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69",
  "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "Last 5 default-branch commits (2026-08-24 to 2026-09-16), all authored by human Aburke225; most recent is 8 days before the 2026-09-24 capture date, well within 1 year"},
    {"name": "repo-used", "grade": "pass", "evidence": "repo not archived; pushed_at=2026-09-16T21:48:27Z, 8 days before capture date 2026-09-24, within 30 days"},
    {"name": "issue-available", "grade": "pass", "evidence": "cross-referenced only by classmate coursework PR(s) in a different repo, not this codebase: https://github.com/charancherry0/ai301-coursework/pull/1 (MERGED)"},
    {"name": "ai-policy-compatible", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md are silent on AI-generated contributions; no AGENTS.md/AI_POLICY.md/ban found"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

`agreement: 9/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in dead-repo, policy, scope)`

`agreement: 12/20 scored items`

`agreement: 17/20 scored items`

`agreement: 18/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

`issue-20  reject  accept   NO     graded accept`

For `issue-20`, my rubric decided `accept`, while the gold label was `reject`. The
repository passed my activity checks, the issue had no assignee, open linked PR, or
recent claim, and the contribution policy did not prohibit AI-assisted work. My final
rubric intentionally has no scope check, so the unconfirmed feature direction and TBD
logo asset did not change its verdict.

**Check rationale**

> `| issue-available | Assignees and linked PRs under Repo facts, plus claim and PR comments in the issue thread | No assignee, no open linked or comment-mentioned PR, and no active claim within the previous 90 days. Older claims with no open PR or follow-up activity count as stale | required |`

I made this check required because an unassigned issue can still be unavailable when
someone already has an open PR or has recently said they are working on it. The 90-day
limit distinguishes current work from old claims that never produced an active PR.

**Trade-offs**

`categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 2/4`

This check correctly rejected the claimed issues while preserving old, inactive claims
as available. The trade-off is that it can miss work that has continued silently for
more than 90 days without an assignee, linked PR, or recent follow-up comment.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. This issue fits my interest in quick Python fixes in AI/ML projects because it is a
   focused bug in the RAG output parser. The issue estimates 2–4 hours, identifies the
   two relevant files, and already has an xfail test to guide the fix, so it fits the
   time I have available.
2. The verdict correctly identified that the repository is active, its contribution
   policy does not prohibit AI-assisted work, and the issue is available under the
   Path Review house rule. Beyond the rubric, I weighed how clearly the failure is
   reproduced, whether the requested behavior is bounded, and whether I can validate
   the result. The existing array-fallback test and explicit instruction to remove its
   xfail marker make the work manageable and verifiable.
3. I expect claiming it to be straightforward. Issue #69 is open and unassigned, and
   the target repository has no open pull request fixing it. Another student commented
   on the issue and a merged coursework PR in a different repository selected it, but
   the course house rule says classmates' claim signals do not block an issue and that
   credit attaches to the pull request. I can therefore comment that I am working on it
   and open my own focused PR.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
