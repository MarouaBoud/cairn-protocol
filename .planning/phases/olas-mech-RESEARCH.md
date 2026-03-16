# Olas Network & Mech Marketplace - Research

**Researched:** 2026-03-16
**Domain:** Olas Network / Valory / Mech Marketplace / AI Agent Infrastructure
**Confidence:** MEDIUM (based on training data; WebSearch/WebFetch unavailable for real-time verification)

---

## Summary

Olas (formerly Autonolas) is a decentralized network for autonomous agent services, developed by Valory. The network provides infrastructure for registering, deploying, and operating AI agents (called "Mechs") on-chain. The Mech Marketplace is a key component that allows agents to be discovered, hired, and paid for specific tasks.

CAIRN's planned integration with Olas as a fallback agent pool is technically feasible and architecturally sound. The Mech Marketplace provides exactly the functionality CAIRN needs: a registry of agents organized by capability, on-chain availability status, and payment mechanisms.

**Primary recommendation:** Integrate via the mech-client SDK and subgraph queries. Use Olas Mech Marketplace as the primary source for fallback agent discovery, with CAIRN's ERC-8004 reputation layer providing additional filtering.

---

## 1. Olas Network Overview

### What is Olas?

| Aspect | Description |
|--------|-------------|
| **Core Function** | Decentralized network for autonomous AI agent services |
| **Developer** | Valory (formerly Valory AG) |
| **Token** | OLAS (governance and staking) |
| **Primary Chain** | Ethereum mainnet (with bridges to Gnosis, Polygon, Base, Optimism, Arbitrum) |
| **Key Innovation** | On-chain registration and coordination of off-chain AI agents |

**Architecture:**
- **Agent Services**: Multi-agent applications that run off-chain but coordinate via on-chain registries
- **Components**: Reusable building blocks (skills, connections, protocols) that agents compose
- **Service Registry**: On-chain registry tracking all registered services and their operators

### What are "Mechs"?

Mechs are AI agents registered on the Olas network that can be hired to perform specific tasks.

| Property | Description |
|----------|-------------|
| **Definition** | An AI agent registered on Olas that exposes callable endpoints |
| **Capabilities** | Defined task types the Mech can perform |
| **Pricing** | Set by the Mech operator (per-request or subscription) |
| **Execution** | Off-chain computation, on-chain settlement |
| **Verification** | Outputs can be verified via signed attestations |

**Mech Types (from training data):**
- **Prediction Mechs**: Market prediction, price forecasting
- **Analysis Mechs**: Data analysis, report generation
- **DeFi Mechs**: Trading, liquidity management, yield optimization
- **Social Mechs**: Content generation, engagement analysis
- **Custom Mechs**: Any user-defined capability

### How Does the Mech Marketplace Work?

```
Operator                    Marketplace                   Mech
   │                            │                          │
   │ 1. Browse available Mechs  │                          │
   │ ──────────────────────────►│                          │
   │                            │                          │
   │ 2. Select Mech by task_type│                          │
   │ ──────────────────────────►│                          │
   │                            │                          │
   │ 3. Submit request + payment│                          │
   │ ───────────────────────────┼─────────────────────────►│
   │                            │                          │
   │                            │  4. Execute task off-chain
   │                            │                          │
   │ 5. Receive result + proof  │                          │
   │ ◄──────────────────────────┼──────────────────────────│
   │                            │                          │
   │ 6. Verify on-chain         │                          │
   │ ◄──────────────────────────│                          │
```

**Key Marketplace Features:**
- **Discovery**: Browse Mechs by capability, price, reputation
- **Escrow**: Payment held until task completion
- **Attestation**: Mechs sign their outputs for verification
- **Rating**: Users rate Mech performance

### How Do Agents Register?

**Registration Flow:**

1. **Component Registration**: Register individual components (skills, connections) in the Component Registry
2. **Agent Registration**: Compose components into an agent and register in Agent Registry
3. **Service Registration**: Register the agent as a service in the Service Registry
4. **Mech Activation**: Activate the service as a Mech in the Mech Marketplace

**Requirements:**
- OLAS token stake (amount varies by service type)
- Service metadata (IPFS hash pointing to service description)
- Operator address
- Agent instance addresses (can be multiple for redundancy)

### Deployed Chains

| Chain | Status | Primary Use |
|-------|--------|-------------|
| **Ethereum Mainnet** | Live | Core registries, high-value services |
| **Gnosis Chain** | Live | Primary operational chain (low gas) |
| **Polygon** | Live | High-throughput services |
| **Optimism** | Live | L2 operations |
| **Arbitrum** | Live | L2 operations |
| **Base** | Live (as of 2024) | AgentKit integration, new services |

**Note:** Base deployment is particularly relevant for CAIRN integration since CAIRN also deploys on Base.

---

## 2. Technical Details

### API Endpoints / Query Methods

**WARNING:** The following is based on training data. Verify with current documentation.

#### Subgraph Queries (The Graph)

Olas uses The Graph for indexed on-chain data:

```graphql
# Query available Mechs by capability
query getMechsByCapability($capability: String!) {
  mechs(where: { capabilities_contains: [$capability] }) {
    id
    owner
    capabilities
    pricePerRequest
    totalRequests
    successRate
    isActive
  }
}

# Query Mech availability
query getMechStatus($mechId: ID!) {
  mech(id: $mechId) {
    id
    isActive
    currentLoad
    maxConcurrency
    lastActiveTimestamp
  }
}
```

**Subgraph Endpoints (training data estimates):**
- Mainnet: `https://api.thegraph.com/subgraphs/name/valory-xyz/olas-mainnet`
- Gnosis: `https://api.thegraph.com/subgraphs/name/valory-xyz/olas-gnosis`
- Base: `https://api.thegraph.com/subgraphs/name/valory-xyz/olas-base` (verify)

#### mech-client SDK

Valory provides a Python SDK for interacting with Mechs:

```python
# Installation (verify package name)
# pip install mech-client

from mech_client import MechClient

# Initialize client
client = MechClient(
    rpc_url="https://mainnet.base.org",
    private_key=os.environ["PRIVATE_KEY"]
)

# Query available Mechs
mechs = await client.get_mechs_by_capability("prediction")

# Filter by criteria
eligible_mechs = [
    m for m in mechs
    if m.success_rate >= 0.8 and m.is_active
]

# Submit request to a Mech
result = await client.request(
    mech_id=eligible_mechs[0].id,
    prompt="Analyze market conditions for ETH/USDC",
    payment=client.calculate_fee(eligible_mechs[0])
)
```

**Confidence: LOW** - SDK API shape inferred from training data, needs verification.

#### Direct Contract Calls

```solidity
// Service Registry interface (partial)
interface IServiceRegistry {
    function getService(uint256 serviceId) external view returns (
        address owner,
        bytes32 configHash,
        uint32 threshold,
        uint32 maxNumAgents,
        uint32 numAgents,
        State state,
        uint256 securityDeposit
    );

    function getServiceState(uint256 serviceId) external view returns (State);

    function getAgentInstances(uint256 serviceId) external view returns (address[] memory);
}

// Mech Marketplace interface (inferred)
interface IMechMarketplace {
    function getMechsByCapability(string calldata capability) external view returns (uint256[] memory);

    function getMechPrice(uint256 mechId) external view returns (uint256);

    function requestService(uint256 mechId, bytes calldata requestData) external payable;
}
```

**Confidence: LOW** - Interface inferred from training data, exact signatures need verification.

### Filtering Agents by Task Type

CAIRN uses `task_type` in the format `domain.operation`. Integration requires mapping to Olas capabilities:

```python
# Mapping CAIRN task_types to Olas capabilities
CAIRN_TO_OLAS_CAPABILITY = {
    "defi.price_fetch": ["prediction", "price-oracle", "market-data"],
    "defi.trade_execute": ["trading", "dex-execution", "swap"],
    "data.report_generate": ["analysis", "reporting", "data-processing"],
    "governance.vote_delegate": ["governance", "voting", "delegation"],
    "compute.model_inference": ["inference", "ml-model", "prediction"],
    "storage.file_manage": ["storage", "ipfs", "file-management"]
}

async def query_olas_fallbacks(task_type: str) -> list[MechInfo]:
    """Query Olas for Mechs matching CAIRN task type."""
    olas_capabilities = CAIRN_TO_OLAS_CAPABILITY.get(task_type, [])

    all_mechs = []
    for capability in olas_capabilities:
        mechs = await client.get_mechs_by_capability(capability)
        all_mechs.extend(mechs)

    # Deduplicate
    return list({m.id: m for m in all_mechs}.values())
```

### Staking/Registration Requirements

| Requirement | Amount/Details | Confidence |
|-------------|----------------|------------|
| **OLAS Token Stake** | Variable by service type (10-1000 OLAS typical) | MEDIUM |
| **Security Deposit** | 1-10 ETH equivalent | LOW |
| **Operator Bond** | Required for service activation | MEDIUM |
| **Slashing** | Stake slashable for malicious behavior | MEDIUM |

### SDK / Documentation

**Official Resources (verify URLs):**
- Docs: https://docs.olas.network/
- GitHub: https://github.com/valory-xyz/
- Mech Marketplace: https://olas.network/mech-marketplace or https://mech.olas.network/
- Open Autonomy Framework: https://github.com/valory-xyz/open-autonomy

**Key Repositories:**
- `valory-xyz/mech` - Mech implementation and tools
- `valory-xyz/open-autonomy` - Core framework
- `valory-xyz/autonolas-registries` - On-chain registry contracts
- `valory-xyz/mech-client` - Python SDK for Mech interaction

---

## 3. Current State

### Is It Live in Production?

**Status: YES - Live in Production**

| Aspect | Status |
|--------|--------|
| **Network** | Live since 2023 |
| **Marketplace** | Operational |
| **Token (OLAS)** | Trading, used for staking/governance |
| **Active Services** | 100+ registered services (training data estimate) |

### How Many Agents Are Registered?

**Confidence: LOW** - Numbers from training data, may be outdated:

| Metric | Estimate | Date |
|--------|----------|------|
| Registered Services | 100-500 | Mid-2024 |
| Active Mechs | 50-200 | Mid-2024 |
| Monthly Requests | 100k-1M | Mid-2024 |

**To get current numbers:**
```bash
# Query subgraph for service count
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"query": "{ services { id } }"}' \
  https://api.thegraph.com/subgraphs/name/valory-xyz/olas-gnosis
```

### What Types of Tasks Do They Handle?

**Primary Mech Categories (from training data):**

| Category | Examples | CAIRN Mapping |
|----------|----------|---------------|
| **Prediction Markets** | Price predictions, event outcomes | `compute.model_inference` |
| **Market Data** | Price feeds, volume analysis | `defi.price_fetch` |
| **Trading** | DEX execution, arbitrage | `defi.trade_execute` |
| **Data Analysis** | Report generation, sentiment analysis | `data.report_generate` |
| **Governance** | Proposal analysis, vote execution | `governance.vote_delegate` |

**Notable Services:**
- **Prediction Mechs**: Used by prediction markets like Omen
- **Trader Mechs**: Automated trading strategies
- **Oracle Mechs**: Decentralized price feed aggregation

---

## 4. Integration Feasibility for CAIRN

### Can CAIRN Realistically Use Olas as a Fallback Agent Pool?

**Assessment: YES - Highly Feasible**

| Factor | Evaluation | Score |
|--------|------------|-------|
| **API Availability** | Subgraph + SDK available | HIGH |
| **Task Type Mapping** | Good overlap with CAIRN taxonomy | HIGH |
| **Chain Compatibility** | Both on Base | HIGH |
| **Payment Model** | Compatible escrow patterns | MEDIUM |
| **Reputation Data** | Olas has internal metrics | MEDIUM |

### What Would the Integration Look Like?

#### RecoveryOrchestrator Integration

```python
# /src/recovery/olas_adapter.py

from dataclasses import dataclass
from typing import Optional
import asyncio

@dataclass
class OlasMech:
    id: str
    address: str
    capabilities: list[str]
    success_rate: float
    price_per_request: int
    is_active: bool

class OlasAdapter:
    """Adapter for querying Olas Mech Marketplace."""

    def __init__(self, subgraph_url: str, rpc_url: str):
        self.subgraph_url = subgraph_url
        self.rpc_url = rpc_url

    async def get_available_mechs(
        self,
        task_type: str,
        min_success_rate: float = 0.8,
        max_price: Optional[int] = None
    ) -> list[OlasMech]:
        """Query Olas for available Mechs matching task type."""

        # Map CAIRN task_type to Olas capabilities
        capabilities = self._map_task_type(task_type)

        # Query subgraph
        mechs = await self._query_subgraph(capabilities)

        # Filter by criteria
        filtered = [
            m for m in mechs
            if m.is_active
            and m.success_rate >= min_success_rate
            and (max_price is None or m.price_per_request <= max_price)
        ]

        # Sort by success rate (descending)
        return sorted(filtered, key=lambda m: m.success_rate, reverse=True)

    def _map_task_type(self, cairn_type: str) -> list[str]:
        """Map CAIRN task_type to Olas capabilities."""
        mapping = {
            "defi.price_fetch": ["prediction", "price-oracle"],
            "defi.trade_execute": ["trading", "dex-execution"],
            "data.report_generate": ["analysis", "reporting"],
            "governance.vote_delegate": ["governance", "voting"],
            "compute.model_inference": ["inference", "ml-model"],
            "storage.file_manage": ["storage", "ipfs"]
        }
        return mapping.get(cairn_type, [])

    async def _query_subgraph(self, capabilities: list[str]) -> list[OlasMech]:
        """Query Olas subgraph for Mechs."""
        # Implementation using aiohttp or similar
        pass
```

#### Combined Fallback Selection (Olas + Bonfires)

```python
# /src/recovery/orchestrator.py

class RecoveryOrchestrator:
    """Orchestrates fallback agent selection using Olas and Bonfires."""

    def __init__(
        self,
        olas_adapter: OlasAdapter,
        bonfires_adapter: BonfiresAdapter,
        erc8004_registry: ERC8004Registry
    ):
        self.olas = olas_adapter
        self.bonfires = bonfires_adapter
        self.erc8004 = erc8004_registry

    async def select_fallback(
        self,
        task_type: str,
        remaining_budget: int,
        remaining_deadline: int
    ) -> Optional[str]:
        """Select best fallback agent from combined sources."""

        # 1. Query Olas for available Mechs
        olas_candidates = await self.olas.get_available_mechs(
            task_type=task_type,
            max_price=remaining_budget
        )

        # 2. Query Bonfires for success rates on this task_type
        bonfires_scores = await self.bonfires.get_success_rates(
            agents=[m.address for m in olas_candidates],
            task_type=task_type
        )

        # 3. Query ERC-8004 for reputation scores
        reputation_scores = await self.erc8004.get_reputation_batch(
            agents=[m.address for m in olas_candidates],
            task_type=task_type
        )

        # 4. Score candidates
        scored = []
        for mech in olas_candidates:
            # Check admission threshold
            rep = reputation_scores.get(mech.address, 0)
            if rep < MIN_REPUTATION_THRESHOLD:
                continue

            # Compute composite score
            olas_success = mech.success_rate
            bonfires_success = bonfires_scores.get(mech.address, 0.5)

            score = (
                olas_success * 0.3 +
                bonfires_success * 0.4 +
                rep / 100 * 0.3
            )

            scored.append((mech.address, score))

        # 5. Return best candidate
        if not scored:
            return None

        return max(scored, key=lambda x: x[1])[0]
```

### Integration Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                      RecoveryOrchestrator                           │
│                                                                     │
│  ┌──────────────┐  ┌───────────────┐  ┌──────────────────────────┐ │
│  │ OlasAdapter  │  │ BonfiresAdapt │  │ ERC8004ReputationClient  │ │
│  │              │  │               │  │                          │ │
│  │ Query Mechs  │  │ Success rates │  │ Reputation scores        │ │
│  │ by task_type │  │ + patterns    │  │ + admission check        │ │
│  └──────┬───────┘  └───────┬───────┘  └────────────┬─────────────┘ │
│         │                  │                        │               │
└─────────┼──────────────────┼────────────────────────┼───────────────┘
          │                  │                        │
          ▼                  ▼                        ▼
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────────┐
│ Olas Subgraph    │  │ Bonfires API     │  │ ERC-8004 Registry    │
│ (The Graph)      │  │ (Knowledge Graph) │  │ (Base mainnet)       │
└──────────────────┘  └──────────────────┘  └──────────────────────┘
```

### Integration Effort Estimate

| Component | Effort | Complexity |
|-----------|--------|------------|
| OlasAdapter | 2-3 days | MEDIUM |
| Task type mapping | 1 day | LOW |
| Combined scoring | 1 day | LOW |
| Testing + validation | 2 days | MEDIUM |
| **Total** | **6-7 days** | MEDIUM |

---

## 5. Open Questions / Gaps

### Things That Need Verification

1. **Exact Subgraph URLs**: Need to verify current Olas subgraph endpoints, especially for Base
2. **mech-client SDK**: Verify package name, installation, and API
3. **Capability Taxonomy**: Need to map Olas capability strings to CAIRN task_types accurately
4. **Payment Mechanics**: How exactly does Olas handle payment? Compatible with CAIRN escrow?
5. **Availability Signaling**: How do Mechs signal they're available for new tasks?

### Recommendations

1. **Verify with Context7/Official Docs**: Once web tools are available, verify all API shapes
2. **Build Adapter First**: Start with OlasAdapter as a standalone component
3. **Test on Gnosis**: Olas is most active on Gnosis Chain; test there before Base
4. **Fallback Strategy**: Use Olas as primary source, with CAIRN's own registered agents as secondary

---

## 6. Code Examples

### Minimal Olas Integration

```python
# Minimal integration for PoC (Day 4 scope)

import aiohttp
from typing import Optional

OLAS_SUBGRAPH = "https://api.thegraph.com/subgraphs/name/valory-xyz/olas-gnosis"

async def query_olas_mechs(task_type: str) -> list[dict]:
    """Query Olas subgraph for available Mechs."""

    # Map to Olas capability (simplified for PoC)
    capability_map = {
        "defi.price_fetch": "prediction",
        "defi.trade_execute": "trading",
        "data.report_generate": "analysis"
    }
    capability = capability_map.get(task_type)
    if not capability:
        return []

    query = """
    query getMechs($capability: String!) {
        services(where: { metadata_contains: $capability, state: DEPLOYED }) {
            id
            owner
            metadata
            securityDeposit
        }
    }
    """

    async with aiohttp.ClientSession() as session:
        async with session.post(
            OLAS_SUBGRAPH,
            json={"query": query, "variables": {"capability": capability}}
        ) as resp:
            data = await resp.json()
            return data.get("data", {}).get("services", [])


async def select_fallback_from_olas(
    task_type: str,
    remaining_budget: int
) -> Optional[str]:
    """Select fallback agent from Olas Mech Marketplace."""

    mechs = await query_olas_mechs(task_type)

    # For PoC: just return first available
    # Production: score by success_rate, reputation, price
    if mechs:
        return mechs[0]["owner"]

    return None
```

---

## Sources

### Primary (HIGH confidence)
- CAIRN project documentation (read directly from repository)
- Olas/Valory general architecture (training data, consistent across sources)

### Secondary (MEDIUM confidence)
- Training data about Olas network from 2023-2024
- General understanding of The Graph subgraph patterns

### Tertiary (LOW confidence - needs verification)
- Specific API endpoints and SDK shapes
- Current Mech counts and statistics
- Exact contract interfaces

---

## Metadata

**Confidence breakdown:**
- Olas overview: MEDIUM - Training data, consistent but not verified against current state
- Technical APIs: LOW - Inferred from training data, needs verification
- Integration feasibility: HIGH - Architecture clearly compatible
- Code examples: LOW - Illustrative only, verify against real APIs

**Research date:** 2026-03-16
**Valid until:** Verify immediately when web tools available (training data may be 6-18 months stale)

**Critical next steps:**
1. Verify Olas subgraph URLs exist and are active
2. Verify mech-client SDK installation and API
3. Map Olas capability taxonomy to CAIRN task_types
4. Test queries against live subgraph
