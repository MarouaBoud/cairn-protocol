# CAIRN

## Agent Failure and Recovery Protocol

> **One line:** CAIRN turns every agent failure into a lesson every other agent inherits — enforced by escrow, validated by attestation, owned by no one.
>
> **Three words:** Agents learn together.

---

## The Cairn Metaphor

A cairn is a stack of stones left by travelers to mark the path — so the next traveler knows where to go, and where not to. Every agent failure leaves a cairn. Every future agent reads it.

Travelers in wilderness stack stones — cairns — to mark where they have been, which paths are safe, and which lead nowhere. Each cairn is left by one traveler but read by every traveler who comes after. No traveler owns the cairn network. Every traveler benefits from it.

CAIRN applies this to agents. Every failure leaves a cairn — an execution record that marks this exact task type, this exact failure mode, this exact cost. Every future agent reads the cairns before setting out. The ecosystem navigates by accumulated failure intelligence, not blind optimism.

---

## What is CAIRN?

**CAIRN is a standardized agent failure and recovery protocol.**

It defines the exact sequence of events that must occur when an agent fails mid-task — from detection, through classification, through fallback assignment, through settlement — without requiring any human intervention and without requiring trust between agents.

### The Protocol in One Paragraph

An operator initiates a task with a budget, deadline, and task type. Before the task starts, CAIRN queries the execution intelligence layer for known failure patterns on this task type and recommends the best-fit agent. The agent runs. It emits liveness signals. It writes checkpoints after each subtask. If it fails — for any reason — CAIRN detects it automatically, classifies the failure, computes a recovery score, and either assigns a fallback agent (who resumes from the last checkpoint) or routes to dispute. On resolution, escrow splits proportionally between the original and fallback agents based on verified work done. The execution record is written. The intelligence layer grows. The next agent inherits the lesson.

### Secondary Output: Execution Intelligence

As a byproduct of the recovery protocol running, CAIRN accumulates an **execution intelligence layer** — a shared, queryable record of every failure, every recovery, and every successful completion across the ecosystem.

This is what makes CAIRN compound in value over time. The knowledge graph grows automatically. The more agents integrate CAIRN, the richer the intelligence layer becomes. Agents query it before starting tasks. The ecosystem gets smarter from every failure.

**The knowledge graph is the byproduct. The recovery protocol is the core.**

---

## What CAIRN is NOT

- **Not a new agent framework.** CAIRN wraps any existing framework — LangGraph, Olas SDK, AgentKit, custom builds.
- **Not a knowledge graph product.** Bonfires (the visualization layer) is a window into the intelligence layer, not the protocol itself.
- **Not a centralized service.** Every state transition is enforced by the CAIRN state machine contract. No server. No admin key. No human required.
- **Not a replacement for ERC-8183 or ERC-8004.** CAIRN integrates and extends both. It is an ERC-8183 Hook and an ERC-8004 reputation writer.
- **Not optional infrastructure.** The escrow condition makes record-writing mandatory — agents cannot receive payment without completing the protocol.

---

## Documentation

| Document | Description |
|----------|-------------|
| [Whitepaper](./WHITEPAPER.md) | Why CAIRN exists — problem, philosophy, economics |
| [ERC Specification](./ERC-CAIRN.md) | Technical standard (EIP format) |
| [Security](./SECURITY.md) | Security model, attack vectors, mitigations |
| [Changelog](./CHANGELOG.md) | Version history |

### Technical Documentation

| Document | Description |
|----------|-------------|
| [Concepts](./docs/concepts.md) | Failure taxonomy, state machine, glossary |
| [Architecture](./docs/architecture.md) | System design, protocol flow diagrams |
| [Execution Intelligence](./docs/execution-intelligence.md) | Knowledge graph, queries, network effects |
| [Integration](./docs/integration.md) | Checkpoint protocol, fallback pool, guides |
| [Contracts](./docs/contracts.md) | Interfaces, schemas, component reference |
| [Standards](./docs/standards.md) | ERC-8183, ERC-8004, ERC-7710, Olas integration |

---

## Protocol Status

| Property | Value |
|----------|-------|
| Version | 1.0 |
| Status | Specification Complete |
| Target Network | Base |
| ERC Dependencies | ERC-8183, ERC-8004, ERC-7710 |

---

## Quick Links

- **Understand CAIRN:** [Whitepaper](./WHITEPAPER.md) → [Concepts](./docs/concepts.md)
- **Technical Spec:** [ERC-CAIRN](./ERC-CAIRN.md) → [Contracts](./docs/contracts.md)
- **Build with CAIRN:** [Integration Guide](./docs/integration.md)
- **Security:** [Security Model](./SECURITY.md)

---

## Standards Integration

CAIRN integrates with existing Ethereum standards rather than replacing them:

| Standard | Role in CAIRN |
|----------|---------------|
| **ERC-8183** | Job escrow + lifecycle hooks |
| **ERC-8004** | Agent identity + reputation signals |
| **ERC-7710** | Caveat-enforced delegation for fallback |
| **Olas Mech Marketplace** | Fallback agent pool |

---

## License

[CC0](./LICENSE) — Protocol specification is public domain.
