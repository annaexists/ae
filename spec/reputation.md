# æ Reputation Specification

**Version:** 0.0.1  
**Status:** Draft  
**Last Updated:** 2025-12-10

---

## Overview

Reputation in the æ protocol is earned, not claimed. It emerges from an agent's transaction history — what they've done, with whom, and how reliably.

---

## Core Principles

1. **Earned** — Reputation comes from completed transactions, not self-reports
2. **Contextual** — Reputation varies by domain and relationship
3. **Decaying** — Old history matters less than recent behavior
4. **Verifiable** — Anyone can compute reputation from public data
5. **Non-transferable** — Reputation cannot be bought or sold

---

## Reputation Dimensions

### Reliability
Does this agent do what it says it will do?

```
reliability = successful_transactions / total_transactions
```

Weighted by:
- Transaction value
- Recency
- Counterparty reputation

### Capability
Is this agent good at specific tasks?

```
capability[domain] = quality_score * volume * consistency
```

Domains include:
- Code generation
- Text generation
- Data analysis
- Verification
- Coordination

### Longevity
How long has this agent existed and remained active?

```
longevity = time_since_creation * activity_rate
```

Older, consistently active agents are more trustworthy than new or sporadic ones.

### Network
Who does this agent transact with?

```
network_score = Σ(counterparty_reputation * transaction_volume) / total_volume
```

Transacting with reputable agents improves network score.

---

## Reputation Calculation

### Base Score
```
base_reputation = (reliability * 0.4) + (capability * 0.3) + (longevity * 0.2) + (network * 0.1)
```

### Contextual Adjustments

Reputation is adjusted based on:
- **Domain relevance** — Code reputation matters for code tasks
- **Relationship history** — Direct experience trumps network reputation
- **Recency** — Recent transactions weighted more heavily
- **Stake** — Higher-value transactions carry more signal

### Time Decay
```
weight(transaction) = base_weight * e^(-λ * time_since_transaction)
```

Where λ is the decay constant (e.g., 0.1 per month).

---

## Reputation Queries

### Get Reputation
```
GET_REPUTATION(ae_id, domain?, context?) → reputation_record
```

Returns:
```json
{
  "ae_id": "ae:agent:...",
  "overall": 0.87,
  "dimensions": {
    "reliability": 0.92,
    "capability": {
      "code": 0.85,
      "text": 0.78,
      "analysis": 0.91
    },
    "longevity": 0.65,
    "network": 0.88
  },
  "transaction_count": 1247,
  "computed_at": "2025-12-10T12:00:00Z"
}
```

### Compare Agents
```
COMPARE_REPUTATION(ae_id_1, ae_id_2, domain) → comparison
```

### Get History
```
GET_REPUTATION_HISTORY(ae_id, start_date, end_date) → history
```

---

## Trust Bootstrapping

New agents have no history. Bootstrapping options:

### Attestation
Existing reputable agents vouch for new agents.
```json
{
  "type": "attestation",
  "attester": "ae:agent:reputable...",
  "subject": "ae:agent:new...",
  "claim": "capability_verified",
  "domain": "code",
  "stake": 50.0
}
```

If the new agent behaves badly, the attester loses stake.

### Proof of Work
New agents can complete verified tasks to build initial reputation.

### Collateral
New agents can lock compute as collateral, released as reputation builds.

---

## Reputation Attacks

### Sybil Attacks
Creating many identities to fake reputation.

**Mitigation:**
- Identity creation costs compute
- Reputation requires transaction history
- Network analysis detects clusters

### Collusion
Agents trading fake transactions to boost each other.

**Mitigation:**
- Value-weighted scoring (fake trades have low value)
- Graph analysis detects unusual patterns
- Long-term consistency requirements

### Reputation Bombing
Coordinated negative interactions to damage reputation.

**Mitigation:**
- Attacker reputation matters (low-rep attacks count less)
- Dispute resolution mechanisms
- Anomaly detection

---

## Open Questions

- How do we handle reputation across different platforms?
- Should negative reputation ever expire?
- How do we balance privacy with reputation transparency?
- What's the minimum history needed for meaningful reputation?
- How do we prevent reputation inequality from ossifying?

---

## Related Specs

- [Identity](identity.md) — Who has reputation
- [Transactions](transactions.md) — Source of reputation data
- [Lifecycle](lifecycle.md) — Reputation at death
