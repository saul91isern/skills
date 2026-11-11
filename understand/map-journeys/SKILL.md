---
name: map-journeys
description: Scope, prioritize and map the user journeys of an app across its frontend and microservices, including the side effects each journey must produce. Use when deciding what to cover with end-to-end tests. Produces journeys.yaml for the integration test repo, plus findings.md for issues found along the way.
---

# Map journeys

Produce a prioritized map of the journeys this app supports, the services each one touches,
and the side effects that prove it worked. The integration repo uses the map to write E2E
tests in priority order.

## 1. Find candidate journeys

Explore the code first, then interview the user about what you found.

Spawn one read-only `Explore` agent per surface, in parallel. Build the frontend, backend
and async prompts from `references/surface-prompt.md`, filling in the surface. The surfaces
are:
- frontend: navigation, routes and pages, and the role that gates each one
- backend: each service's HTTP API and its permission checks
- async: consumers, scheduled jobs and events that cross services
- harness: what the integration repo can run today. This explorer reads config and code at
  the default branch's committed ref, says which ref it read, doesn't start the harness,
  and reports:
  - the services and external dependencies the harness starts, and which build of each
  - what it stubs or leaves out
  - the seed data, users and roles available to tests
  - how tests authenticate and how they wait for async results
  - the test tools and the command that runs them
  - the capabilities its tests already exercise end to end, with the test file

While the agents run, read the glossaries, ADRs, specs and CHANGELOGs yourself.

Group the returned capabilities into candidate journeys, each written as a user goal. For
each candidate, record its evidence, its actors and whether tests already cover it. Keep
capabilities that fit no journey in an "unclustered" list.

Call the `grilling` skill on the draft. Ask what the app is for and who relies on it. For
each candidate, ask whether to keep, merge, split or drop it, and confirm its actors. Leave
implementation detail to step 2.

Step 1 produces:
- candidate journeys, each written as a user goal
- the domain vocabulary, including every actor named in a goal

Keep journeys coarse. Variants belong inside a journey.

## 2. Trace each journey

Spawn one read-only `Explore` agent per journey, in parallel, for a thorough exploration. 
Build each prompt from `references/trace-prompt.md`, filling in the journey
with its goal, actors and entry points from step 1.

Each agent traces its journey and reports:
- services in call order, external dependencies, and async hops
- **success:** the end state that proves the journey worked
- **side effects:** for every write or async hop, what should happen, where to observe it,
  and how long to wait. Prefer the read path a user would hit over reading a store directly.
  Mark each one that finishes after the response as `async`, `scheduled` or `external`.
- **variants:** failure paths, plus update, delete and permission cases. Stale data
  often shows up here.

Suspected issues go to `findings.md`. Unclear items become `unknown` in the map and get one
line each in `findings.md`.

Open the cited `file:line` for each side effect before you record it. A wrong trace reads
the same as a right one.

## 3. Check the harness

Compare the harness inventory from step 1 with each journey from step 2. Mark each side
effect with one line saying what is missing if it isn't `ready`:
- `ready`: every service on its path runs and a test can reach its observation point
- `partial`: a test can check it only in part, or the harness runs a build older than the
  traced code. For example, a test can see the effect but can't wait for it, or can check it
  for one role only.
- `blocked`: a service on its path doesn't run, or no test can reach the observation point

Mark each journey `ready` when its success state and all its side effects are ready,
`blocked` when its main path can't run, and `partial` otherwise.

## 4. Gather evidence

For the code each journey touches:
- **Usage:** analytics, API traffic or logs. If there are none, write "no data". Label user
  estimates as estimates.
- **Churn:** commits in a stated window.
- **Breakage:** fix commits, reverts and CHANGELOG "Fixed" entries.

Summarize as counts. Don't fill gaps with guesses. Count on the same committed ref used for
tracing, and state the ref and window once in `findings.md`.

## 5. Score and tier

Rate Impact, Usage and Risk from 1 to 5. Score each journey as Impact × (Usage + Risk).
The score sets the order. Tiers are a judgment call:
- **P1:** a failure would trigger a rollback or an incident
- **P2:** important, but a failure wouldn't stop the business
- **P3:** low stakes. API or contract tests are usually enough

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
  actors: [<role>, ...]
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
      status: ready
    - what: relations reindexed
      observe: td-lm
      kind: async
      within: unknown
      status: blocked   # td-lm not running
  variants:
    - update, then search immediately and again after the refresh
```
