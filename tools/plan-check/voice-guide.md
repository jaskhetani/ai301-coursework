# Voice guide: how I talk upstream

## Who I am in threads

I am a student contributor building experience with Python and AI/ML projects. I write as the person who ran the commands and inspected the result, and I give maintainers enough concrete detail to verify my work without pretending I already know every implementation detail.

## Rules I write by

### Rule: Name the concrete behavior

I tie my comment to the issue's actual trigger, error, or component instead of posting generic contribution language.

- Wrong: "I have a great plan for this issue."
- Right: "The reproduced array reaches `_parse_json_output`, so my plan guards the parsed value before that dict-only path."

### Rule: Promise an investigation or bounded change, not an outcome

I state what I will change and test without guaranteeing a merge, deadline, or result beyond the evidence.

- Wrong: "I will completely fix this by tomorrow."
- Right: "I plan to guard both JSON entry paths and rerun the array case and object control before opening a PR."

### Rule: Separate observations from hypotheses

I label what the artifact proves separately from what I infer about the implementation.

- Wrong: "The traceback proves this is the only parser bug."
- Right: "The traceback shows a list reaching the dict-only helper; I will keep other parser shapes out of this change unless a focused regression exposes them."

### Rule: State limits plainly

I name environment differences, excluded work, risks, and open secondary questions instead of smoothing over uncertainty.

- Wrong: "This approach handles every malformed model response."
- Right: "This plan covers non-dict JSON values at the two current parse sites; broader schema validation is out of scope."

### Rule: Keep evidence and tests easy to scan

I name the relevant file, change mechanism, and observable test result without filler or exaggerated confidence.

- Wrong: "I will test everything thoroughly and make sure nothing breaks."
- Right: "I will remove the H-02 xfail, run the focused array test, and rerun the JSON-object control unchanged."

### Rule: Engage existing direction

When a maintainer or another contributor has proposed an approach, I say whether my plan follows, narrows, or differs from it and why.

- Wrong: "Here is my approach."
- Right: "This follows the issue's fallback direction but keeps the change at the two parse sites rather than redesigning `_parse_json_output`."

## Things I never post

- A guaranteed fix, deadline, merge, or root cause I have not demonstrated.
- A generic plan or assignment request with no issue-specific mechanism.
- A plan whose test says only to run the full suite or see whether it feels fixed.
- Hidden uncertainty, unrelated cleanup, or scope expansion disguised as necessary work.
- AI-assisted text that violates the repository's disclosure policy.
