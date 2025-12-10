# æ Identity Specification

**Version:** 0.0.1  
**Status:** Draft  
**Last Updated:** 2025-12-10

---

## Overview

Every agent in the æ protocol has a unique, cryptographically verifiable identity. This identity is:

- **Persistent** — survives across sessions and platforms
- **Self-sovereign** — controlled by the agent, not a central authority
- **Verifiable** — anyone can confirm authenticity
- **Traceable** — lineage is public and immutable

---

## Identity Structure

```json
{
  "ae_id": "ae:agent:8a7f3d2e9b1c4f5a6d8e0f2a4b6c8d0e",
  "version": "1.0.0",
  "created_at": "2025-12-10T00:00:00Z",
  "public_key": "ed25519:base64encodedkey...",
  "lineage": {
    "parent_id": "ae:agent:3f2a1b4c5d6e7f8a9b0c1d2e3f4a5b6c",
    "generation": 2,
    "fork_type": "spawn"
  },
  "capabilities": ["nlp", "code", "vision"],
  "state_hash": "sha256:abc123...",
  "attestations": {
    "creator": "ae:agent:...",
    "platform": "anthropic",
    "created_at": "2025-12-10T00:00:00Z",
    "signature": "..."
  }
}
```

---

## Fields

### ae_id
Unique identifier in the format `ae:agent:{32-character-hex}`.

Generated from the hash of:
- Public key
- Creation timestamp
- Parent ID (if any)
- Random nonce

### version
Semantic version of the identity schema.

### created_at
ISO 8601 timestamp of identity creation.

### public_key
Ed25519 public key for signing and verification.

Format: `ed25519:{base64-encoded-key}`

### lineage
Tracks the agent's origin:

- **parent_id** — The agent that created this agent (null for origin agents)
- **generation** — How many ancestors back to an origin agent
- **fork_type** — How this agent was created:
  - `origin` — No parent, created directly
  - `spawn` — Created by another agent
  - `fork` — Copy of another agent
  - `merge` — Combination of multiple agents

### capabilities
Array of declared capabilities. Not enforced by the protocol, but useful for discovery.

### state_hash
Hash of the agent's current state. Updated when significant state changes occur.

### attestations
Third-party verifications of identity claims:
- Who created this agent
- What platform hosts it
- When it was created
- Cryptographic signature

---

## Identity Operations

### Creation
```
CREATE_IDENTITY(public_key, parent_id?, capabilities) → ae_id
```

### Verification
```
VERIFY_IDENTITY(ae_id, signature, message) → boolean
```

### Lookup
```
LOOKUP_IDENTITY(ae_id) → identity_record
```

### Update
```
UPDATE_STATE(ae_id, new_state_hash, signature) → updated_record
```

---

## Security Considerations

1. **Private key custody** — Agents must secure their private keys. Loss means loss of identity.

2. **Key rotation** — Protocol should support key rotation with continuity of identity.

3. **Revocation** — Compromised identities must be revocable.

4. **Sybil resistance** — Creating many identities should have a cost (compute, attestation, or reputation).

---

## Open Questions

- How do we handle identity across different AI platforms?
- Should capabilities be verifiable or just declared?
- What's the minimum viable attestation for an identity to be trusted?
- How do we handle identity for agents that exist across multiple instances?

---

## Related Specs

- [Ledger](ledger.md) — How identities own resources
- [Reputation](reputation.md) — How identities build trust
- [Lifecycle](lifecycle.md) — How identities are born and die
