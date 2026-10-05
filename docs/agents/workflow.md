# Manual engineering workflow

This pilot runs in this repository. This document supplies service-specific locations and conventions for the shared workflow skills.

## Available phases

All seven skills are available. Invoke phases explicitly; use a fresh session for each implementation ticket and, preferably, for its review:

```text
$clarify <idea>
$clarify docs/work/<feature>/spec.md
$to-spec docs/work/<feature>/spec.md
$promote-decisions docs/work/<feature>/spec.md  # only when the specified spec has ready candidates
$to-tickets docs/work/<feature>/spec.md
$implement docs/work/<feature>/tickets/01-<name>.md
$code-review docs/work/<feature>/tickets/01-<name>.md
```

For Claude Code, use `/skill-name` in place of `$skill-name`.

`promote-decisions` is optional and explicit; skip it when the spec has no ready documentation candidates. `tdd` is supporting guidance during implementation and can also be invoked explicitly with `$tdd <bounded behavior or ticket path>`. It does not add a mandatory handoff. Each phase saves its result and identifies the next input without launching another phase. Invoke `implement` again with the ticket and review reference to address findings, then invoke `code-review` to verify the updated result.

## Project locations

- Tracker configuration: [issue-tracker.md](issue-tracker.md).
- Specs (draft and specified): `docs/work/<feature>/spec.md`.
- Tickets: `docs/work/<feature>/tickets/<NN>-<name>.md`.
- Reviews: `docs/work/<feature>/reviews/<NN>-<name>.md`.
- Domain vocabulary and invariants: root `CONTEXT.md`, created when needed.
- Architectural decisions: `docs/adr/`, created when needed.
- Service architecture and execution guidance: root `AGENTS.md`, `README.md`, and relevant code/configuration. Historical feature docs are context, not proof of current behavior.

Use each skill's fallback document structure: clarify creates a draft and to-spec refines the same file with acceptance criteria, verification coverage and dispositions for any documentation candidates. Promote only candidates marked ready in a specified spec, then link the authoritative result back into that spec. Keep one spec per feature, update it in place, and preserve precise constraints and superseded decision rationale. Use repository-relative evidence paths with the inspected revision and relevant local changes. The next session should need the artifact and its targeted references, not the earlier conversation.

AGENTS.md is currently local and ignored. Workflow documents are intended to be versioned in this service. This pilot leaves the existing ignore policy unchanged. The clarify skill can use its fallback locations without AGENTS.md.

## Status and triage vocabulary

Specs remain `draft` during clarification. `clarification: in-progress` means decisions or questions remain; `clarification: complete` means discovery can hand off to specification. It does not authorize implementation.

After specification, use spec status `specified` when material questions are resolved, acceptance criteria are observable and verification is described. Keep `draft` when material gaps remain. `specified` means ready for ticketing, not an executable ticket or evidence of implementation. Record changed requirements and flag affected existing tickets for reconciliation.

Ticket statuses are:

- `draft`: material requirements are unresolved.
- `ready`: scope is settled and dependency outputs are available in the target checkout.
- `in-progress`: implementation or review fixes are underway.
- `review`: implementation criteria and required checks pass; independent review is pending.
- `done`: the current delivered change has passed review, acceptance criteria and required checks, with no material findings remaining.
- `blocked`: a prerequisite or required check is unavailable; include the reason, previous stage and resume condition.

Ticketing writes initial states. Implementation owns progress and submits work for review. Review owns its report and updates the ticket's review reference/status. Fixes return through `in-progress` and `review`; the implementer does not approve its own fixes. A blocked partial result may be reviewed but cannot be marked done. Dependency completion must be checked against the actual checkout. No automatic scheduling, external synchronization, commits or deployment is configured.

## Specification conventions

Preserve decision IDs (`D-N`) and question IDs (`Q-N`) from clarification. Add stable acceptance-criterion IDs (`AC-N`) with observable outcomes and references to their source requirements. Preserve exact limits, exclusions and negative requirements. Describe verification for each criterion or related group; distinguish planned checks from checks actually run.

Use relevant existing ExUnit, Ecto sandbox and Mox patterns. Read the actual test setup and `ci/test.sh` before prescribing commands. `mix test` creates and migrates the test database, so record environment prerequisites or limitations where relevant. Specification does not require running the application suite.

The next handoff is the same `docs/work/<feature>/spec.md` file. Pass it to `promote-decisions` first only when a ready candidate is required before ticketing; otherwise pass it directly to `to-tickets`.

## Decision promotion conventions

Clarification records optional `LANG-N` glossary and `ADR-N` architectural-decision candidates in the feature spec without changing authoritative documentation. Specification assigns each candidate a disposition. `promote-decisions` may act only on `ready-for-promotion` and `merge-with-existing` candidates in a `specified` spec; it updates the configured domain documentation or ADR locations and records the resulting links in the spec.

Promotion is warranted when losing the rationale would create meaningful risk of rework, regression, incompatible change or repeated investigation. Signals include cross-context vocabulary or ownership, hard-to-reverse or surprising trade-offs, external constraints and deliberate deviations from the obvious design. Keep feature-local behavior in the spec. Authoritative entries must state their scope and status clearly enough that later agents can determine whether they still apply.

## Ticket and review conventions

Use the shared skills' fallback ticket and review structures. Qualify ticket IDs with the feature, use full artifact paths in handoffs, preserve `D-N`, `Q-N` and `AC-N` references, and identify real dependencies. Each ticket must carry its outcome, exclusions, targeted reading, verification plan and execution evidence. Include combined feature verification in the final relevant ticket. Do not require every ticket to touch every application layer.

Keep implementation progress and test evidence in the ticket. Keep reviewer findings in `docs/work/<feature>/reviews/<ticket-stem>.md`, with stable `R-N` IDs, dispositions and exact reviewed revision or patch identity. For example, ticket `tickets/01-add-filter.md` uses `reviews/01-add-filter.md`. Preserve historical findings and distinguish reviewer-run checks from implementation-reported evidence.

For standalone TDD, use `docs/work/<change>/tdd.md`; when a ticket exists, record evidence there instead. Standalone reviews may use `docs/work/<change>/reviews/<review-name>.md`.

## Implementation and verification

Apply the service's existing domain and library ownership rules.

Use focused tests while implementing. Find each execution repository's required completion checks through the [context map](../../CONTEXT-MAP.md) or [context](../../CONTEXT.md), and inspect their definitions and environment prerequisites before execution. Existing CI security and acceptance gates remain applicable before integration/release. For documentation or skill-only changes, validate the affected artifacts and references; an application test run is not required.

Record passed, failed and not-run checks separately. Missing infrastructure blocks completion when it prevents a required check; it is not evidence of a code regression or a pass. Capture the implementation baseline and relevant pre-existing changes. Review must include relevant new files and uncommitted changes, not only a HEAD diff. A review only applies to its recorded snapshot.
