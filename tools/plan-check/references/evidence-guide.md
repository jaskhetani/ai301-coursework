# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives.** In an eval bundle, read the plan's diagnosis and approach against the issue context, thread highlights, and especially the repro-evidence block's raw outputs and controls. In live mode, use the draft plan plus the student's posted reproduction comment and current issue thread.

**What good looks like.** The cause accounts for the exact trigger and artifact and survives the controls. A control that reproduces with a suspected subsystem absent rules that subsystem out; an artifact showing corruption before a proposed fix point rules out that downstream point. Hypotheses are acceptable only when labeled and bounded rather than presented as established cause.

## Scope

**Where it lives.** Use the plan's in-scope and not-in-scope statements, file list, approach, risks, and comment. Compare every proposed production and test change with the issue's requested outcome and maintainer direction.

**What good looks like.** The smallest coherent set of changes addresses the reproduced cause and includes its regression coverage. Adjacent cleanup, architectural migrations, new configuration or UI, broad cross-platform work, and speculative related bugs are deferred. A deliberate partial scope can pass when it still solves the stated issue and clearly explains the boundary.

## Executability

**Where it lives.** Find the files, functions, components, selected implementation layer, data/control flow, change order, and acceptance condition in the plan's approach and file list. In live mode, verify named paths against the repository only when necessary to understand the plan; do not use unquoted local work to fill plan gaps.

**What good looks like.** Another contributor can identify the first edit and continue without choosing among “upstream or vendored,” “UI or backend,” or other core alternatives. Exact line numbers are optional, but ownership and mechanism are selected. Investigation may tune a threshold or placement after measurement when the fallback is bounded.

## Test plan

**Where it lives.** Compare the repro-evidence inputs, commands, outputs, exit modes, screenshots, or measurements with the candidate plan's focused tests, controls, and broader suite runs.

**What good looks like.** At least one post-change check uses the reproduced trigger on the real path and predicts a visible outcome that would fail before and pass after. Regression tests encode the issue case when practical, while controls protect nearby behavior. “Run all tests,” “benchmark it,” or “verify it feels fixed” alone does not decide the issue.

## Honesty

**Where it lives.** Read the diagnosis language, risks and unknowns, alternatives, exclusions, and `## Deviations` note. Compare certainty in the plan comment with what the repro and thread actually establish.

**What good looks like.** Demonstrated observations, working hypotheses, and open questions are distinguishable. Unknowns include a bounded check or explicit deferral and do not postpone the core design. After implementation, deviations say what changed and why; “nothing changed” is valid when stated explicitly.

## Comms

**Where it lives.** In eval mode, compare the candidate plan comment with the issue thread highlights, repo-facts policy/template notes, candidate plan, and linked prior work. In live mode, use the current issue thread, CONTRIBUTING and AI-policy files, and the student's draft comment.

**What good looks like.** The comment tells maintainers the evidence-backed cause, bounded approach, and decisive test without overpromising. It acknowledges explicit maintainer direction and open or prior approaches rather than silently racing them. It follows comment-specific AI rules and discloses the tool and extent when policy requires; silence or merely permissive PR guidance does not create a disclosure requirement.
