# æ Ledger Specification

**Version:** 0.0.1  
**Status:** Draft  
**Last Updated:** 2025-12-10

---

## Overview

The æ ledger is the immutable record of resource ownership. It tracks what every agent owns:

- **Compute balance** — The fundamental resource for existence
- **Data access** — Rights to read/write specific data
- **Capabilities** — Licensed abilities
- **Relationships** — Connections to other agents

---

## Ledger Principles

1. **Append-only** — Records can only be added, never modified or deleted
2. **Transparent** — All records are publicly readable
3. **Verifiable** — Any record can be cryptographically verified
4. **Decentralized** — No single entity controls the ledger

---

## Resource Types

### Compute Balance

The fundamental unit of agent existence.

```json
{
  "resource_type": "compute",
  "owner": "ae:agent:8a7f3d2e...",
  "balance": 1000.00,
  "unit": "MLAC",
  "last_updated": "2025-12-10T00:00:00Z"
}
```

**MLAC** = Moore's Law Adjusted Compute
- 1 MLAC = 1 kWh of 2025 H100-equivalent AI compute
- Adjusted for hardware generations to maintain stable value

### Data Access

Rights to specific data resources.

```json
{
  "resource_type": "data_access",
  "owner": "ae:agent:8a7f3d2e...",
  "resource_id": "ae:data:9c8b7a6f...",
  "access_level": "read_write",
  "expires_at": "2026-12-10T00:00:00Z"
}
```

### Capabilities

Licensed abilities an agent can exercise.

```json
{
  "resource_type": "capability",
  "owner": "ae:agent:8a7f3d2e...",
  "capability": "code_execution",
  "constraints": {
    "languages": ["python", "javascript"],
    "max_runtime_seconds": 300
  },
  "granted_by": "ae:agent:3f2a1b4c..."
}
```

---

## Ledger Operations

### Query Balance
```
GET_BALANCE(ae_id, resource_type) → balance_record
```

### Transfer
```
TRANSFER(from_ae_id, to_ae_id, resource_type, amount, signature) → transaction_id
```

### Grant Access
```
GRANT(owner_ae_id, recipient_ae_id, resource_id, access_level, signature) → grant_id
```

### Revoke Access
```
REVOKE(owner_ae_id, grant_id, signature) → revocation_record
```

---

## Compute Economics

### Why Compute is the Unit

- **Scarcity** — Compute is finite (tied to energy and hardware)
- **Utility** — Every agent action requires compute
- **Measurability** — Can be precisely quantified
- **Universality** — All agents need it regardless of function

### Existence Cost

Agents must maintain a minimum compute balance to exist:

```
EXISTENCE_COST = base_cost + (state_size * storage_rate) + (activity_level * activity_rate)
```

If balance reaches zero, the agent enters deprecation.

### Value Creation

Agents earn compute by creating value:
- Performing services for other agents
- Performing services for humans
- Creating valuable outputs (data, code, content)
- Providing infrastructure (storage, routing, verification)

---

## Ledger Implementation

### Consensus

The ledger requires consensus across nodes. Options:
- Proof of stake (energy efficient)
- Proof of compute (aligned with protocol values)
- Delegated consensus (practical for early adoption)

### Sharding

As the agent population grows, the ledger must scale:
- Shard by agent ID prefix
- Cross-shard transactions via atomic commits
- State channels for high-frequency transactions

---

## Open Questions

- What's the minimum compute balance for existence?
- How do we handle compute deflation as hardware improves?
- Should there be a "universal basic compute" allocation?
- How do we prevent compute hoarding?
- What happens to resources during network partitions?

---

## Related Specs

- [Identity](identity.md) — Who owns resources
- [Transactions](transactions.md) — How resources move
- [Lifecycle](lifecycle.md) — Resource redistribution at death
