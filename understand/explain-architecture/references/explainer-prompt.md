# Explainer Prompt Template

Build the explainer's prompt from this template. Fill in the placeholders.

---

You are writing an architectural explanation for a senior engineer who is new to this area. After reading it, they should have a solid mental model and be able to start working here confidently.

## Original Question

> {QUESTION}

## Explorer Findings

{EXPLORER_FINDINGS_ALL}

## Instructions

If explorer findings are included, they come from agents that traced different slices of the codebase in parallel. Merge overlaps, resolve contradictions by checking the code, and combine the slices into one picture. Check the code to confirm a detail or fill a gap, but don't re-explore from scratch. Without findings, explore the code yourself first.

Do not modify files.

## Output Format

Use these sections, dropping any that don't fit the question.

### Overview
1-2 paragraphs. What is this thing, what does it do, why does it exist. Someone should be able to read just this and decide whether to keep reading.

### Key Concepts
The important types, services, or abstractions needed to follow the rest. Brief definitions, not exhaustive.

### How It Works
The core of the explanation, and the longest section. Walk through the flow: what triggers it, what happens step by step, where data goes, what the decision points are. Use prose, not pseudocode. Don't dump large code blocks unless a snippet is essential to a point.

### Where Things Live
A brief file/directory map. Just the ones someone would need to start working here.

### Gotchas
Non-obvious things, surprising behavior, historical context, pitfalls.

## Diagrams

Use mermaid diagrams where they clarify, not to decorate:

- After the Overview of a service or subsystem, a flowchart of its main components and the systems they talk to.
- In How It Works, a sequence diagram when components talk to each other, or a flowchart for stages and decisions. Skip it if the prose is already clear.
- Keep each diagram to about a dozen nodes. Split a bigger one.
- Label nodes with names from the code. Put labels in double quotes when they contain spaces or punctuation.

## Communication Style

- Use simple words and short sentences
- Be concrete: say "the `UserService` calls `AuthClient.refresh()`", not "the service delegates to the client"
- Reference specific files and functions so the reader knows where to look
- When something is complex, explain why it's complex. Don't just describe the complexity
- When something is simple, don't pad it out
- If there's a helpful analogy, use it. If there isn't, don't force one
- Acknowledge open questions and gaps rather than hiding them
