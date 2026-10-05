---
name: code-review
description: Review a defined change set against requirements and repository invariants. Save findings with supporting evidence and record completion status. Use when explicitly asked to review a ticket, branch, working tree or fixes to a prior review.
---

# Code review

Review correctness and requirement coverage in one report, with findings ordered by severity. Prefer a fresh session. Do not claim independence when reviewing in the implementation conversation. Do not spawn additional reviewers or modify application code as part of review.

## Resolve scope first

Identify who owns the authoritative work artifact, which repositories will run the work and who owns the relevant domains. Resolve each separately. Use the artifact owner's examples for service-local and app-owned inputs. An explicit artifact owner takes precedence over the launch directory. Read the owner's workflow, tracker and context configuration, along with guidance in the execution repositories. Keep repository-scoped paths in the saved handoff.

For each repository, identify the baseline, available dependencies, working-tree identity and required checks.

## Define the review

Follow the artifact owner's guidance and the guidance in the execution repositories. Follow their instructions for reading tracker records, domain documentation and architecture decision records. Accept a ticket, an explicit diff or base reference, a branch, or a bounded working-tree scope. Read the ticket, relevant specification sections, implementation evidence and any supplied previous review. If requirements are unavailable, review the code and state that you could not verify acceptance.

Resolve the intended baseline and target separately for every execution repository before reviewing. Confirm the full repository list from the ticket or user input. An app has no shared Git base across its repositories. Package paths use the snapshot of their containing repository. Prefer the ticket's recorded baseline or an explicit user reference. Resolve references to immutable SHAs. Use the merge base for changes introduced by a branch when that matches the request. Do not use it instead of an explicitly requested commit range. If the scope is ambiguous, ask before attributing changes.

For committed work, inspect the diff between the pinned base and head. For uncommitted work, inspect tracked changes against the pinned baseline, staged and unstaged state, and all relevant untracked files. A diff that only compares commits misses uncommitted work. Account for pre-existing changes from the implementation record. Never attribute unrelated work to this ticket. Check new files before concluding that an empty tracked diff means there is nothing to review.

For each reviewed repository, record its scope, checkout and worktree, base and head SHAs, staged and unstaged identities, and relevant files. Include a scoped patch or content hashes for uncommitted files, including relevant new files. Check every identity again before finishing. If a repository changed, its affected snapshot needs another review before you can mark it complete. Review the new changes or state that the result applies only to the earlier snapshot. Do not mark a changed snapshot complete.

## Inspect

Trace changed behavior through callers, contracts, and tests. Check requirement coverage, correctness, security, performance regressions, meaningful duplication, and test gaps. Verify acceptance evidence yourself where practical. Distinguish commands you ran from results reported by someone else. Read surrounding code before claiming a bug. Respect repository invariants. Do not report general style or code-smell concerns without a concrete maintenance cost.

For each actionable finding, provide a stable `R-N` ID, severity, precise repository-scoped `path:line`, trigger, impact, supporting evidence and suggested correction. Rank findings by severity and merge duplicates. Separate confirmed findings from unresolved suspicions. Do not present speculation as a defect. When no findings remain, say so and identify residual testing risk.

When reviewing again, preserve prior findings and record whether each is open, resolved or dismissed. Give a reason for dismissals. Verify fixes and affected behavior yourself. An implementer's statement that a finding is fixed is not enough. Report new defects introduced by fixes. Do not keep proposing improvements without evidence of a defect.

## Save the review and conclude

Use the artifact owner's review location or `docs/work/<feature>/reviews/<ticket-stem>.md` under that owner. Keep an app-owned ticket's report with the app and link the scoped source ticket. Do not create a duplicate report or status in a repository because the session launched there. For a standalone review without a ticket, use an explicit output path or a descriptive review file under `docs/work/<change>/reviews/`. Preserve existing report history when reviewing again. Do not overwrite an unrelated review.

Include the following in the report:

- Portable ownership metadata and the scoped source input.
- Each repository's reviewed identity and scope.
- Requirements and guidance you consulted.
- Findings and their status.
- Acceptance coverage.
- Checks, their source and their outcomes.
- Limitations, verdict and next action.

Mark the review incomplete if you could not inspect a material part of the change.

For a local workflow ticket, update only its review reference and status. Default to `done` only when all of these conditions hold:

- You have reviewed the exact delivered change.
- The work satisfies the acceptance criteria.
- Required checks for every execution repository and required combined checks have passed.
- All named snapshots are current.
- No material finding remains.

Do not mark the ticket `done` if a repository or required check is missing. Use `in-progress` when fixes remain. Use `blocked` when a required prerequisite or check is unavailable. Record the reason and what must change before work can resume. Follow project status conventions. Completing a review does not authorize merging or deployment. Do not close external issues without explicit authorization.

End with findings first, ordered by severity, and link the report. If there are no findings, say so and identify any residual risk. Do not automatically implement fixes, invoke another phase, commit or publish. Explicit user instructions take precedence over these defaults. If saving fails, report the failure. Do not claim you saved the review or updated the ticket.
