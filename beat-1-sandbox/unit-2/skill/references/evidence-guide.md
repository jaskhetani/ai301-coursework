# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives.** In an eval bundle, compare the repro report's environment line and setup details with the issue body and the repo-facts block. In live mode, compare the draft report with the issue, its labels and maintainer comments, the bug-report template, and relevant repository setup documentation.

**What good looks like.** The record names the operating system or platform, the tested project/tool version or commit, and versions or settings that could change the trigger. It need not inventory irrelevant hardware or packages. A different version or platform is acceptable only when the report calls out the difference and limits the conclusion accordingly.

## Steps

**Where it lives.** In an eval bundle, use the repro report's commands, code/configuration snippets, fixture descriptions, and ordered actions, checked against the trigger in the issue context. In live mode, use the draft plus any public gist, branch, or minimal repository it explicitly links; do not silently fill gaps from unposted local files.

**What good looks like.** A stranger can start from the stated setup, construct the necessary input/configuration, run the relevant command or action, and reach the observation. The steps must preserve trigger details such as argument syntax, ordering, locale, shell, driver, build mode, or UI action when those details matter. Private repositories and unshared configuration do not make rerunnable evidence.

## Behavior shown

**Where it lives.** Find artifacts in fenced output, logs, screenshots, measurements, test failures, exit codes, or before/after and control results in the repro report. Read them against the issue body's exact symptom and expected behavior, not against the report's label for them.

**What good looks like.** The artifact contains an observable signal unique enough to support the conclusion: for example, the reported error text or exit mode, a missing/incorrect value, a failing assertion, or a measured UI state. A version banner, successful startup, visible tabs, or an author's statement that a failure occurred is not proof of that failure. When a nearby failure could look similar, a control or discriminating signal should rule it out.

## Honesty

**Where it lives.** Compare the claim comment and the report's expected/actual and conclusion language with the environment, exact steps, raw artifacts, issue context, and maintainer clarifications. Inspect root-cause language separately from symptom evidence.

**What good looks like.** The wording tracks what actually happened and preserves uncertainty. A faithful attempt that does not trigger the bug can pass when it shows the non-reproduction, identifies meaningful differences, and avoids claiming the issue is false. A report fails when it calls validation output a crash, generalizes beyond the tested setup, hides a changed trigger, reverses expected and actual behavior, or presents an unsupported cause as verified.

## Comms

**Where it lives.** In an eval bundle, compare the claim comment with the issue title/body/thread and compare both comments with the repo-facts contribution-policy and bug-template notes. In live mode, also check the current issue template, CONTRIBUTING file, CODE_OF_CONDUCT, any AI policy, and maintainer instructions in the thread.

**What good looks like.** The claim refers to this issue and names a concrete next investigation or reproduction action without guaranteeing a fix or deadline. The report supplies the information the repository explicitly asks for. If the repository requires disclosure of AI assistance, the posted words state the tool and extent required by that policy; if it does not, absence of a disclosure is not a failure. Classroom house rules allow an independent claim and report even when classmates have already posted, but never allow a content-free piggyback.
