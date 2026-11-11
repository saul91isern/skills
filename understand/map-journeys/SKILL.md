---
name: map-journeys
description: Scope, prioritize and map the user journeys of an app across its frontend and microservices, including the side effects each journey must produce. Use when deciding what to cover with end-to-end tests. Produces journeys.yaml for the integration test repo, plus findings.md for issues found along the way.
---

# Map journeys

Produce a prioritized map of the journeys this app supports, the services each one touches,
and the side effects that prove it worked. The integration repo uses the map to write E2E
tests in priority order.

## 1. Find candidate journeys

Run `/grill-with-docs` to learn what the app is for and who relies on it. Come out with:
- candidate journeys, each written as a user goal
- the domain vocabulary, including every actor named in a goal

Keep journeys coarse. Variants belong inside a journey.

## 2. Trace each journey

Use `/how` to follow each journey through the system. Record:
- services in call order, external dependencies, and async hops
- **success:** the end state that proves the journey worked
- **side effects:** for every write or async hop, what should happen, where to observe it,
  and how long to wait. Prefer the read path a user would hit over reading a store directly.
  Mark each one as `async`, `scheduled` or `external`.
- **variants:** failure paths, plus update, delete and permission cases. Stale data
  often shows up here.

## 3. Check the harness

Read what the integration repo can run today. Mark each journey and each side effect as
`ready`, `partial` or `blocked`, with one line saying what is missing.

## 4. Gather evidence

For the code each journey touches:
- **Usage:** analytics, API traffic or logs. If there are none, write "no data". Label user
  estimates as estimates.
- **Churn:** commits in a stated window.
- **Breakage:** fix commits, reverts and CHANGELOG "Fixed" entries.

Summarize as counts. Don't fill gaps with guesses.

## 5. Score and tier

Rate Impact, Usage and Risk from 1 to 5. Score each journey as Impact × (Usage + Risk).
The score sets the order. Tiers are a judgment call:
- **P1:** a failure would trigger a rollback or an incident
- **P2:** important, but a failure wouldn't stop the business
- **P3:** low stakes; API or contract tests are probably enough

Give a one-line reason when equal scores land in different tiers. Show the ranked list to the
user and adjust it before writing.

## Keep the map clean

Write anything that isn't map content to `findings.md`. That includes suspected bugs,
security issues, documentation gaps, merge and branch notes, and method notes. Tell the user
about security issues right away. Keep absolute local paths, commit hashes and branch
history out of the map.

## Output

Write `journeys.yaml` to the integration repo, sorted by score. If it exists, update it and
keep existing `id`s stable, because tests reference them.

```yaml
- id: concept-publish
  name: <journey name>
  tier: P1
  score: 45
  goal: <user goal>
  services: [td-web, kong, td-bg, td-audit]
  external: [postgres, elasticsearch, redis]
  status: partial   # <what the harness is missing>
  evidence: "Usage 5 (estimate). Churn <n> commits. Breakage <n> fixes, <n> CHANGELOG fixes."
  success:
    - concept version is published
  side_effects:
    - what: concept is searchable
      observe: POST /api/business_concept_versions/search
      kind: async
      within: 15s
      harness: ready
    - what: relations reindexed
      observe: td-lm
      kind: async
      harness: blocked   # td-lm not running
  variants:
    - update, then search immediately and again after the refresh
```
