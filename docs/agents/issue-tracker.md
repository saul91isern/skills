# Issue tracker

This pilot uses local Markdown files. No external tracker is configured.

- Specs: `docs/work/<feature>/spec.md`.
- Tickets: `docs/work/<feature>/tickets/<NN>-<name>.md`.
- Pass full repository-relative paths to fresh sessions; bare ticket numbers are ambiguous across features.
- Local files are authoritative for this workflow. User-supplied external issue links are references; do not infer a tracker from branch names or CI configuration.
- Status vocabulary: [workflow statuses](workflow.md#status-and-triage-vocabulary).

Clarification creates or updates a draft spec; specification refines that same file, dispositions documentation candidates and can mark it `specified`. Optional decision promotion updates configured authoritative documentation and records links in the same spec. None of these phases creates tickets. External issue creation, synchronization and closure are not configured.

`to-tickets` creates local ticket files. `implement` updates their progress and verification evidence. `review-ticket` persists its report and updates the local ticket review reference/status. Preserve history; do not synchronize statuses to external systems automatically.
