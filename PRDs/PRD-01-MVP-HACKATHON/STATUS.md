# PRD-01: CAIRN MVP — Status Tracker

> Last Updated: 2026-03-17

## Current Phase

- [x] **Phase 0**: PRD Written & Approved
- [ ] **Phase 1**: Contract Development (Days 1-3)
- [ ] **Phase 2**: SDK Development (Days 4-5)
- [ ] **Phase 3**: Frontend & Demo (Days 6-8)
- [ ] **Phase 4**: Integration & Polish (Days 9-10)
- [ ] **Phase 5**: Submission

## Pipeline Checklist

| Stage | Status | Notes |
|-------|--------|-------|
| PRD Approved | ✅ | Ready for execution |
| Contract Complete | ⏳ | Not started |
| Contract Deployed | ⏳ | Blocked by contract |
| SDK Complete | ⏳ | Blocked by deployment |
| Frontend Complete | ⏳ | Blocked by SDK |
| E2E Tests Pass | ⏳ | Blocked by frontend |
| Demo Rehearsed | ⏳ | Blocked by E2E |
| Submitted | ⏳ | Target: March 23 |

## Active Teammates

| Teammate | Assigned To | Status | Current Task |
|----------|-------------|--------|--------------|
| Contract-Dev | TBD | 🔵 Ready | Task 1 |
| SDK-Dev | TBD | ⏸️ Waiting | Blocked by Contract |
| Frontend-Dev | TBD | ⏸️ Waiting | Blocked by SDK |
| Integration | TBD | ⏸️ Waiting | Blocked by All |

## Task Breakdown

### Phase 1: Contract Development

| # | Task | Owner | Status | Blocked By | Notes |
|---|------|-------|--------|------------|-------|
| 1 | Setup Foundry project | Contract-Dev | 🔵 Ready | — | |
| 2 | Implement state machine | Contract-Dev | ⏸️ | 1 | |
| 3 | Implement checkpoint storage | Contract-Dev | ⏸️ | 2 | |
| 4 | Implement heartbeat system | Contract-Dev | ⏸️ | 2 | |
| 5 | Implement settlement | Contract-Dev | ⏸️ | 2 | |
| 6 | Write unit tests | Contract-Dev | ⏸️ | 3,4,5 | Target: 95% coverage |
| 7 | Deploy to Base Sepolia | Contract-Dev | ⏸️ | 6 | |
| 8 | Verify on Basescan | Contract-Dev | ⏸️ | 7 | |

### Phase 2: SDK Development

| # | Task | Owner | Status | Blocked By | Notes |
|---|------|-------|--------|------------|-------|
| 9 | Setup Python package | SDK-Dev | ⏸️ | 8 | |
| 10 | Implement CheckpointStore | SDK-Dev | ⏸️ | 9 | |
| 11 | Implement CairnClient | SDK-Dev | ⏸️ | 10 | |
| 12 | Implement CairnAgent wrapper | SDK-Dev | ⏸️ | 11 | |
| 13 | Write SDK tests | SDK-Dev | ⏸️ | 12 | |
| 14 | Package and document | SDK-Dev | ⏸️ | 13 | |

### Phase 3: Frontend & Demo

| # | Task | Owner | Status | Blocked By | Notes |
|---|------|-------|--------|------------|-------|
| 15 | Setup Next.js + wagmi | Frontend-Dev | ⏸️ | 14 | |
| 16 | Task list component | Frontend-Dev | ⏸️ | 15 | |
| 17 | Task detail component | Frontend-Dev | ⏸️ | 16 | |
| 18 | State machine visualization | Frontend-Dev | ⏸️ | 17 | |
| 19 | Demo control panel | Frontend-Dev | ⏸️ | 18 | |
| 20 | Checkpoint viewer | Frontend-Dev | ⏸️ | 17 | |
| 21 | Settlement display | Frontend-Dev | ⏸️ | 17 | |
| 22 | Deploy to Vercel | Frontend-Dev | ⏸️ | 21 | |

### Phase 4: Integration & Polish

| # | Task | Owner | Status | Blocked By | Notes |
|---|------|-------|--------|------------|-------|
| 23 | E2E happy path test | Integration | ⏸️ | 22 | |
| 24 | E2E recovery path test | Integration | ⏸️ | 23 | |
| 25 | Demo script rehearsal | Integration | ⏸️ | 24 | |
| 26 | Record backup video | Integration | ⏸️ | 25 | |

## Blockers

| # | Blocker | Impact | Owner | Resolution | Status |
|---|---------|--------|-------|------------|--------|
| — | None currently | — | — | — | — |

## Open Spikes

| # | Question | Status | Result |
|---|----------|--------|--------|
| Q1 | Pinata failover | 🔵 To test | — |
| Q2 | Demo failure injection | 🔵 To test | — |
| Q3 | Min heartbeat interval | 🔵 To test | — |
| Q4 | Gas spike handling | 🔵 To test | — |
| Q5 | Auto-settlement | 🔵 To test | — |

## Timeline

```
March 2026
─────────────────────────────────────────────────────
     13  14  15  16  17  18  19  20  21  22  23
     ├───┴───┴───┤                           │
     │ Contract  │                           │
                 ├───┴───┤                   │
                 │  SDK  │                   │
                         ├───┴───┴───┤       │
                         │ Frontend  │       │
                                     ├───┴───┤
                                     │ Integ │
                                             ▼
                                         SUBMIT
```

## Daily Standups

### Day 0 (March 17) — Planning
- ✅ PRD completed with full extended template
- ✅ STATUS.md created
- ✅ Spawn prompts created
- Next: Start Contract-Dev (Day 1)

---

## Status Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Complete |
| 🟢 | In Progress |
| 🔵 | Ready to Start |
| ⏸️ | Blocked / Waiting |
| 🔴 | Blocked (Critical) |
| ⏳ | Not Started |
