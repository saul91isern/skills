---
name: to-spec
description: Turn clarified requirements or an existing draft into a concise specification with testable acceptance criteria and a verification approach. Use when explicitly asked to write or refine a spec before ticketing or implementation.
---

# To spec

Turn settled requirements into a specification that a new session can use. Preserve decisions. Do not expand the scope to fill a template or reopen answered questions.

## Resolve scope first

Identify the authoritative artifact owner, the repositories where implementation will happen and the relevant domain owners separately. For an existing draft or a new spec, use the owner's service-local and app-owned examples. An explicit artifact owner takes precedence over the launch directory. Read the owner's workflow, tracker and context configuration. Read local guidance in the implementation repositories. Save paths with their owning app or repository in the handoff.

## Input and context

Accept an existing draft path, supplied requirements, or the current conversation. Read the sources required by the artifact owner and implementation repositories, along with relevant domain references and architecture decision records, or ADRs. No other skill or external tracker is required.

Read the whole draft and update it in place. For a new spec, use the configured location or the scope contract's fallback in the selected artifact owner. Honor paths the user supplies. Check for related specs first. If the input could refer to several, resolve the ambiguity before changing one.

Inspect relevant code and tests to verify current behavior and constraints, and to identify where tests can check the behavior. Record the inspected revision and relevant uncommitted changes. Distinguish repository facts, historical documentation, user requirements and agent proposals. Do not rely on a draft's completion marker alone.

## Synthesize

1. Preserve the problem, desired outcome, scope, exclusions, confirmed decisions and important rejected alternatives. Keep decision and question IDs and their rationale. Mark replaced decisions as superseded and retain their history.
2. Turn requirements into observable acceptance criteria with stable `AC-N` IDs. Preserve exact defaults, limits, ordering, compatibility and negative requirements. Reference relevant decision IDs. Include failure cases where required by the agreed behavior. Do not invent policies or extra features.
3. Describe contracts and invariants precisely enough to implement. Use examples or a small schema when they are more precise than prose. Include code and test entry points to help readers find relevant code. Readers must verify them at the recorded revision. They are not mandatory implementation instructions.
4. Describe a suitable verification method for each criterion or related group. Prefer existing public boundaries and project test patterns. State what distinguishes success from failure. Choose test locations that follow project conventions without another approval round. Flag choices that would change the agreed contract or materially expand the scope.
5. Check that every in-scope requirement has a criterion or an explicit verification note. Check that every criterion follows from a requirement. Distinguish new behavior from invariants already true at the baseline. Refactors may preserve existing behavior. Do not demand a failing baseline test for those invariants.
6. Review documentation candidates without modifying authoritative documents. For each `LANG-N` or `ADR-N`, check that its source decision is settled and search for an existing definition or decision. Confirm its scope, including domain responsibilities and invariants where relevant. Assign one disposition: `ready-for-promotion`, `feature-local`, `merge-with-existing`, `deferred` or `rejected`. Record the target and reason. Require promotion before ticketing only when tickets need authoritative vocabulary, ownership boundaries or constraints for consistent interpretation.

Do not turn assumptions into requirements. Investigate questions that the code can answer. If the intended behavior is unclear or requirements conflict, save the useful parts of the spec as `draft`. Record the question that affects the spec and ask only what is needed. Continue work that does not depend on the answer without reopening settled decisions. Follow existing authorization. Do not require approval of the whole document as a formality.

## Spec structure

Use the configured project template. Otherwise retain or adapt these sections:

- Metadata: include the artifact owner and artifact path, with an explicit app-relative or repository-relative base. List implementation repositories, relevant domain and package owners, status and date. For each repository, record the inspected revision and evidence of local changes. Use status `draft` or `specified`. Preserve clarification metadata without inventing a prior clarification session.
- Problem and intended outcome: distinguish current behavior from the requested change.
- Scope and exclusions.
- Decisions and contracts: precise behavior, rationale, relevant `D-N` IDs and linked ADRs.
- Acceptance criteria: include each `AC-N` ID, an observable condition and result, and a requirement or decision reference.
- Verification approach: describe criterion coverage, test boundaries, useful existing tests and known environment limitations. Mark proposed commands as planned. Never present them as evidence of execution.
- Open questions and assumptions: retain `Q-N` IDs and their resolution or blocking consequence. Routine implementation choices can remain open if they do not alter acceptance criteria.
- Documentation candidates: retain candidate IDs, source decisions and their histories. Record the authoritative owner, target path and its owning app or repository, disposition and reason. For `merge-with-existing`, identify the existing document. For `ready-for-promotion`, state whether promotion is required before ticketing.
- Required reading and handoff: include relevant references, remaining work, the next phase and its input.

Keep sections proportional to the change. Do not require user stories for every requirement. Link to established domain definitions in their authoritative docs. Keep feature-specific decisions in the spec. Preserve unrelated user-authored content.

## Finish

Read the saved spec and compare it with the source draft or supplied requirements. Check exact constraints, exclusions, decision history, criterion coverage and references. A new session must be able to understand the intended behavior without the conversation.

Use the project's spec status convention. If there is none, use `specified` when questions that affect the spec are resolved, acceptance criteria are observable and the spec describes verification. Otherwise keep `draft` and state what must happen before work can resume. Record an unavailable test environment as a verification limitation. Do not treat it as evidence that tests passed. It need not block specification unless it prevents defining a credible verification approach.

If an existing specified spec changes materially, record the change and identify dependent tickets that need reconciliation. Do not silently rewrite their scope or statuses. The `specified` status means the spec is ready to break into tickets. It does not mean implementation is complete or authorize dispatching workers.

End with the saved path, a concise summary, unresolved questions or limitations, the next phase and its input. If ready documentation candidates require promotion before ticketing, identify that step. Otherwise hand off to ticketing and note any optional or deferred promotions. Only suggest a named skill if it is available. Do not automatically promote documentation, create tickets, launch another phase, spawn agents, change application code, commit or publish external issues. If saving fails, report the unsaved material and the failure. Do not claim that a handoff exists. Explicit user instructions take precedence over these defaults.
