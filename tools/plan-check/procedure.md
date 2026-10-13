# Procedure: how this skill grades a plan package

## Read order

1. In live mode, read `scope.md`, confirm the issue repository is in scope, and record the applicable Path Review house rules. Then read `voice-guide.md`. In eval mode, skip both files.
2. Read the issue context and thread before the candidate plan. Record the requested behavior, maintainer directions, linked or competing approaches, and repository conventions without adopting any diagnosis yet.
3. Read the repro evidence next. Record the exact trigger, environment, failure artifact, controls, and what each control rules out. Treat these observations as the boundary any diagnosis must explain.
4. Read the complete candidate plan, then the candidate plan comment. Record the diagnosis, in/out scope, named files, chosen mechanism, tests and expected observations, risks, unknowns, and deviations. Reading the comment last makes it possible to compare its public promise with the actual plan.

## Evidence gathering

1. For diagnosis evidence, pair each causal statement in the plan with the repro artifact or thread fact that supports it. Also record any control or maintainer finding that contradicts it.
2. For scope evidence, list every proposed production change, test change, migration, refactor, platform expansion, and explicitly deferred item. Compare that list with the issue's requested outcome.
3. For executability evidence, record the named files or code areas, selected layer and mechanism, order of changes, and any decision left for implementation time.
4. For test evidence, map each proposed test to the original trigger, the real changed path, its command or fixture, and the exact post-fix observation. Record controls and broader regression runs separately.
5. For honesty evidence, record each risk, unknown, alternative, and deviation together with how it will be resolved, bounded, or deferred.
6. For communication evidence, compare the plan comment with the candidate plan, thread directions, competing work, and repository policy. Record required disclosures and whether the comment contains them.

## Check execution

1. Grade `diagnosis-grounded` first because later checks cannot rescue a cause contradicted by the reproduction.
2. Grade `scope-bounded` and `approach-executable` from the gathered change list and mechanism. Do not reward extra detail when the plan still leaves the core layer or approach undecided.
3. Grade `tests-decisive` by tracing at least one proposed check from the original input through the changed code to an observable result. A suite name alone is not an observation.
4. Grade `uncertainty-honest`, distinguishing a bounded secondary unknown from a core design decision deferred to the build.
5. Grade `thread-and-conventions` last so the comment can be checked against all prior findings and the actual plan.
6. Give each check `pass`, `fail`, or `unclear` and cite one decisive package fact or short quote. Use `unclear` only when required evidence is absent; use `fail` when present evidence violates the pass condition. Do not infer missing files, commands, policy compliance, or intent.

## Verdict assembly

1. Confirm that every rubric row has exactly one grade and evidence line.
2. Apply the rubric rule mechanically: any required `fail` or `unclear` produces `reject`; only all required passes produce `accept`. Preferred rows never change the verdict.
3. In the readable summary, lead with the first deciding failure and name any additional failures without inventing remediation.
4. Emit the required fenced JSON using the issue URL or bundle id, the checks in rubric order, and the binary verdict. Validate that the JSON is parseable and print nothing after the fence.
