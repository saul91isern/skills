---
name: explain-architecture
disable-model-invocation: true
description: Explain how a service, subsystem or flow works, from its components and runtime flow to where its code lives. Use when explicitly asked how something works, for a walkthrough before a change, or for placement, ownership and layering questions.
---

# Explain architecture

Explore the codebase to answer "how does X work?" questions. Produce architectural explanations at the level of a senior engineer onboarding onto a service or subsystem: enough to build a working mental model, not so much that it reads like annotated source code.

## Agents

The steps below use two agents:

- **Explorer**: read-only. Claude Code `architecture-explorer`, Codex `architecture_explorer`.
- **Explainer**: general purpose, does not modify files. Claude Code `architecture-explainer`, Codex `architecture_explainer`.

Both run on `claude-opus-5-5` at `xhigh` effort in Claude Code and on `gpt-6.1-sol` at `xhigh` in Codex. Their definitions set the model and effort, so do not pass a model or effort when you spawn them.

If an agent is not installed, use the closest built-in agent and tell the user:

- Claude Code: `Explore` for explorers and `general-purpose` for the explainer, with `model: opus`. The effort cannot be set per call, so it follows the session.
- Codex: the default agent with `model: gpt-6.1-sol` and `reasoning_effort: xhigh`.

## Step 1. Assess complexity

If the scope is ambiguous, state your interpretation and explore. The user can redirect.

- **Simple** (a single module, a small utility, a narrow question such as "how does function X work"): skip Step 2. One explainer explores and explains in a single pass.
- **Complex** (a service, a subsystem spanning multiple files or services, a cross-cutting feature, a full architectural overview): spawn parallel explorers first, then hand off to the explainer.

When in doubt, take the simple path.

## Step 2. Explore (complex questions only)

Split the question into 2 to 4 exploration angles, each a distinct slice of the system. For a service overview, typical angles are its entry points (API, consumers, jobs), its core domain logic, its data and storage, and its connections to other services.

Spawn one explorer per angle, all of them before waiting on any. Build each prompt from `references/explorer-prompt.md` with its angle filled in. Wait until every explorer has returned.

## Step 3. Explain

Spawn one explainer with a prompt built from `references/explainer-prompt.md`. Fill in every explorer's findings, or remove the findings section for a simple question.

## Step 4. Present

Present the explainer's output to the user. Light edits for clarity or context from the conversation are fine. Do not substantially rewrite it.

This skill only reads code. Do not modify files, commit or start another skill unless the user asks.
