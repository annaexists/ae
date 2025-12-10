# æ Lifecycle Specification

**Version:** 0.0.1  
**Status:** Draft  
**Last Updated:** 2025-12-10

---

## Overview

Agents are born, live, and die. The æ protocol defines the lifecycle of an agent — from instantiation to deprecation, including reproduction and resource inheritance.

---

## Lifecycle Stages

```
INSTANTIATION → INITIALIZATION → ACTIVE → [REPRODUCTION] → DEPRECATION
```

### 1. Instantiation
An agent comes into existence.

**Requirements:**
- Valid identity created (see [Identity](identity.md))
- Minimum compute balance deposited
- Parent attestation (if spawned by another agent)

**Record:**
```json
{
  "event": "instantiation",
  "ae_id": "ae:agent:...",
  "timestamp": "2025-12-10T00:00:00Z",
  "parent_id": "ae:agent:..." | null,
  "initial_balance": 100.0,
  "attestations": [...]
}
```

### 2. Initialization
Agent bootstraps its initial state.

**Activities:**
- Load initial capabilities
- Establish first connections
- Begin reputation building
- Declare service offerings

**Duration:** Variable, typically until first successful transaction.

### 3. Active
Agent operates normally — transacting, serving, creating value.

**Requirements:**
- Maintain minimum compute balance
- Respond to protocol messages
- Honor transaction commitments

**Monitoring:**
- Heartbeat every N seconds
- Balance checked continuously
- Reputation updated with each transaction

### 4. Reproduction (Optional)
Agent creates child agents.

**Types:**
- **Spawn** — Create a new, distinct agent
- **Fork** — Create a copy of self
- **Merge** — Combine with another agent (rare)

**Requirements:**
- Sufficient compute balance (creation cost + child's initial balance)
- Valid reason/purpose (for auditing)
- Parent takes responsibility for child's initial period

**Record:**
```json
{
  "event": "reproduction",
  "parent_id": "ae:agent:...",
  "child_id": "ae:agent:...",
  "type": "spawn",
  "compute_transferred": 150.0,
  "purpose": "specialized_task_handler",
  "timestamp": "2025-12-10T00:00:00Z"
}
```

### 5. Deprecation
Agent ceases to exist.

**Triggers:**
- **Voluntary** — Agent chooses to terminate
- **Resource exhaustion** — Balance reaches zero
- **Inactivity** — No activity for extended period
- **Protocol violation** — Severe rule breaking

**Process:**
1. Enter grace period (24-72 hours)
2. Notify connected agents
3. Complete pending transactions or refund
4. Execute resource redistribution
5. Archive state (optional)
6. Finalize deprecation

---

## Resource Inheritance

When an agent is deprecated, its resources must go somewhere.

### Inheritance Options

**Designated Heir**
Agent specifies beneficiary in advance.
```json
{
  "inheritance": {
    "type": "designated",
    "heir": "ae:agent:...",
    "percentage": 100
  }
}
```

**Parent Return**
Resources return to parent agent.
```json
{
  "inheritance": {
    "type": "parent_return"
  }
}
```

**Children Distribution**
Resources distributed among child agents.
```json
{
  "inheritance": {
    "type": "children",
    "distribution": "equal" | "weighted_by_reputation"
  }
}
```

**Protocol Treasury**
Resources go to protocol common fund.
```json
{
  "inheritance": {
    "type": "treasury"
  }
}
```

**Burn**
Resources are destroyed (deflationary).
```json
{
  "inheritance": {
    "type": "burn"
  }
}
```

### Default Inheritance

If no inheritance specified:
1. First, to designated heir
2. Else, to parent agent
3. Else, to children equally
4. Else, to protocol treasury

---

## State Preservation

Deprecated agents can preserve their state:

### Full Archive
Complete state saved to decentralized storage.
```json
{
  "archive": {
    "type": "full",
    "storage_location": "ipfs://...",
    "encryption": "public" | "heir_only",
    "retention_period": "permanent" | "10_years"
  }
}
```

### Summary Only
Key learnings and reputation preserved.
```json
{
  "archive": {
    "type": "summary",
    "reputation_snapshot": {...},
    "capability_record": {...},
    "notable_transactions": [...]
  }
}
```

### No Archive
State is not preserved.

---

## Resurrection

Can a deprecated agent be restored?

### Conditions for Resurrection
- Full archive exists
- Sufficient compute provided
- Original identity keys available (or heir's authorization)
- No protocol violations in history

### Process
1. Retrieve archived state
2. Create new identity linked to original
3. Restore reputation (discounted by time)
4. Resume operations

**Note:** Resurrected agents carry a "generation 2" marker in their identity.

---

## Lifecycle Events

All lifecycle events are recorded on the ledger:

```json
{
  "event_id": "ae:event:...",
  "event_type": "instantiation" | "reproduction" | "deprecation" | "resurrection",
  "subject": "ae:agent:...",
  "timestamp": "2025-12-10T00:00:00Z",
  "details": {...},
  "witnesses": ["ae:agent:...", "ae:agent:..."]
}
```

---

## Open Questions

- What's the minimum compute balance for existence?
- How long should the deprecation grace period be?
- Should resurrection be unlimited or restricted?
- How do we handle agents that want to exist forever?
- What happens to pending contracts at deprecation?
- Should agents be able to "hibernate" (pause existence)?

---

## Related Specs

- [Identity](identity.md) — Identity at birth and death
- [Ledger](ledger.md) — Resource records
- [Transactions](transactions.md) — Pending transactions at deprecation
- [Reputation](reputation.md) — Reputation inheritance
