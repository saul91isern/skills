---
name: clarify
disable-model-invocation: true
description: Investigate a proposed change and discuss the decisions that repository evidence cannot settle. Record decisions and open questions in a draft spec that another session can resume. Use when asked to explore an idea or resume clarification before specification.
---

# Clarify

Turn an idea into a record of intent, decisions, and unresolved questions. This phase produces a draft spec. It does not implement the change or start a later phase.

## Locate context

Accept an idea, an existing draft path, or the current conversation. If no topic is available, ask what change to explore.

Read the applicable `AGENTS.md` guidance. Follow its links to workflow conventions, domain documentation, architectural decisions, and the configured tracker. If the repository has no configured workflow, use `docs/work/<feature>/spec.md` under its root. Use the user's output path when they provide one. Do not require a tracker or another skill.

Inspect relevant code and tests before asking questions that the repository can answer. Check for an existing draft before creating one. Read the full draft before updating it. Preserve unrelated content and requirements for later phases. When resuming, follow the handoff and recheck evidence that may have changed.

## Investigate and discuss

Model material questions as a dependency tree or graph. A question is ready once its prerequisite facts and decisions are settled. The ready questions form the current frontier. Use this model to order the discussion, not to explore every possible design branch. A question is material if its answer would change observable behavior, scope, exclusions, compatibility, persisted data, permissions, failure behavior, or acceptance criteria.

1. State the intended outcome. Separate observed behavior from desired behavior. Cite a few useful code and test paths. Record the inspected revision and relevant uncommitted changes.
2. Create the draft as soon as the topic is clear. Use the project's template when available; otherwise use the structure below.
3. Ask one focused question or a small related group from the ready frontier. Include a recommendation and its consequences. Defer questions that depend on unresolved facts or decisions. Do not ask the user to reason from assumptions. Avoid generic questionnaires, an overwhelming list of frontier questions, and repeated confirmation of settled decisions.
4. Save each resolved answer before the next round. Complete any independent investigation that remains useful, then recompute the frontier. Preserve exact limits, defaults, ordering, exclusions, and rationale. Mark recommendations and assumptions as proposals until the user accepts them. Silence is not acceptance. Record unresolved questions and continue independent investigation while waiting.
5. Challenge contradictions with evidence. Existing code describes current behavior, not automatic approval of future behavior. Briefly record important rejected alternatives. Mark replaced decisions superseded with a reason rather than silently erasing them.

When the change affects domain behavior, inspect the established vocabulary and bounded-context documentation. Point out conflicting or overloaded terms and propose precise alternatives. Use concrete scenarios to clarify relationships, ownership, lifecycle, and invariants. Skip domain modeling when it adds no useful distinction.

Keep feature decisions in the draft. This phase may create or update only the feature's `spec.md`. Do not create or modify ADRs, domain documentation, or other project files. If clarification identifies reusable vocabulary, an invariant, or a long-term technical trade-off that may belong in an authoritative document, record a documentation candidate. Give it a stable `LANG-N` or `ADR-N` ID. Include the proposed content, destination, rationale, source decisions, and the status `candidate`. It remains non-authoritative until someone explicitly promotes it in a later phase.

## Fallback draft structure

- Title and metadata: target repository, spec status `draft`, clarification status `in-progress` or `complete`, update date, inspected revision, and relevant local changes.
- Problem and outcome: current behavior, desired behavior, and why the change matters.
- Scope and exclusions.
- Observations: verified current behavior, terminology and relevant contradictions. Repository evidence establishes what exists, not what is desired.
- Confirmed decisions: stable `D-N` IDs, precise desired behavior, rationale, and authority. The authority is either a direct user requirement or a resolved answer. Include supporting evidence where useful.
- Proposals and questions: stable `Q-N` IDs, the assumption or recommendation, its consequence, whether it blocks clarification, and what would resolve it. If dependencies affect ordering or resumption, record the relevant `D-N`, `Q-N`, or investigation reference. Add a status such as `blocked` or `ready`. Omit this metadata when it adds no value. Keep a brief record of resolved questions with their decision references.
- Documentation candidates: optional `LANG-N` and `ADR-N` entries with the proposed content, destination, reason it should outlast the feature, source decisions, and status `candidate`.
- Verification observations: observable examples of success and failure, plus useful existing test boundaries.
- Required reading: a short list of relevant code, tests, and documentation. State why each reference matters.
- Handoff: remaining work and next concrete action.

Keep the draft concise. Omit empty optional sections; explicitly record unknowns that affect progress. Summarize decisions, not the chat transcript. Do not invent requirements to fill the template.

## Finish or resume

Save the current questions and next action before ending a round or session. If saving fails, report the failure and unsaved material; do not claim persistence succeeded.

Clarification is complete when the outcome, scope, exclusions, and material behavior decisions are explicit. The draft must also identify useful verification observations and contain no unresolved material question or assumption. Do not continue only to exhaust non-material branches. Routine implementation choices may remain open if the draft labels them as such. Mark clarification `complete` and leave the spec `draft`. Otherwise, keep clarification `in-progress` and record the condition for resuming.

Read the saved file and check that another session can continue without this conversation. End with the artifact path, a short decision summary, outstanding questions, and the next phase or required input. Suggest a specific next skill only if it is available. Do not start another phase, spawn agents, commit, or publish external issues unless the user asks. Explicit user instructions take precedence over these defaults.
