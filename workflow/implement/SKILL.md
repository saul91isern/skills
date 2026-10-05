---
name: implement
description: Implement one ticket or bounded change, verify behavior and save an execution record for resuming work and independent review. Use when explicitly asked to implement a work item or fix findings from its review.
---

# Implement

Complete one bounded work item and leave a reviewable result. Do not automatically advance to another ticket or invoke review.

## Resolve scope first

Identify who owns the authoritative work artifact, which repositories will run the work and who owns the relevant domains. Resolve each separately. Use the artifact owner's examples for service-local and app-owned inputs. An explicit artifact owner takes precedence over the launch directory. Read the owner's workflow, tracker and context configuration, along with guidance in the execution repositories. Keep repository-scoped paths in the saved handoff.

For each repository, identify the baseline, available dependencies, working-tree identity and required checks.

## Establish the work item

Accept a full ticket reference or explicit requirements for a bounded change. Follow the artifact owner's guidance and the guidance in the execution repositories. Follow their instructions for reading tracker records, domain documentation and architecture decision records. Read the ticket, relevant specification sections and dependency evidence. Resolve ambiguous ticket numbers. If the requirements exist only in the conversation, save a small ticket in the configured location or at `docs/work/<feature>/tickets/01-<name>.md` before implementing.

Verify every selected execution repository. Record its branch and worktree, full base and HEAD SHAs, staged and unstaged changes, and relevant untracked files. Record each starting state in the authoritative ticket, even when an app owns that ticket. Packages use the record for their containing repository. Record the exact dependencies, versions and availability in the consuming checkout. Preserve unrelated work. Do not reset, stash, switch branches or commit as an implicit implementation step. Do not run concurrent writers in the same checkout.

Check that dependency outputs are available and that the requirements still match the current code. If you find a material conflict, record the blocker and ask the specific question needed to resolve it. Continue other authorized work that does not depend on the answer. Do not redefine acceptance criteria to match the implementation. Mark the ticket `in-progress` only when work can proceed.

## Build and verify

Use the ticket's test strategy and project conventions. Consult the available `tdd` skill for changes to meaningful behavior. This does not start a separate phase. If the skill is unavailable, reproduce one behavior failure, implement the smallest coherent change and verify it. Refactor while the tests pass. Record reasonable exceptions, such as documentation-only changes or refactors that preserve behavior and already have test coverage.

Implement the stated outcome, including agreed failure behavior and exclusions. Read only the context needed for the ticket. Reuse project-owned helpers and interfaces. Record discoveries that affect future work. Treat requirement changes as proposals until someone resolves them.

Run focused checks during work, then the ticket's required checks. Read the project's command definitions and prerequisites. Use evidence to distinguish pre-existing failures from regressions. An infrastructure failure does not count as a failing behavior test. Unavailable checks do not count as passes. Re-run affected checks after fixes. Do not repeat broad suites without a reason.

Update the execution record with:

- Keep separate starting and current records for each repository. Include the branch and worktree, base and target SHAs, working-tree identities and relevant pre-existing changes as defined by the evidence contract.
- List changed files and explain why they changed. Record the exact ticket scope and any deviations.
- Record each acceptance criterion's evidence, test location or remaining gap.
- For each repository, record commands, execution scope and root directory, inspected snapshot, outcomes and relevant failure details. Label checks as passed, failed or not run. Include combined verification where this ticket owns it.
- Record open blockers, remaining work and the next concrete action if work stops.
- Identify the review input for each repository. For committed work, record the immutable base SHA and head SHA. For uncommitted work, record the base SHA, scoped working-tree diff and relevant new-file list. Record enough identity to distinguish later edits. Save a scoped patch or file hashes when HEAD cannot identify the result.

Do not include secrets or large logs in artifacts. Store only evidence needed to reproduce or assess the result. Save a checkpoint before ending an incomplete session as well as after successful completion.

## Handoff and review fixes

When the work satisfies the acceptance criteria and required checks pass, mark the ticket `review`. Report its path and the exact change set to inspect. If a required check is unavailable, record `blocked` and state what must change before work can resume. The partial result may still be reviewed, but do not call it complete. Follow the project's configured statuses if they differ.

When explicitly invoked to address review findings, read the report and verify the findings before fixing them. In the ticket, record how you propose to resolve each addressed or disputed finding and the supporting evidence. Preserve the reviewer's report. Re-run affected checks and return to `review`. Link the previous report and state that the modified result needs another review. Do not mark your own unreviewed fixes done.

Do not automatically invoke another phase, spawn workers, commit, publish or deploy. Follow any explicit authorization already provided for those actions. Report what changed, which checks ran and what remains unresolved. Include the information needed to reproduce the checks and review the result. If saving fails, report the unsaved progress. Do not claim you saved it.
