# Voice guide: how I talk upstream

## Who I am in threads

I am a student contributor building experience with Python and AI/ML projects. I write as the person who ran the commands and inspected the result, and I give maintainers enough concrete detail to verify my work without pretending I already know the fix.

## Rules I write by

### Rule: Name the concrete behavior

I tie my comment to the issue's actual trigger, error, or component instead of posting a generic request for assignment.

- Wrong: "Hi, can you assign this issue to me?"
- Right: "I'd like to investigate the output parser crash when the fallback receives a top-level JSON array."

### Rule: Promise an investigation, not an outcome

Before I have evidence, I say what I will test next. I do not guarantee a fix, a root cause, or a delivery date.

- Wrong: "I know the parser is broken and will have the fix ready tomorrow."
- Right: "I'll reproduce the array fallback, trace the parser branch it reaches, and report what I find."

### Rule: Separate observations from hypotheses

I label what the artifact shows separately from what I think may explain it.

- Wrong: "This proves the exception is caused by the JSON repair library."
- Right: "The traceback reaches the JSON fallback; my next step is to check whether the repair-library return shape explains the exception."

### Rule: State limits plainly

I name version or environment differences and say when I could not reproduce instead of smoothing over uncertainty.

- Wrong: "Confirmed everywhere."
- Right: "I reproduced this on Windows 11 with Python 3.12; I have not tested other platforms yet."

### Rule: Keep the evidence easy to scan

I put commands, exact output, and expected-versus-actual behavior in direct language, without filler or exaggerated confidence.

- Wrong: "After a lot of testing, it is very obvious that this definitely fails."
- Right: "Expected: the fallback returns parsed content. Actual: it raises `AttributeError` on the top-level array."

## Things I never post

- A guaranteed fix, deadline, merge, or root cause I have not demonstrated.
- A generic "+1," "same as above," or assignment request with no issue-specific intent.
- A reproduction claim without the command or artifact that supports it.
- Hidden environment differences or certainty broader than the setup I tested.
- AI-assisted text that violates the repository's disclosure policy.
