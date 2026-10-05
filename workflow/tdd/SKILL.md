---
name: tdd
disable-model-invocation: false
description: Develop or fix observable behavior by writing a failing test, implementing the change and refactoring. Use when explicitly requested for test-driven work or as supporting guidance during implementation.
---

# TDD

Use short feedback cycles to verify behavior through meaningful interfaces. Apply this method during implementation or invoke it directly for a bounded change. It does not start other phases.

## Resolve scope first

Identify who owns the authoritative work artifact, which repositories will run the work and who owns the relevant domains. Resolve each separately. Use the artifact owner's examples for service-local and app-owned inputs. An explicit artifact owner takes precedence over the launch directory. Read the owner's workflow, tracker and context configuration, along with guidance in the execution repositories. Keep repository-scoped paths in the saved handoff.

For each repository, identify the baseline, available dependencies, working-tree identity and required checks.

## Establish behavior

Read the authoritative ticket or supplied requirements, the owner and execution-repository guidance, and the relevant tests. Keep ticket evidence with the ticket's owner, including when an app-owned input spans services. Use project domain terms and follow applicable architecture decision records. Resolve ambiguity that affects the contract before writing a test. Follow the existing codebase's test boundaries without asking for repeated approval.

Prefer the lowest-cost public boundary that reliably observes the behavior. Add broader integration coverage when it catches risks that the focused test cannot. Do not force every test through the highest-level endpoint. Do not test private implementation details just because they are easy to reach.

Use TDD for new behavior and reproducible bugs. Before a mechanical refactor, use meaningful existing tests or characterization tests to preserve behavior. Those tests may already pass. For documentation or trivial wiring, a new test may only repeat the implementation. Use existing checks that match the risk and explain the exception. Do not manufacture a failing test to satisfy the process.

## Loop

1. Select one required behavior and an observation that could prove it wrong. Derive expected values from requirements, examples worked out independently or established contracts. Do not copy the implementation logic.
2. Add or adapt a focused test. Run it and confirm that it fails because of the expected missing behavior or defect. Fix setup or compilation mistakes before counting the failure as evidence. If the test already passes, check whether the behavior exists or the test misses the defect. Do not change the assertion just to force a failure.
3. Implement the smallest coherent change that makes the test pass. Run the test again.
4. Refactor related code when it improves the change. Preserve behavior and keep the tests passing. Avoid unrelated cleanup and abstractions that have no current use.
5. Repeat for the next required behavior, then run affected integration and regression checks and required project checks.

Use the project's mocking conventions to isolate external systems and nondeterministic behavior. Avoid assertions about the order and shape of internal calls unless that interaction is the contract. Database assertions are appropriate when persistence is the observable contract. Keep tests deterministic. Cover errors, authorization boundaries, or ordering when the change requires them.

## Evidence and handoff

When working within a ticket, use that ticket's execution record. Record the behavior or acceptance ID, repository-scoped test location, refactors and remaining gaps. For each repository, record the failing and passing command outcomes and identify the snapshot for each run. Do not duplicate an implementation log.

When invoked on its own, use the configured work-artifact location or a small record at `docs/work/<change>/tdd.md`. Include the input requirements, portable ownership metadata, each repository's starting and current identities, changed files, checks and next action. Resolve that location under the artifact owner. A package reuses its repository record. A ticket-based invocation never creates a second TDD artifact.

If the test environment is unavailable, report the blocker and which checks you could not run. Do not claim a verified failing-then-passing test cycle from inspection alone. Save progress and state what must change before work can resume. If you wrote code before the test, record it as regression coverage. Do not claim test-first evidence after the fact.

This method does not replace independent review or mark a ticket done. Do not automatically invoke review, spawn agents, commit, publish or deploy. Explicit user instructions take precedence over these defaults.
