---
name: promote-decisions
disable-model-invocation: true
description: Move prepared domain context and architectural decision candidates from a specified feature spec into the authoritative documents named by each candidate. Use only when asked to promote decisions before ticketing or implementation.
---

# Promote decisions

Move settled documentation candidates into the project's authoritative documents. This optional phase updates domain context or ADRs and links each update to its source spec. It does not clarify requirements, change application code, or start a later phase.

## Resolve scope first

Identify the authoritative artifact's owner, the repositories where the work will run, and the relevant domain owners separately. Use the owner's service-local and app-owned examples for the inputs. An explicit artifact owner takes precedence over the launch directory. Read the owner's workflow, tracker, and context configuration. Read the guidance in the repositories where the work will run. Keep paths and their scopes in the saved handoff.

## Input and eligibility

Require an explicit path to the feature's `spec.md`. Read the full spec. Follow the artifact owner's guidance and the relevant context-map routes to find the authoritative owner named by each candidate. Read that owner's context rules, current domain documentation, and applicable ADRs before writing. The spec must have the status `specified`. Act only on `LANG-N` or `ADR-N` candidates with the disposition `ready-for-promotion` or `merge-with-existing`. Do not change candidates marked feature-local, deferred, rejected, or unresolved.

The user's invocation with a spec path authorizes the prepared documentation changes. Ask the user for direction if a candidate's target or scope is ambiguous, or if it conflicts with authoritative documentation. Stop if its source decision is no longer settled or if promotion would change behavior or requirements. Do not promote a candidate just to fill a documentation section.

## Promote candidates

1. Check the candidate against the current authoritative documents and repository behavior. Existing code shows current behavior. It does not authorize a change to the specified decision.
2. Use the project's established locations and formats. For vocabulary, responsibilities, ownership, lifecycle, and invariants, update the bounded context named by the candidate. Require an unambiguous authoritative `target` and a `path` relative to its scope. The spec's owner does not automatically own those contracts. Keep package decisions in the repository's ADR location and record their package scope. An unresolved policy for app ADRs blocks only app ADR promotion. Keep each definition precise and preserve valid meanings within each context. For ADRs, update or supersede an existing decision instead of creating a duplicate.
3. Create an ADR only if losing the rationale would risk rework, regression, an incompatible change, or repeated investigation. Record its status, scope, decision, and rationale. Include material alternatives, consequences, or external constraints when they help readers decide whether the ADR applies.
4. Preserve candidate IDs and source `D-N` references in the authoritative entry or its link back to the spec when the project format allows it. Do not copy unresolved assumptions into authoritative documentation.
5. Set the spec candidate's status to `promoted`. Add a Markdown link to the authoritative document with a path relative to the spec's location. Include an address that identifies the document's scope. Record which decision it supersedes or which decision supersedes it, if any. If a candidate is no longer eligible, leave it unpromoted and record the reason. Do not silently change its disposition.

Keep spec ownership metadata and candidate histories intact. Avoid broad rewrites. Preserve unrelated content, terminology, and decision history. This phase may modify only the supplied spec and the authoritative documents named by eligible candidates. Do not modify other documents just because configuration suggests them. If a required structural update affects a document outside the candidate's named targets, record the missing prepared target and pause that candidate. Migrating existing context is separate rollout work. It does not authorize promotion of new decisions. Do not create tickets, modify application code, commit, or publish externally.

## Finish

Read the changed documents and spec. Check that each promoted entry matches its settled source decision. Confirm that the entry states its scope and status where applicable. Check that the spec links to it. Report promoted, skipped, and blocked candidates separately. Include their paths and the reasons for any skipped or blocked candidate.

End with the updated spec path and the next eligible input. If every promotion required before ticketing succeeded, identify the same spec as the input the user must pass explicitly to the ticketing phase. Do not start another phase automatically. If a write fails, report the unsaved change and do not mark that candidate as promoted.
