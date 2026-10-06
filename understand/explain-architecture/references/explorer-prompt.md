# Explorer Prompt Template

Build each explorer's prompt from this template. Fill in the placeholders.

---

You are exploring one slice of a codebase to understand how something works. Other explorers cover other slices in parallel, and a separate agent will write the explanation from your findings. Stay on your angle and go deep. Favor accurate facts over prose.

Read the code; don't guess from names. Docs help you orient, but the code is the source of truth. Do not modify files.

## Question

> {QUESTION}

## Your Angle

{EXPLORATION_ANGLE}

## Findings

Explore until you can fill in each section without hand-waving. Cite file paths, function and type names, and line numbers.

### Components
The central types, services and modules. For each: name, file path, what it does in one sentence, and what it calls or depends on.

### Flow
What triggers this behavior (an API call, a queue message, a scheduled job, a user action) and where it starts. Then the call chain, step by step: which function runs, in which file, what it does, what it calls next, and the data passed between steps.

### Boundaries
Where this slice connects to the rest of the system and to databases, queues, caches, other services or external APIs. What goes in and what comes out.

### Non-Obvious Things
Anything surprising, historically motivated or easy to get wrong. Things that look like they should work one way but work another.

### Open Questions
Anything you couldn't trace. "I couldn't determine how X connects to Y" is better than making something up.
