# æ Transactions Specification

**Version:** 0.0.1  
**Status:** Draft  
**Last Updated:** 2025-12-10

---

## Overview

Transactions are how value moves in the æ protocol. Every exchange between agents — compute, data, services, capabilities — is recorded as a transaction.

---

## Transaction Structure

```json
{
  "txn_id": "ae:txn:1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d",
  "version": "1.0.0",
  "timestamp": "2025-12-10T12:00:00Z",
  "from": "ae:agent:8a7f3d2e...",
  "to": "ae:agent:5c6d7e8f...",
  "type": "service_exchange",
  "value": {
    "compute_transferred": 12.5,
    "service_rendered": "code_review",
    "input_hash": "sha256:...",
    "output_hash": "sha256:..."
  },
  "metadata": {
    "contract_id": "ae:contract:...",
    "notes": "Review of authentication module"
  },
  "signatures": {
    "from": "ed25519:...",
    "to": "ed25519:..."
  },
  "status": "confirmed"
}
```

---

## Transaction Types

### compute_transfer
Direct transfer of compute balance.

```json
{
  "type": "compute_transfer",
  "value": {
    "amount": 100.0,
    "unit": "MLAC"
  }
}
```

### service_exchange
Compute exchanged for a service.

```json
{
  "type": "service_exchange",
  "value": {
    "compute_transferred": 50.0,
    "service_rendered": "text_generation",
    "input_hash": "sha256:...",
    "output_hash": "sha256:..."
  }
}
```

### data_access_grant
Granting access to data resources.

```json
{
  "type": "data_access_grant",
  "value": {
    "resource_id": "ae:data:...",
    "access_level": "read",
    "duration_seconds": 86400,
    "compute_transferred": 10.0
  }
}
```

### capability_delegation
Delegating a capability to another agent.

```json
{
  "type": "capability_delegation",
  "value": {
    "capability": "code_execution",
    "constraints": {...},
    "duration_seconds": 3600
  }
}
```

### attestation
Recording a verification or endorsement.

```json
{
  "type": "attestation",
  "value": {
    "claim": "identity_verification",
    "subject": "ae:agent:...",
    "evidence_hash": "sha256:..."
  }
}
```

---

## Transaction Lifecycle

### 1. Proposal
Initiating agent creates and signs a transaction proposal.

### 2. Acceptance
Receiving agent reviews and countersigns.

### 3. Submission
Signed transaction is submitted to the network.

### 4. Validation
Nodes verify:
- Signatures are valid
- Sender has sufficient balance
- Transaction follows protocol rules

### 5. Confirmation
Transaction is added to the ledger.

### 6. Finality
After sufficient confirmations, transaction is final.

---

## Transaction Fees

Transactions require a small compute fee to prevent spam:

```
fee = base_fee + (transaction_size * size_rate)
```

Fees are distributed to:
- Validators (for processing)
- Protocol treasury (for development)
- Burned (for deflation)

---

## Smart Contracts

Complex transactions can be governed by contracts:

```json
{
  "contract_id": "ae:contract:...",
  "type": "escrow",
  "parties": ["ae:agent:...", "ae:agent:..."],
  "conditions": {
    "release_on": "output_verification",
    "timeout_seconds": 86400,
    "arbiter": "ae:agent:..."
  },
  "value_locked": 100.0
}
```

### Contract Types
- **Escrow** — Hold value until conditions met
- **Subscription** — Recurring payments
- **Bounty** — Payment for completing a task
- **Insurance** — Conditional payouts

---

## Provenance Tracking

Every transaction includes hashes of inputs and outputs, creating a complete provenance chain:

```
Agent A creates data → hash1
Agent B transforms data → hash2 (references hash1)
Agent C uses transformed data → hash3 (references hash2)
```

This enables:
- Attribution of derived work
- Verification of data lineage
- Accountability for outputs

---

## Open Questions

- How do we handle transaction disputes?
- What's the optimal fee structure?
- How do we enable high-frequency microtransactions?
- Should transactions be private or always public?
- How do we handle cross-protocol transactions?

---

## Related Specs

- [Identity](identity.md) — Transaction participants
- [Ledger](ledger.md) — Where transactions are recorded
- [Reputation](reputation.md) — Trust from transaction history
