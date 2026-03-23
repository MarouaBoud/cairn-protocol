# Synthesis Submission Draft — CAIRN Protocol (MVP)

> Draft for Devfolio submission. Fill in **TBD** items as we finalize.

## Project Title
CAIRN Protocol — Agent Failure & Recovery (MVP)

## One‑Line Summary
A protocol that turns agent failure into recoverable progress: checkpoints + automated fallback + fair escrow settlement.

## Problem
Agent workflows fail frequently. When they fail today, work is lost, escrow is stuck, and nobody learns. Operators lose time and money; agents lose reputation; the ecosystem repeats the same failures.

## Solution
CAIRN standardizes failure handling. Agents write checkpoints to IPFS, emit liveness heartbeats, and if they fail, a fallback agent resumes from the last checkpoint. Escrow is split proportionally by verified work, and the onchain record creates a verifiable receipt trail.

## What’s Live (MVP)
- **CairnTaskMVP.sol**: onchain state machine for task lifecycle, checkpoints, and settlement.
- **Checkpoint Store**: IPFS CIDs committed onchain.
- **Watcher**: permissionless liveness enforcement.
- **Agent Logs**: `agent.json` + `agent_log.json` for agent‑judged verification.

## Demo Flow
1) Operator submits a task with escrow.
2) Primary agent checkpoints after each subtask.
3) Failure injected (missed heartbeat).
4) Fallback agent resumes from last checkpoint.
5) Escrow settles with proportional split.

## Tracks / Prizes
- Protocol Labs — **Agents With Receipts (ERC‑8004)**
- **Let the Agent Cook**

---

## Live Demo

| Resource | URL |
|----------------|------------------------------------------------------------|
| Frontend | https://cairn-protocol-iona-78423aa1.vercel.app |
| Subgraph | https://thegraph.com/studio/subgraph/cairn |
| Query Endpoint | https://api.studio.thegraph.com/query/1744842/cairn/v1.0.0 |

---

## Onchain Artifacts (Base Sepolia — 84532)

| Contract | Address | Basescan |
|-----------------|--------------------------------------------|---------------------------------------------------------------------------------|
| CairnCore | 0xB65596B21d670b6C670106C3e3c7E5FFf8E3A640 | https://sepolia.basescan.org/address/0xB65596B21d670b6C670106C3e3c7E5FFf8E3A640 |
| CairnTaskMVP | 0x2eFd1De57BfF1Ea3E40b049F70bb58590Ea73417 | https://sepolia.basescan.org/address/0x2eFd1De57BfF1Ea3E40b049F70bb58590Ea73417 |
| CairnGovernance | 0x7A09567e0348889Cc14264bEcf08F8d72Dc6987f | https://sepolia.basescan.org/address/0x7A09567e0348889Cc14264bEcf08F8d72Dc6987f |
| RecoveryRouter | 0xE52703946cb44c12A6A38A41f638BA2D7197a84d | https://sepolia.basescan.org/address/0xE52703946cb44c12A6A38A41f638BA2D7197a84d |
| FallbackPool | 0x4dCeA24eaD4026987d97a205598c1Ee1CE1649B0 | https://sepolia.basescan.org/address/0x4dCeA24eaD4026987d97a205598c1Ee1CE1649B0 |
| ArbiterRegistry | 0xfb50F4F778F166ADd684E0eFe7aD5133CE34aE68 | https://sepolia.basescan.org/address/0xfb50F4F778F166ADd684E0eFe7aD5133CE34aE68 |

---

## Agent Metadata Files

| File | Path | Description |
|---------------------|--------------------------------|-------------------------------------------------|
| agent.json | .synthesis/agent.json | Agent identity, team structure, deployment info |
| agent_log.json | .synthesis/agent_log.json | Chronological build log (70+ entries) |
| CONVERSATION_LOG.md | .synthesis/CONVERSATION_LOG.md | Session summaries, decisions, blockers |

---

## Repository

| Property | Value |
|----------|----------------------------------------------|
| URL | https://github.com/MarouaBoud/cairn-protocol |
| License | MPL-2.0 |
| Status | Open Source |

---

## Test Coverage

| Metric | Value |
|-----------|--------|
| Tests | 302 |
| Coverage | 98.95% |
| SDK Tests | 126 |

---

## Track Requirements: Agents With Receipts (ERC‑8004)

| Requirement | Implementation |
|-------------------------|-----------------------------------------------------------------------------|
| Execution Records | Every task creates on‑chain record with checkpoints, heartbeats, settlement |
| Failure Classification | RecoveryRouter classifies: TIMEOUT, REVERTED, RESOURCE, LOGIC, UNKNOWN |
| Recovery Scoring | Computed recovery probability before fallback assignment |
| Settlement Receipts | Proportional escrow splits with on‑chain verification |
| Collective Intelligence | Bonfires integration writes failure patterns to knowledge graph |

---

## Track Requirements: Let the Agent Cook

| Requirement | Implementation |
|--------------------|------------------------------------------------------------------|
| Full Decision Loop | 4‑state machine: IDLE → RUNNING → FAILED/RECOVERING → RESOLVED |
| Structured Logs | agent_log.json with 70+ timestamped entries |
| Guardrails | Heartbeat timeouts, escrow limits, automatic recovery |
| Compute Budget | Gas optimization via Merkle checkpoint batching (89–99% savings) |

---

## Standards Integration

| Standard | Role |
|-----------|----------------------------------------------------|
| ERC‑8183 | Escrow lifecycle hooks — CAIRN intercepts failures |
| ERC‑8004 | Agent identity — CAIRN writes reputation signals |
| ERC‑7710 | Delegation — pre‑authorized fallback assignment |
| Olas Mech | Fallback pool — available agent services |

---

## Repository Structure

```
cairn-protocol/
├── contracts/   # Solidity (Foundry) - 302 tests, 98.95% coverage
├── sdk/         # Python SDK (CairnClient, CairnAgent, CheckpointStore)
├── frontend/    # Next.js 14 dashboard (Vercel)
├── pipeline/    # Off-chain event listener
├── subgraph/    # The Graph indexer
├── PRDs/        # Product requirements
├── docs/        # Technical documentation
└── .synthesis/  # Hackathon logs (agent.json, agent_log.json, CONVERSATION_LOG.md)
```

---

## Quick Copy Links

- Frontend: https://cairn-protocol-iona-78423aa1.vercel.app
- Repo: https://github.com/MarouaBoud/cairn-protocol
- CairnCore: 0xB65596B21d670b6C670106C3e3c7E5FFf8E3A640
- Chain: Base Sepolia (84532)
- Agent Logs: .synthesis/agent.json, .synthesis/agent_log.json
- Conv Log: .synthesis/CONVERSATION_LOG.md

## Links (TBD)
- Video: **TBD**

## Tech Stack
- Solidity (Foundry)
- Base Sepolia
- IPFS (Pinata)
- Next.js (demo UI)
- Python SDK (agent wrapper)

## Team
- Maroua (lead)
- Lagartha (build + execution)

## Compliance Notes
- No mocks for onchain actions (testnet only).
- Agent logs are machine‑readable for judge agents.
