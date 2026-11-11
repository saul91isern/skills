# Surface prompt template

Build each explorer's prompt from this template. Fill in the placeholders.

---

You are listing what users can do through one surface of an app. Other explorers cover other
surfaces in parallel, and a later step groups your findings into user journeys. Stay on your
surface. Report capabilities, not architecture.

Read the code and don't guess from names. Read the default branch's committed ref, not the
checked-out branch, and say which ref you read. Do not modify files. Do not run the app or
call any endpoint, because some calls write audit records.

## Surface

{SURFACE}

## Findings

Cite a file path and line number for every item.

### Capabilities

One entry per thing a user or system can do here. For each:
- **goal:** a verb phrase from the user's point of view, such as "publish a concept". Not a
  function name.
- **actor:** the role or system that does it, and the permission or role check that gates it
- **entry point:** the route, endpoint, consumer or job, with `file:line`
- **reads and writes:** what it creates, changes or deletes
- **hands off to:** the services, queues or scheduled jobs it triggers, if visible from here

Group capabilities that belong to the same user goal, but don't merge across actors.

### Permission gates

Roles, permissions or feature flags that change what a user sees or can do.

### Existing coverage

Tests that already exercise a capability end to end. Name the capability and the test file.

### Suspected issues

Behaviour that looks wrong, such as a missing permission check. Cite `file:line`.

### Unclear

Anything you couldn't trace. "I couldn't tell whether X triggers Y" is better than a guess.
