---
name: map-journeys
description: Scope, prioritize and map the user journeys of an app across its frontend and microservices. Use when deciding what to cover with end-to-end tests. Produces a prioritized journey map for the integration test repo.
---

# Map Journeys

Produce a prioritized map of the journeys this app supports and the services each one touches. The integration repo uses this map to write E2E tests in priority order.

## 1. Find candidate journeys

Run `/grill-with-docs` to learn what the app is for and who relies on it. Come out with:
- candidate journeys, each written as a user goal ("Customer places an order and receives confirmation")
- the domain vocabulary used to name them

Keep journeys coarse. Variants such as a declined card or an out-of-stock item belong inside a journey, not as separate journeys.

## 2. Map journeys to services

Use `/how` to trace each journey through the system. For each journey, record:
- the services it touches, in call order
- external dependencies
- asynchronous hops (queues, events)

## 3. Gather evidence

Look for signals of use and risk in the code each journey touches:
- **Usage:** analytics, API traffic or logs, if the user can provide them. If they can't, record "no data".
- **Churn:** how often that code changes in git history.
- **Breakage:** fix commits, reverts and CHANGELOG "Fixed" entries.

If a signal is missing, say so. Don't fill the gap with a guess.

## 4. Score and tier

Rate each journey from 1 to 5:
- **Impact:** how bad a failure would be (revenue, blocked users, data, compliance)
- **Usage:** how often the journey is used
- **Risk:** churn, breakage history and the number of services involved

Score = Impact × (Usage + Risk). Use the score to order the journeys. Then assign tiers by judgment:
- **P1:** a failure would trigger a rollback or an incident
- **P2:** important, but a failure wouldn't stop the business
- **P3:** low stakes; API-level or contract tests are probably enough

Show the ranked table to the user and adjust it based on their feedback before writing the map.

## Output

Write `journeys.md` to the integration repo, sorted by score. If the file already exists, update it instead of replacing it.

    ## <Journey name> — P1 (score 45)
    Goal: <user goal>
    Services: Frontend → Cart → Orders → Payments → Notifications
    External: Stripe, email provider
    Async: OrderPlaced event → Notifications
    Evidence: <usage / churn / breakage, or "no data">
    Variants: <failure and edge cases worth testing>
