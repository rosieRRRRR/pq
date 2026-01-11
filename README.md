# PQ — Post-Quantum Security Ecosystem

**An Open Standard for Deterministic, Post-Quantum-Safe Custody, Compliance, and AI Operations**

* **Specification Version:** 2.0.0
* **Status:** Public beta
* **Date:** 2026
* **Author:** rosiea
* **Contact:** PQRosie@proton.me
* **Licence:** Apache License 2.0 — Copyright 2026 rosiea

---

## Summary

PQ is a composed ecosystem of specifications that eliminates entire classes of security failures present in modern systems while ensuring cryptographic agility for the post-quantum transition.

Most post-quantum projects focus narrowly on future cryptography. PQ addresses attacks that succeed daily—replay, time forgery, silent runtime compromise, consent reuse, execution-gap exploitation—while preparing for quantum-capable adversaries.

PQ replaces trust assumptions with explicit, verifiable predicates. No component grants authority in isolation. All enforcement flows through a single deterministic core: **PQSEC**.

**This document is a conceptual overview and ecosystem guide. For normative enforcement semantics, predicate definitions, and implementation requirements, see PQSEC.**

---

## Non-Normative Overview — For Explanation and Orientation Only

**This section is NOT part of the conformance surface.  
It is provided for explanatory and onboarding purposes only.**

### Plain Summary

PQ is a family of specifications that work together to provide deterministic, auditable security for custody, AI operations, and regulated transactions. Each specification produces evidence or defines structure. None grants authority alone. Authority emerges only when all required predicates are satisfied and evaluated by PQSEC.

### What PQ Is / Is Not

| PQ IS | PQ IS NOT |
|-------|-----------|
| An ecosystem of composed specifications | A single monolithic protocol |
| A conceptual and architectural framework | An enforcement engine (that's PQSEC) |
| A guide to component relationships | A replacement for component specs |
| Post-quantum ready | Post-quantum only |

### The Core Insight

**Nothing grants authority. Everything produces evidence. PQSEC refuses or doesn't refuse.**

That's the entire security model.

### Why This Exists

Modern systems fail because they assume:

* clocks are honest
* runtimes are stable
* models behave consistently
* signatures imply authority
* stored data can be protected indefinitely

These assumptions are routinely false. PQ replaces them with structural guarantees: explicit verification of time, runtime state, intent, consent, policy, and authority—all enforced deterministically through a single refusal-only core.

---

## 1. Reading Guide

### Where to Start

| If you want to... | Start with... |
|-------------------|---------------|
| Understand the architecture | This document (PQ) |
| Implement enforcement | **PQSEC** — the enforcement core |
| Implement Bitcoin custody | PQHD → PQSEC |
| Implement AI governance | PQAI → PQSEC |
| Understand time semantics | Epoch Clock |
| Understand encoding rules | PQSF |
| Implement execution boundaries | ZET/ZEB → PQEH |

### Specification Relationships

```
                    ┌─────────────────────────────────────────┐
                    │              PQ (this document)         │
                    │         Conceptual hub and guide        │
                    └─────────────────────────────────────────┘
                                        │
          ┌─────────────────────────────┼─────────────────────────────┐
          │                             │                             │
          ▼                             ▼                             ▼
   ┌─────────────┐              ┌─────────────┐              ┌─────────────┐
   │ Epoch Clock │              │    PQSF     │              │    PQVL     │
   │  Verifiable │              │  Canonical  │              │   Runtime   │
   │    Time     │              │  Encoding   │              │ Attestation │
   └──────┬──────┘              └──────┬──────┘              └──────┬──────┘
          │                            │                            │
          └────────────────────────────┼────────────────────────────┘
                                       │
                                       ▼
                         ┌───────────────────────┐
                         │        PQSEC          │
                         │  ━━━━━━━━━━━━━━━━━━━  │
                         │   ENFORCEMENT CORE    │
                         │   All authority flows │
                         │   through here        │
                         └───────────┬───────────┘
                                     │
          ┌──────────────┬───────────┼───────────┬──────────────┐
          │              │           │           │              │
          ▼              ▼           ▼           ▼              ▼
   ┌───────────┐  ┌───────────┐ ┌─────────┐ ┌─────────┐  ┌────────────┐
   │   PQHD    │  │   PQAI    │ │ ZET/ZEB │ │  PQEH  │  │Neural Lock │
   │  Custody  │  │    AI     │ │Execution│ │Quantum  │  │  Human     │
   │  Policy   │  │ Identity  │ │Boundary │ │Hardening│  │  State     │
   └───────────┘  └───────────┘ └─────────┘ └─────────┘  └────────────┘
```

### Dependency Summary

All specifications in the PQ ecosystem produce evidence or define structure.
No specification grants authority in isolation.
All enforcement and refusal semantics are defined exclusively by **PQSEC**.

| Specification | Depends On |
|---------------|------------|
| PQSEC | PQSF, Epoch Clock, PQVL |
| PQHD | PQSEC, PQSF, Epoch Clock |
| PQAI | PQSEC, PQSF, Epoch Clock, PQVL |
| ZET / ZEB | PQSEC, Epoch Clock |
| PQEH | PQSEC, PQHD, ZET / ZEB, Epoch Clock |
| Neural Lock | PQSEC, PQSF, PQHD, Epoch Clock |
| PQVL | PQSF, Epoch Clock |
| Epoch Clock | Bitcoin |
| PQSF | Epoch Clock |

---

## 2. Architecture Principles

### 2.1 Refusal-Only Enforcement

PQ does not ask “is this allowed?” It asks “is there any reason to refuse?”

No artefact, key, model, device, or component grants authority. An operation proceeds only if PQSEC does not refuse it after evaluating all required predicates.

This is not a semantic distinction. It changes the failure mode from “fail-open on missing permission” to “fail-closed on missing evidence.”

---

### 2.2 Evidence Production vs Authority

Every component except PQSEC produces evidence:

| Component | Produces |
|-----------|----------|
| Epoch Clock | Time artefacts (ticks) |
| PQVL | Runtime attestation envelopes |
| PQAI | Model identity, behavioural fingerprints, drift classification |
| PQHD | Custody policy, predicate requirements |
| ZET/ZEB | Execution intents and results |
| Neural Lock | Operator state attestations |

None of these artefacts carry authority. They are inputs to PQSEC, which produces the sole authoritative output: an EnforcementOutcome.

---

### 2.3 Single Enforcement Authority

PQSEC is the only component that produces enforcement decisions.

Any parallel enforcement logic outside PQSEC is non-conformant and creates bypass vectors. This is not a recommendation; it is a structural requirement.

---

### 2.4 Determinism

Given identical inputs, PQSEC produces identical outputs. There is no probabilistic evaluation, no heuristic judgment, no “usually works.” Enforcement is reproducible and auditable.

---

### 2.5 Fail-Closed

Uncertainty results in refusal:

* Missing input → refuse  
* Non-canonical encoding → refuse  
* Ambiguous time → refuse  
* Unverifiable signature → refuse  
* Partial predicate satisfaction → refuse  

There are no degraded modes for Authoritative operations.

---

### 2.6 Enforcement Invariant (Ecosystem Requirement)

Across the entire PQ ecosystem, enforcement authority is centralized.

1. **Only PQSEC MAY emit an authoritative ALLOW outcome** for any operation attempt.
2. No other specification, component, artefact, or subsystem MAY emit any signal whose semantics imply permission, approval, or execution capability.
3. All other specifications define structure or produce evidence only. They MUST NOT grant authority, directly or indirectly.
4. Any implementation that produces an allow or approval signal outside PQSEC is non-conformant and creates enforcement bypass vectors.

This invariant applies uniformly across custody, execution, time, runtime attestation, AI operations, and human-state extensions.

---

## 3. Component Overview

### 3.1 Epoch Clock — Verifiable Time

**Problem:** System clocks lie. Network time can be manipulated.

**Solution:** Bitcoin-anchored, threshold-signed time artefacts.

Epoch Clock produces signed ticks that can be verified independently. Profiles are inscribed as Bitcoin ordinals (immutable). Ticks are distributed via mirrors without trust requirements. Consumers verify signatures locally.

Epoch Clock produces time artefacts only. It does not enforce freshness—PQSEC does.

**Specification:** Epoch Clock v2.1.1

### 3.2 PQSF — Canonical Encoding and Cryptographic Indirection

**Problem:** Ambiguous encoding breaks signatures. Algorithm transitions break systems.

**Solution:** Deterministic CBOR, JCS Canonical JSON (for Epoch Clock), and CryptoSuiteProfile indirection.

PQSF defines how artefacts are encoded, hashed, and signed. It provides cryptographic agility through profile references—algorithm changes don't require architectural changes.

PQSF defines grammar and encoding only. It grants no authority.

**Specification:** PQSF v2.0.2

### 3.3 PQVL — Runtime Attestation

**Problem:** Compromised runtimes produce compromised outputs.

**Solution:** Deterministic probe collection, baseline comparison, and drift classification.

PQVL produces attestation envelopes describing measured runtime state. Drift is classified as NONE, WARNING, or CRITICAL. Attestation is evidence, not permission—PQSEC decides what to do with it.

**Specification:** PQVL v1.0.3

### 3.4 PQSEC — Enforcement Core

**Problem:** Distributed enforcement creates bypass vectors.

**Solution:** Single, deterministic, refusal-only enforcement authority.

PQSEC consumes evidence from all other components and produces exactly one outcome per operation: ALLOW, DENY, or FAIL_CLOSED_LOCKED. It evaluates predicates, enforces freshness and monotonicity, manages lockout, and maintains audit trails.

**PQSEC is where authority lives. All other components feed into it.**

**Specification:** PQSEC v2.0.2

#### 3.4.1 Session Continuity and Resumption
Session continuity and optional session resumption are treated as evidence-only mechanisms. Any reuse of session state across connections is subject to deterministic evaluation by PQSEC and MUST NOT bypass time, policy, consent, runtime, or ledger predicates.

Normative enforcement rules for session resumption, when enabled by policy, are defined exclusively in PQSEC.

### 3.5 PQHD — Custody Authority

**Problem:** Key possession is treated as authority. Keys can be stolen.

**Solution:** Predicate-driven custody where keys are necessary but not sufficient.

PQHD defines what must be true before Bitcoin signing is allowed: time bounds, consent, policy, runtime integrity, quorum, ledger continuity. Key possession alone conveys no authority.

PQHD defines custody policy. PQSEC enforces it.

**Specification:** PQHD v1.1.0

### 3.6 ZET/ZEB — Execution Boundary

**Problem:** Executable artefacts exist before authorization completes, enabling front-running and reaction attacks.

**Solution:** Strict phase separation between intent and execution.

ZET defines a rail-agnostic execution boundary: intents are non-authoritative and safe to observe; execution occurs only after PQSEC approval. ZEB implements the Bitcoin profile with broadcast discipline and exposure detection.

ZET/ZEB provide execution mechanics only. They grant no authority.

**Specification:** ZEB v1.2.0 (includes ZET)

### 3.7 PQEH — Post-Quantum Execution Hardening

**Problem:** Classical signatures can be observed before broadcast, enabling quantum pre-construction attacks.

**Solution:** S1/S2 revelation pattern that denies pre-construction.

PQEH separates commitment (S1, non-executable) from execution revelation (S2). No valid transaction exists until S1 is revealed, which happens only after PQSEC approval and immediately before broadcast. This reduces the quantum attack window from signing-to-confirmation to broadcast-to-confirmation.

PQEH does not provide full post-quantum immunity (a Bitcoin consensus limitation). It provides state-of-the-art denial-of-pre-construction within current consensus.

**Specification:** PQEH v2.1.1

### 3.8 PQAI — AI Identity and Drift

**Problem:** AI systems cannot be trusted to self-assert safety or permission.

**Solution:** Externalized behavioural verification through inspectable artefacts.

PQAI defines model identity binding, behavioural fingerprinting, drift detection, and SafePrompt consent binding. Models cannot self-classify their action authority. PQSEC gates AI operations based on PQAI artefacts.

**Specification:** PQAI v1.1.1

### 3.9 Neural Lock — Human State Attestation (Extension)

**Problem:** Coercion attacks succeed because keys equal authority.

**Solution:** Operator state as an additional predicate dimension.

Neural Lock produces attestations about human cognitive/physiological state (NORMAL, STRESSED, DURESS, IMPAIRED). It does not authorize or sign transactions—it provides evidence that PQSEC can use to gate high-risk operations.

Neural Lock is optional and deployment-specific.

**Specification:** Neural Lock v1.0.0

---

## 4. What PQ Eliminates

PQ structurally eliminates the following failure classes:

| Failure Class | Eliminated By |
|---------------|---------------|
| Replay attacks | Epoch Clock ticks + single-use binding |
| Time forgery | Bitcoin-anchored, threshold-signed time |
| Silent runtime compromise | PQVL attestation + PQSEC drift gating |
| AI behavioural drift | PQAI fingerprinting + drift classification |
| Consent reuse | Session-bound, single-use ConsentProof |
| Execution-gap attacks | ZET boundary + PQEH S1/S2 pattern |
| Key-equals-authority | PQHD predicate composition |
| Distributed enforcement bypass | PQSEC consolidation |

These are structural guarantees, not probabilistic mitigations.

---

## 5. What PQ Does Not Define

PQ explicitly does NOT define:

* Identity federation or SSO protocols
* OAuth, JWT, SAML, or X.509 compatibility
* Transport-layer authorization
* Optimistic execution models
* Heuristic or probabilistic enforcement
* Implicit trust assumptions
* Privacy or anonymity guarantees
* Censorship resistance
* Miner behaviour or mempool strategy

These are either out of scope or explicitly rejected as incompatible with PQ’s security model.

* **Emergency Revocation and Kill-Switches:**  
  PQ does not define emergency revocation or “kill switch” orchestration at the ecosystem level.  
  Revocation semantics, including identity or session invalidation under compromise, are expected to be defined by producing specifications and enforced by PQSEC through existing refusal, lockout, and monotonicity guarantees.

* **Hardware-Rooted Attestation:**  
  PQ does not define manufacturer trust anchors, hardware roots of trust, or device-specific measurement grammars (e.g., TPM, SGX, TEE).  
  Where hardware attestation is required, it MUST be provided by an external producing specification and consumed as evidence by PQSEC. PQ intentionally avoids embedding vendor- or jurisdiction-specific trust assumptions into the core ecosystem.

* **Social Recovery Orchestration:**  
  While PQ supports multi-signature custody models, guardian quorums, and recovery delays via PQHD and PQSEC, it does not define the user-experience, communication, or coordination protocols for social recovery.  
  Recovery orchestration is the responsibility of the implementing wallet or custody service.

---

## 6. Conformance

### 6.1 Ecosystem Conformance

An implementation claiming **PQ ecosystem conformance** MUST:

1. Delegate all enforcement to PQSEC.
2. Use Epoch Clock ticks for all time references.
3. Use PQSF canonical encoding for all signed or hashed artefacts.
4. Treat no artefact as authoritative until PQSEC evaluation.
5. Fail closed on any ambiguity, missing input, or verification failure.

Ecosystem conformance asserts that enforcement authority is centralized,
deterministic, refusal-only, and structurally consolidated within PQSEC.

---

### 6.2 Component Conformance

Each component specification within the PQ ecosystem defines its own
conformance requirements.

An implementation MAY be conformant to individual component specifications
without claiming PQ ecosystem conformance.

Component-level conformance does not imply enforcement correctness unless
all ecosystem conformance requirements are also satisfied.

---

### 6.3 Non-Conformance

The following patterns are explicitly non-conformant with the PQ ecosystem:

- Parallel enforcement logic outside PQSEC.
- Use of system clocks for authority, freshness, or expiry decisions.
- Non-canonical encoding of signed or hashed artefacts.
- Implicit trust in network identity, coordinator identity, or mirror identity.
- Degraded, heuristic, or best-effort modes for Authoritative operations.
- Model self-assertion of action class, permission, or authority.

Any implementation exhibiting these patterns MUST NOT claim PQ ecosystem
conformance.

---

## 7. Version Compatibility

### 7.0 Ecosystem Minimum Versions

Implementations claiming **PQ ecosystem conformance** MUST meet the minimum
specification versions below.

These minimums define the lowest versions at which the specifications are
considered mutually compatible at the ecosystem level. They do not replace
or override component-specific conformance requirements defined in individual
specifications.

| Specification | Minimum Version | Notes |
|---------------|-----------------|-------|
| Epoch Clock | ≥ 2.1.1 | Verifiable time artefacts only |
| PQSF | ≥ 2.0.2 | Canonical encoding and CryptoSuiteProfiles |
| PQSEC | ≥ 2.0.1 | Deterministic enforcement core |
| PQVL | ≥ 1.0.3 | Runtime attestation (when applicable) |

Implementations MAY evaluate using earlier versions for testing or research
purposes, but MUST NOT claim PQ ecosystem conformance while below the stated
minimum versions.

---

### 7.1 Current Versions

The following specification versions are aligned and implementation-ready
within the PQ ecosystem:

| Specification | Version | Status |
|---------------|---------|--------|
| PQ (this document) | 2.0.0 | Implementation Ready |
| Epoch Clock | 2.1.1 | Implementation Ready |
| PQSF | 2.0.2 | Implementation Ready |
| PQSEC | 2.0.2 | Implementation Ready |
| PQVL | 1.0.3 | Implementation Ready |
| PQHD | 1.1.0 | Implementation Ready |
| ZEB (includes ZET) | 1.2.0 | Implementation Ready |
| PQEH | 2.1.1 | Implementation Ready |
| PQAI | 1.1.1 | Implementation Ready |
| Neural Lock | 1.0.0 | Domain Evaluation Requested |

---

### 7.2 Deprecated Specifications

The following specifications are formally deprecated and MUST NOT be used
in new implementations:

| Specification | Status | Superseded By |
|---------------|--------|---------------|
| UDC (User-Defined Control) | DEPRECATED | PQAI + PQSEC |

---

## 8. Security Considerations

### 8.1 Threat Model

PQ assumes adversaries may:

* Compromise individual devices, coordinators, or mirrors
* Manipulate system clocks and network time
* Replay, reorder, or suppress messages
* Present stale or fabricated artefacts
* Possess future quantum computation capability
* Exploit ambiguity in encoding or representation
* Attempt model substitution or behavioural manipulation
* Coerce or impersonate legitimate operators

### 8.2 Trust Assumptions

PQ operates under minimal trust assumptions:

* Bitcoin blockchain consensus is honest majority
* Threshold signature schemes resist minority compromise
* Hash functions (SHA-256, SHAKE-256) are pre-image resistant
* Cryptographic verification is performed locally
* Canonical encoding eliminates representation ambiguity

PQ does NOT assume:

* Trusted system clocks
* Trusted networks or coordinators
* Trusted runtimes without attestation
* Honest model self-reporting
* Secrecy of classical key material (for post-quantum readiness)

### 8.3 Residual Risks

PQ does not protect against:

* Total compromise of all threshold signers
* Bitcoin consensus failure
* Post-broadcast quantum attacks (within current Bitcoin consensus)
* Long-term captivity with patient adversaries
* Miner censorship or transaction exclusion

These are acknowledged limitations, not specification failures.

---

## Annex A — Quick Reference: Predicates

The following predicates are evaluated by **PQSEC**.
This list is **informative only**; see **PQSEC** for normative definitions, evaluation rules, and enforcement semantics.

Predicates listed here **do not grant authority**.
They are evaluated exclusively by PQSEC according to the active enforcement configuration and policy.

| Predicate               | Evaluated From                                      |
| ----------------------- | --------------------------------------------------- |
| valid_structure         | PQSF canonical encoding                             |
| valid_tick              | Epoch Clock artefacts                               |
| valid_policy            | Policy bundles                                      |
| valid_runtime           | PQVL AttestationEnvelope                            |
| valid_consent           | ConsentProof artefacts                              |
| valid_quorum            | Custody quorum satisfaction                         |
| valid_ledger            | Ledger continuity                                   |
| valid_action_class      | PQAI-derived action classification evidence         |
| valid_model_identity    | PQAI ModelIdentity                                  |
| valid_drift             | PQAI drift classification                           |
| valid_delegation        | DelegationConstraint artefacts                      |
| valid_guardian_quorum   | Guardian approvals                                  |
| recovery_delay_elapsed  | Time since RecoveryIntent                           |
| safe_mode_active        | SafeModeState                                       |
| valid_payment_endpoint  | PaymentEndpointKey                                  |
| operator_state_ok       | Neural Lock attestation                             |
| valid_build_provenance  | BuildAttestation and related supply-chain artefacts |
| valid_runtime_signature | RuntimeSignature                                    |
| valid_publish_signature | PublishSignature                                    |
| valid_operation_key     | OperationKey                                        |
| valid_audit_chain       | AuditSignature and ledger continuity                |

---

### Annex A.1 Interpretation Boundary (Informative)

1. Predicates are **refusal-only signals**.
2. No predicate grants authority, permission, or execution capability.
3. Absence of a predicate requirement MUST NOT be interpreted as trust.
4. Supply-chain predicates are evaluated **only when explicitly required** by policy or enforcement configuration.
5. All enforcement, refusal, escalation, and lockout behaviour is defined exclusively by **PQSEC**.

This annex provides a reference map only.
Normative behaviour is defined by PQSEC.

---

## Annex B — Glossary

**Artefact** — A cryptographically signed, canonically encoded data structure produced by a PQ component.

**Authoritative Operation** — An operation with irreversible effects (signing, custody mutation, policy change). Requires PQSEC ALLOW outcome.

**Drift** — Measured deviation from baseline behaviour. Classified as NONE, WARNING, or CRITICAL.

**EnforcementOutcome** — The authoritative decision produced by PQSEC: ALLOW, DENY, or FAIL_CLOSED_LOCKED.

**Epoch Clock Tick** — A signed, monotonic time artefact anchored to Bitcoin.

**Execution Gap** — The dangerous period when executable artefacts exist before authorization completes.

**Fail-Closed** — Security posture where uncertainty results in refusal rather than permission.

**Non-Authoritative Operation** — A read-only operation with no irreversible effects.

**Predicate** — A boolean condition that must be satisfied for an operation to proceed.

**Refusal-Only** — Enforcement model where the engine only refuses; it never grants authority.

**S1/S2 Pattern** — PQEH revelation pattern separating commitment (S1) from execution capability (S2).

---

## Annex C — Proof of Ignorance for Dangerous Artefacts (Experimental)

**Status:** OPTIONAL  
**Maturity:** DOMAIN EVALUATION  
**Authority:** Evidence-only (non-authoritative)

### C.1 Purpose and Scope

This annex defines an experimental, evidence-only protocol for producing a
Proof of Ignorance (PoI): a cryptographic artefact asserting that a classified
dangerous input was handled within a constrained execution boundary, without
the artefact becoming externally available, and that a defined neutralisation
procedure was completed.

This annex defines structure, validation rules, and evidentiary boundaries only.  
All enforcement, refusal, escalation, and policy interpretation are defined
exclusively by PQSEC.

This annex introduces no authority, no allow semantics, and no mandatory
behaviour.

### C.2 Authority and Safety Boundary

1. Proof of Ignorance MUST NOT be interpreted as:
   - proof of global deletion,
   - proof of human non-awareness,
   - proof of legal or regulatory compliance,
   - proof of moral correctness, or
   - proof of absolute containment.
2. Proof of Ignorance MUST NOT grant permission, capability, or execution rights.
3. Proof of Ignorance MUST NOT override refusal, lockout, or policy constraints.
4. Absence, invalidity, or expiry of Proof of Ignorance MUST evaluate to UNAVAILABLE
   when consumed as predicate evidence by PQSEC.
5. No construct in this annex may emit ALLOW semantics.

Proof of Ignorance is conditional evidence only, produced under explicit
assumptions.

### C.3 Definitions

| Term | Definition |
|---|---|
| Dangerous Artefact | Information classified as potentially enabling catastrophic harm |
| Proof of Ignorance (PoI) | Evidence that neutralisation occurred without artefact export |
| Neutralisation | Deterministic destruction rendering artefact unrecoverable |
| AD_MODE | Ignorance-preserving execution mode |
| Secure Execution Boundary | Runtime boundary attested externally (e.g. PQVL) |
| Context Hash | Hash binding PoI to canonical context |

### C.4 Protocol States (Informative)

| State | Description |
|---|---|
| NORMAL | Default operation |
| AD_MODE | Ignorance-preserving handling |
| NEUTRALISED | Artefact neutralised |
| REFUSED | Operation blocked |

### C.5 Proof of Ignorance Artefact

```cddl
ProofOfIgnorance = {
  poi_version: uint,
  event_id: tstr,
  issued_tick: uint,
  epoch_tick_hash: bstr,
  context_hash: bstr,
  action: "neutralised" / "refused",
  drift_class: "NONE" / "WARNING" / "CRITICAL",
  proof_payload: bstr,
  suite_profile: tstr,
  signature: bstr
}
```

### C.6 Signature Computation

Signing and verification follow PQSF canonical CBOR rules with signature
omission.

### C.7 Validation Rules

ProofOfIgnorance is valid if and only if:

* canonical encoding is valid (PQSF)
* poi_version == 1
* signature is valid under suite_profile
* issued_tick is valid under the active time model
* context_hash is present
* action == "neutralised" implies drift_class == "NONE"
* no unknown fields are present

Validation failures MUST be treated as UNAVAILABLE when consumed as predicate
evidence by PQSEC.

### C.8 Assumptions and Limits

PoI correctness is conditional on documented assumptions including:

* valid runtime attestation (for example via PQVL), when required by policy
* deterministic neutralisation
* proof generated post-neutralisation

### C.9 Integration Guidance (Informative)

PQAI → PQSEC → AD_MODE → PQVL → Neutralisation → PoI → PQSEC

### C.10 Non-Goals

This annex does not define:

* censorship
* compliance claims
* cognition claims
* enforcement logic

### C.11 Conformance Statement

Support MAY be claimed if:

* ProofOfIgnorance is produced canonically
* enforcement remains delegated exclusively to PQSEC
* assumptions and operating limits are documented

---

## Changelog

### Version 2.0.0 (Current)

* **Enforcement Centralization:** Centralized all enforcement logic and authority decisions into a single deterministic core: PQSEC.
* **Scope Expansion:** Shifted from "PQ-ready" cryptography to addressing modern execution-gap exploits, including replay, time forgery, and consent reuse.
* **Structural Decoupling:** Redefined the relationship between modules (Clock, VL, AI) such that no component grants authority in isolation; they now provide verifiable predicates for the core enforcement layer.
* **Deprecation Management:** Formally retired the UDC specification and migrated its normative functions into PQAI and PQSEC.

---

## Acknowledgements

The PQ ecosystem builds on decades of work in cryptography, distributed systems, protocol design, and adversarial security analysis.

### Foundational Contributions

* **Satoshi Nakamoto** — for Bitcoin's trust-minimised consensus model
* **Whitfield Diffie and Martin Hellman** — for public-key cryptography
* **Ralph Merkle** — for Merkle trees and tamper-evident structures
* **Daniel J. Bernstein** — for cryptographic engineering and constant-time design
* **The NIST Post-Quantum Cryptography Project** — for standardising post-quantum primitives

### Protocol and Systems Influences

* **The IETF CBOR, COSE, and TLS working groups** — for canonical encoding and session binding primitives
* **Bitcoin Core developers and BIP contributors** — for PSBT, Taproot, and script semantics
* **Zero-trust architecture researchers** — for refusal-based security models
* **Byzantine fault tolerance researchers** — for threshold and quorum patterns

### AI Safety Contributions

* **Stuart Russell, Paul Christiano, and AI alignment researchers** — for externalised oversight models
* **Anthropic, OpenAI, and model evaluation researchers** — for behavioural analysis frameworks

### Specification Development

This ecosystem was developed through extensive collaboration with AI systems, demonstrating that human-AI partnership can produce rigorous, auditable security specifications. The architectural patterns, authority boundaries, and fail-closed semantics emerged from iterative refinement across hundreds of review cycles.

Any errors or omissions remain the responsibility of the author.

---

If you find this work useful and want to support continued development:

**Bitcoin:**  
bc1q380874ggwuavgldrsyqzzn9zmvvldkrs8aygkw
