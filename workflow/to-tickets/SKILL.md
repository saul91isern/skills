---
name: to-tickets
description: Split a specification or settled requirements into tickets that can be verified independently. Include dependencies, relevant context and acceptance criteria coverage. Use when explicitly asked to prepare work for separate implementation sessions.
---

# To tickets

Write the smallest useful set of tickets that separate sessions can implement and verify. Do not implement them or launch workers.

## Resolve scope first

Identify the authoritative artifact owner, the repositories where implementation will happen and the relevant domain owners separately. For a spec or settled requirements, use the owner's service-local and app-owned examples. An explicit artifact owner takes precedence over the launch directory. Read the owner's workflow, tracker and context configuration. Read local guidance in the implementation repositories. Save paths with their owning app or repository in the handoff.

For each repository, identify its baseline, available dependencies, working-tree identity and checks.

## Gather context

Accept a spec path or settled requirements. Use the artifact owner's workflow and tracker configuration. Read each implementation repository's local guidance, domain contracts and architecture decision records, or ADRs. Read the complete input and any existing tickets. Resolve ambiguous references before writing. Inspect relevant code and tests to identify boundaries, existing patterns and dependencies. Do not treat a spec's status alone as proof that requirements are settled.

Follow the project's configured artifact locations. Without configuration, use one file per ticket at `docs/work/<feature>/tickets/<NN>-<name>.md` under the selected artifact owner. Keep tickets for an app-owned spec in the same app. Link existing service tickets as dependencies instead of copying their status. Prefer one implementation repository per ticket. Include multiple repositories only when the outcome requires them. Preserve the source requirements in the feature's `spec.md` when they exist only in the conversation. Do not invent missing requirements that affect the work or require a separate extensive spec for a small change.

## Split and write

Prefer a ticket that delivers one observable behavior across the required layers. Do not force UI work into a library change or create one ticket per layer. A refactor, migration, or investigation with a defined scope can have its own ticket when its outcome is verifiable. For incompatible changes, use an expand, migrate, and contract sequence when needed. State compatibility and release dependencies.

Size each ticket for one new session, including investigation, implementation, and verification. Split work according to its outcome and uncertainty rather than line count. Leave ordinary implementation choices to the implementer. Do not prepare refactors for hypothetical future work or split work without a practical reason. Use one ticket when the whole change fits.

Declare only real dependencies. Check that the dependency graph has no cycles, missing references or self-dependencies. A dependency's required output must be available in the worker's checkout. A file marked done is not enough. Assign each in-scope acceptance criterion to a ticket. For a shared criterion, state each ticket's contribution and who verifies the combined outcome. Include integration verification in the last relevant ticket. Do not assume independent tests prove the whole feature.

Use the project's ticket template or these fields:

- Identity: include the feature-qualified ID, title, artifact owner and artifact path, with an explicit app-relative or repository-relative base. List implementation repositories, relevant domain and package owners, status and date. For each repository, record the inspected revision and evidence of local changes. Include the source spec's address and revision with its owning app or repository, or another stable source reference. Name target packages and their containing repositories.
- Outcome and exclusions: state the observable result and scope boundaries.
- Acceptance: keep stable local criterion IDs linked to spec `AC-N` IDs. Record exact constraints and the observation that would disprove each criterion. Distinguish new behavior from preserved invariants.
- Dependencies: reference authoritative tickets and their owning apps or repositories. Record required outputs, versions or contracts, and their availability in each consuming checkout.
- Required reading: include relevant spec sections, decisions and ADRs, and a short list of code and test entry points. Explain why each entry point matters. Describe the intent so the implementer does not need the planning chat. Do not paste the entire spec into every ticket.
- Verification: list focused checks, expected observations, required completion checks and known prerequisites. Mark commands as planned.
- Execution and handoff: start with execution marked as not started. For each implementation repository, plan the baseline, target, working-tree identity and checks separately, as the evidence contract requires. Later, record progress, actual evidence, remaining work and a review reference with its owning app or repository here.

Use project statuses when configured. Otherwise, use `draft` when unresolved requirements affect the work. Use `blocked` when prerequisite work is unavailable, and state what must happen before work can resume. Use `ready` when requirements are settled and dependencies are available. Do not mark every generated ticket ready automatically.

On rerun, preserve IDs, execution evidence and review history. Reconcile existing tickets instead of duplicating them. Do not silently rewrite in-progress or completed work. Record obsolete scope and references to replacements. Identify affected work that needs reconciliation.

## Finish

Read the saved tickets. Check acceptance criteria coverage, dependency references, scope boundaries, and whether a new session can understand each ticket. Save unresolved portions as drafts while completing independent ready work. Ask only questions needed to resolve ambiguity that affects the work. Do not require another approval round to split work into tickets when the user already authorized it.

Report ticket paths, outcomes, dependencies and which tickets are ready. Identify the exact input for the next phase without starting implementation. Writing local tickets is part of this phase. External publication requires explicit authorization and configured tracker behavior. Do not infer authorization from a tracker URL. Do not commit, spawn agents, or change application code. Explicit user instructions take precedence over these defaults.
