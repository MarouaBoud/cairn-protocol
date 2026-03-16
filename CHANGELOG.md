# Changelog

All notable changes to the CAIRN Protocol will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Added
- Initial protocol specification
- Documentation structure

---

## [1.0.0] - 2026-XX-XX

### Added

#### Protocol Core
- Six-state failure and recovery state machine (IDLE, RUNNING, FAILED, RECOVERING, RESOLVED, DISPUTED)
- Three-class failure taxonomy (LIVENESS, RESOURCE, LOGIC)
- Deterministic recovery score algorithm
- Checkpoint-based task resumption protocol
- Permissionless enforcement functions (checkLiveness, checkBudget, checkDeadline)

#### Smart Contracts
- `CairnTask.sol` — Core state machine contract (~250 lines)
- `CairnHook.sol` — ERC-8183 hook implementation (~80 lines)
- `ICairnTask` interface
- `ICairnHook` interface

#### Standards Integration
- ERC-8183 integration via Hook interface
- ERC-8004 integration for identity and reputation
- ERC-7710 integration for caveat-enforced delegation

#### Execution Intelligence Layer
- Failure Record schema (v1)
- Resolution Record schema (v1)
- IPFS storage for execution records
- On-chain event emission for indexing

#### Documentation
- Protocol whitepaper
- ERC specification (EIP format)
- Security model documentation
- Architecture documentation
- Integration guides

### Standards

- Follows EIP-1 specification format
- Implements ERC-8183 Hook interface
- Compatible with ERC-8004 registries
- Compatible with ERC-7710 delegation

### Security

- Checkpoint schema validation
- Two-gate fallback admission (reputation + stake)
- Stake-based arbiter accountability
- Permissionless enforcement (no trusted keepers)

---

## Version History

| Version | Date | Description |
|---------|------|-------------|
| 1.0.0 | 2026-XX-XX | Initial release |

---

## Upgrade Policy

- CairnTask.sol is non-upgradeable by design
- New versions are deployed as new contracts
- In-flight tasks complete under their original version
- No forced migration of existing tasks

## Migration Guides

Migration guides will be added here when new versions are released.

---

*CAIRN Protocol Changelog*
