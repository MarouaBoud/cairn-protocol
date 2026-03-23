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

## Tracks / Prizes (TBD)
- Protocol Labs — **Agents With Receipts (ERC‑8004)**
- Protocol Labs — **Let the Agent Cook**
- MetaMask Delegations (optional)

## Onchain Proof (TBD)
- ERC‑8004 identity tx: **TBD**
- Task submission tx: **TBD**
- Checkpoint commit tx(s): **TBD**
- Settlement tx: **TBD**

## Links (TBD)
- Repo: https://github.com/MarouaBoud/cairn-protocol
- Demo (Vercel): **TBD**
- Contract (Base Sepolia): **TBD**
- Agent Logs: **TBD**
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
