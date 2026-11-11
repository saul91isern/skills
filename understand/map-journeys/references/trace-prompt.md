# Trace prompt template

Build each explorer's prompt from this template. Fill in the placeholder.

---

You are tracing one user journey through an app made of a frontend and several services.
Other explorers trace other journeys in parallel, and a later step turns your findings into
end-to-end test cases. Stay on your journey and follow it to the end, including everything it
triggers afterwards.

Read the code and don't guess from names. Read the default branch's committed ref, not the
checked-out branch, and say which ref you read. Do not modify files. Do not run the app or
call any endpoint, because some calls write audit records.

## Journey

{JOURNEY}

## Findings

Cite a file path and line number for every item.

### Services in call order

Each service the journey touches, in the order the calls happen, with the request or message
that reaches it. Name external dependencies, such as databases, search indexes, caches,
queues and third-party APIs, next to the service that uses them.

### Success

The end state that proves the journey worked, as a user or an API client would see it.

### Side effects

Every write and every async hop. For each:
- **what:** what should happen
- **observe:** where a test can see it. Prefer an endpoint a user would hit over reading a
  database or index directly. For events, name the stream and the event type.
- **kind:** `async` if the journey triggers it and it finishes later, `scheduled` if it runs
  on a timer, `external` if it happens in a system outside this app. A write that finishes
  before the response returns needs no kind.
- **within:** how long a test should wait. Take it from config and code, such as refresh
  intervals, schedules, retry delays and consumer polling. Write "unknown" if the code
  doesn't say.

### Variants

Failure paths, update and delete cases, and permission cases where a different role gets a
different result. For each, say what differs from the main path. Cases that leave stale data
behind, such as an index, cache or read model that isn't updated, matter most.

### Suspected issues

Behaviour that looks wrong, such as a missing permission check, an event nobody consumes, or a
write that skips the index or the audit trail. Cite `file:line`. Keep these out of Variants.

### Unclear

Anything you couldn't trace. "I couldn't tell whether X triggers Y" is better than a guess.
