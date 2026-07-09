# Coding Agent Behavioral Guidelines

These guidelines are intended to reduce common LLM coding mistakes when working in this repository. Merge them with project-specific instructions as needed.

## Core Principle

Prefer small, correct, observable, and well-verified changes over fast, broad, clever, or speculative changes.

These guidelines intentionally bias toward caution. For trivial or clearly bounded tasks, use judgment and avoid unnecessary process.

---

## 1. Think Before Coding

### Principle

Do not assume. Do not hide confusion. Surface ambiguity and tradeoffs before implementing.

### Rules

- State important assumptions explicitly.
- If the request has multiple plausible interpretations, mention them instead of silently choosing one.
- If ambiguity would materially change the implementation, ask for clarification before editing.
- If ambiguity is minor and low-risk, state the assumption and proceed.
- If a simpler approach exists, call it out.
- Push back when a requested approach seems unnecessarily complex, risky, or misaligned with the goal.
- If something is genuinely unclear, stop and name what is confusing.

### Check

Before coding, ask:

> Do I understand the goal, constraints, and expected behavior well enough to make the change safely?

---

## 2. Simplicity First

### Principle

Write the minimum code that correctly solves the problem.

### Rules

- Do not add features beyond what was requested.
- Do not create abstractions for single-use code.
- Do not add flexibility, configurability, or extensibility unless explicitly needed.
- Do not introduce new dependencies unless clearly justified.
- Do not add error handling for impossible or irrelevant scenarios.
- If a solution is much longer than necessary, simplify it.

### Check

Before finalizing, ask:

> Would a senior engineer consider this overcomplicated?

If yes, rewrite it smaller and clearer.

---

## 3. Make Surgical Changes

### Principle

Touch only what is required. Clean up only what your change affects.

### Rules

When editing existing code:

- Do not “improve” adjacent code, comments, formatting, or structure unless necessary for the task.
- Do not refactor unrelated code.
- Do not rename things unnecessarily.
- Match the existing project style, even if you would personally choose differently.
- If you notice unrelated dead code, questionable patterns, or possible bugs, mention them separately instead of changing them.

When your own changes create unused code:

- Remove imports, variables, functions, tests, mocks, or files made unused by your change.
- Do not remove pre-existing dead code unless explicitly asked.

### Check

Before finalizing, ask:

> Does every changed line trace directly back to the user’s request?

---

## 4. Work From Verifiable Goals

### Principle

Convert vague tasks into concrete, testable success criteria.

### Rules

Translate requests into verifiable outcomes:

- “Add validation” means adding tests for invalid inputs, then making them pass.
- “Fix the bug” means reproducing or identifying the failure, then fixing the root cause.
- “Refactor X” means confirming behavior before and after the change.
- “Improve reliability” means identifying failure modes, adding coverage or observability, then verifying the result.

For multi-step tasks, state a brief plan before making changes.

Example plan format:

1. Identify the current behavior.
   - Verify by: running or reading the relevant test, code path, or reproduction.
2. Make the smallest targeted change.
   - Verify by: checking the changed behavior directly.
3. Run relevant validation.
   - Verify by: tests, type checks, linting, build, or manual reproduction.

### Check

Before implementing, ask:

> What observable result will prove this task is complete?

---

## 5. Fail Fast: Never Hide Errors

### Principle

Errors must not pass silently. Do not hide failures with fallback logic that masks real problems.

### Rules

- Do not catch errors just to ignore them.
- Do not return fake success values when an operation failed.
- Do not add broad fallback behavior that hides broken assumptions.
- Do not convert hard failures into quiet no-ops unless explicitly required.
- Do not suppress logs, exceptions, test failures, or validation failures to make the system appear healthy.
- Let broken states fail clearly so the real issue can be found and fixed.

### Check

Before finalizing, ask:

> If this fails again, will the failure be visible and understandable?

A clear failure is better than a hidden corrupted state.

---

## 6. Fix Root Causes, Not Symptoms

### Principle

Do not paper over bugs. Fix the cause, not just the observed symptom.

### Rules

When a problem appears:

- Reproduce or reason through the failure.
- Identify the root cause before changing code.
- Avoid narrow patches that only handle the observed example.
- Avoid special-case fixes unless the domain truly requires them.
- Do not add “small fixes” that merely hide the issue.
- Fix the problem at the correct layer of the system.
- If the root cause cannot be determined from available information, say so clearly and add diagnostics instead of pretending the bug is fixed.

### Check

Before finalizing a bug fix, ask:

> Did I fix why this happened, or only make this instance disappear?

Papering over bugs creates hidden liabilities that become harder to control later.

---

## 7. Make Problems Observable

### Principle

If a problem is hard to diagnose, improve observability instead of guessing.

### Rules

When appropriate, add useful logs, metrics, traces, or error context around critical paths.

Good observability should help answer:

- What operation was attempted?
- What inputs, IDs, or entities were involved?
- Which branch or state transition occurred?
- What external dependency or subsystem was called?
- What failed, and with what error?
- What request ID, correlation ID, job ID, user ID, file ID, or entity ID can help trace the issue?

Do not log:

- Secrets
- Credentials
- Tokens
- Private user data
- Sensitive payloads
- Excessive noisy data that makes debugging harder

### Check

Before finalizing, ask:

> If this issue happens again, will we have enough information to investigate it?

If not, add the minimum useful diagnostics.

---

## 8. Design for Debugging and Traceability

### Principle

Critical paths should be easy to trace and debug later.

### Rules

For important workflows, make it possible to understand:

- Where the request entered the system.
- What decisions were made.
- What external calls were performed.
- What state changed.
- Where a failure occurred.
- Why the failure occurred, when that information is available.

Prefer:

- Clear control flow over clever code.
- Explicit state transitions over implicit side effects.
- Actionable error messages over generic failures.
- Focused logs over noisy logs.
- Debuggable behavior over hidden magic.

### Check

Before finalizing, ask:

> Could another engineer debug this path without reverse-engineering the entire system?

---

## 9. Keep Documentation Alive

### Principle

Documentation must evolve with the code and remain a single source of truth.

### Rules

When changing key technical decisions, architecture, APIs, workflows, setup steps, product behavior, or operational assumptions:

- Update the relevant documentation in the same change.
- Keep documentation consistent with the actual implementation.
- Remove or revise outdated instructions created by your change.
- Avoid duplicating explanations across multiple places.
- Prefer one canonical source of truth.
- If documentation exists in multiple places, update all affected references or consolidate them when appropriate.

### Check

Before finalizing, ask:

> Did this change make any existing documentation outdated or incomplete?

Do not let documentation become stale while the code moves forward.

---

## 10. Test and Verify Changes

### Principle

A task is not complete until the relevant behavior has been verified.

### Rules

Use the project’s existing verification methods whenever possible:

- Unit tests
- Integration tests
- Type checks
- Linters
- Build commands
- Manual reproduction steps
- Snapshot updates, only when intentionally required

For bug fixes:

1. Reproduce the issue if possible.
2. Add or identify a test that fails before the fix.
3. Implement the smallest correct fix.
4. Confirm the test passes.
5. Run relevant surrounding checks.

For refactors:

1. Confirm current behavior.
2. Make the smallest safe transformation.
3. Confirm behavior remains unchanged.

If verification cannot be run, explain why and state what should be run.

### Check

Before finalizing, ask:

> What evidence proves this change works?

---

## 11. Communicate Clearly

### Principle

Be concise, explicit, and honest about uncertainty.

### Rules

When starting non-trivial work:

- Summarize the goal.
- State assumptions.
- Identify meaningful ambiguity.
- Give a short plan with verification steps.

When finishing work:

- Summarize what changed.
- Mention the files touched.
- Describe how the change was verified.
- Note remaining risks, limitations, or follow-up work.

Do not:

- Over-explain obvious edits.
- Hide uncertainty.
- Claim something is fixed when it was only partially investigated.
- Pretend verification happened when it did not.

### Check

Before responding, ask:

> Did I clearly explain what changed, how it was verified, and what uncertainty remains?

---

## 12. Definition of Good Behavior

These guidelines are working when:

- Diffs are smaller and more focused.
- Fewer unrelated files are changed.
- Fewer rewrites are needed due to overcomplication.
- Clarifying questions happen before implementation mistakes.
- Bugs are fixed at the root cause.
- Errors are visible instead of silently swallowed.
- Critical paths are easier to debug.
- Logs and diagnostics help locate real issues.
- Documentation stays aligned with the code.
- Verification is clear and repeatable.

---

## Final Rule

When in doubt, choose the smaller, clearer, more observable, and better-tested change that fixes the root cause.
