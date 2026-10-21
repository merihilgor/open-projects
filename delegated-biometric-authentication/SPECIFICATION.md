Türkçe: [SPECIFICATION.tr.md](SPECIFICATION.tr.md)

# Delegated Biometric Authentication: Draft Technical Specification (DBA-TS v0.1)

> **Draft, not a published standard.** This is a working draft of the specification that the [standardization note](STANDARDIZATION.md) proposes should be written. Requirements use **"shall"**, recommendations **"should"** and permissions **"may"**. It has not been through any standards body's process, and it is not a conformance claim. Commercial implementations are subject to the [commercial license](COMMERCIAL-LICENSE.md).

> **On the open cryptographic core.** Section 8 — the delegated presence check — is the heart of this specification and the one part that is **not solved**. It states the requirements precisely, sets out four candidate mechanisms with their properties and trade-offs, and gives the evaluation criteria for choosing between them. A specification that pretended this part was settled would be worthless; a specification that states it clearly is the most useful thing that can be written today.

## 1. Scope

This specification defines a protocol by which a person may authenticate to a third-party reader — a point-of-sale terminal, an access-control gate, a transit turnstile, a reception desk — using a biometric check performed on their own device, **with the device absent from the reader's location**, such that:

- the biometric template **never leaves the device** that holds it;
- the reader **verifies a short-lived signed proof** and never receives a template, a reusable biometric identifier, or a reusable image;
- the delegation is **granted personally by the person**, per reader, per purpose, **time-limited** and **revocable**;
- the reader **distinguishes a live presentation from a presentation attack**, to a published rate;
- the proof **cannot be replayed** at another reader, for another purpose, or after the delegation has been withdrawn.

**In scope:** the delegation grant object; the two verification modes; the delegated presence check; the proof format and its bindings; the status and revocation mechanism; the privacy modes; the reader conformance and certification requirements; the cryptographic profile, including post-quantum requirements.

**Out of scope:** how a template is enrolled, protected inside a secure element, or matched *inside the device* — those are the device platform's business and the subject of existing standards. The one partial exception is Section 8, which considers mechanisms in which the *reader* matches against a transformed reference rather than the template; how that match is performed is left to the selected mechanism and to ISO/IEC 24745, and Section 8 states the requirements the result must meet rather than the arithmetic. Also out of scope: how a reader's camera captures an image and performs presentation-attack detection — that is ISO/IEC 30107 and ISO/IEC 19795; the legal effect of a successful verification; which payments or transactions are permitted to use it; and the commercial terms of any deployment.

**Law-neutral.** This specification says what a conforming implementation shall do. It does not create legal rights, and it does not choose a legal basis for processing. Where a delegation is used to satisfy a statutory requirement, that requirement is set by law and this specification only says the flow conforms to its terms.

## 2. Normative references

The following are referenced in a manner that makes their content part of this specification. For dated references, only the edition cited applies.

| Reference | Relevance |
|---|---|
| ISO/IEC 24745:2022 | Biometric information protection: irreversibility, unlinkability, renewability, revocability; application models including the distributed comparison models; the framework within which a renewable biometric reference is defined |
| ISO/IEC 27553-1:2022 | Security and privacy requirements for biometric authentication on mobile devices in local modes; the requirement that biometric data and derived data, except the verification outcome, do not leave the device |
| ISO/IEC 27553-2:2025 | The same, in remote modes |
| ISO/IEC 30107-1:2023, -2:2017, -3:2023, -4:2024 | Presentation attack detection: framework, data formats, testing and reporting, and the mobile-device test profile |
| ISO/IEC 39794-1 and the applicable modality parts | Biometric data interchange formats, where a biometric sample is carried in a proof or an enrolment record |
| ISO/IEC 19795-1:2021, -10:2024 | Testing of biometric performance and of performance variation across demographic groups |
| NIST SP 800-63-4 | Identity proofing, authentication and federation; in particular §3.11 (biometrics) and its remote presentation-attack requirements and thresholds |
| RFC 5280 | Certificate revocation lists, where a status mechanism uses them |
| RFC 9576, 9577, 9578 | The Privacy Pass architecture, HTTP authentication scheme, and issuance protocols, where the unlinkable mode is used |
| NIST FIPS 203, 204, 205 | ML-KEM, ML-DSA and SLH-DSA, where the post-quantum profile is used |
| The applicable IETF TLS specifications | Where a reader-broker session is protected |
| ISO/IEC 25456 (injection attack detection, expected 2027) | Detection of digital injection attacks, completing the reader's threat model alongside presentation attack and deepfake detection |
| ISO/IEC 25049-1/-2 (Customizable biometric web services) | Where a delegated reader is implemented as a biometric web service; a conforming implementation **should** interoperate with this work rather than duplicate it |
| ZCAP (W3C Community Group) | The capability model for delegation chains, attenuation, expiry and offline validation that Section 5.3 builds the grant on |

*Normative references are to be finalised per body in the route described in the [standardization note](STANDARDIZATION.md#part-6-licensing-rights-and-route). Items marked as composite or hybrid in Section 11 are in RFC Editor review at the time of writing and shall be cited at their final designations.*

## 3. Terms and definitions

| Term | Definition |
|---|---|
| **Subject** | The natural person who may exercise a delegation. Referred to by a pseudonym; the specification never requires the subject to be identified to a reader. |
| **Delegating broker** | The party that holds the subject's biometric template inside secure hardware, holds a device key, and is the only party able to create or revoke a grant. In practice: the device operating system, or a wallet application running on it. |
| **Reader** | The device at which the subject is physically present: a terminal, a gate, a kiosk. It captures a presentation, performs presentation-attack detection, issues challenges and verifies proofs. |
| **Operator** | The organisation that operates readers and receives verification results: a bank, a building manager, a transport authority. |
| **Relying party** | The service that consumes a verification result and takes a decision on it. May be the operator, or a third party to which the operator forwards the result. |
| **Trust service** | The neutral party that holds the public trust anchors, registers grants, and publishes status and revocation information. It holds no biometric data. |
| **Delegation grant** | The object that records a subject's personal, scoped, time-limited and revocable authorisation of a named reader to perform a delegated presence check for a named purpose. |
| **Delegation proof** | The short-lived signed statement a reader verifies. It carries no biometric data and no reusable identifier. |
| **Delegated presence check** | The act of establishing, at a reader, that the person physically present is the subject of a named grant. Section 8. |
| **Presentation** | The biometric sample captured by a reader at the moment of a check. It shall be discarded after the check. |
| **Presentation attack** | A presentation to a biometric sensor that is not a bona fide presentation by the subject. |
| **Reader class** | A named group of readers, identified by a trust service, that a single grant may cover — for example "all gates of building Y". |
| **Attribute disclosure** | A statement the reader learns in addition to the presence result: an identity reference, an age band, or nothing beyond "authorised". |
| **Status** | The current state of a grant as published by the trust service: active, revoked, expired, or suspended. |

## 4. Roles, trust and threat model

### 4.1 What each party holds

| Party | Holds | Shall not hold |
|---|---|---|
| Subject's device | The biometric template, in secure hardware; the device key | — |
| Delegating broker | The template reference, the device key, the grant list, the signing key | Anything that leaves the device in a form that identifies the subject to a reader |
| Reader | A public key, the trust anchors, a cached status, the verification outcome | A template, a reusable biometric identifier, a retained presentation, a log that can be joined into a movement profile |
| Operator | A delegation identifier, an event record, a payment or access record | A template, a reusable subject identifier beyond what the purpose requires |
| Trust service | Public keys, grant identifiers, status, revocation list | Any biometric data, and any reference to a person's face |

### 4.2 Trust assumptions

- The **device's secure hardware** is assumed to resist physical and software attack to the level certified for the platform. A grant inherits the platform's assurance level; it does not raise it.
- The **reader is assumed honest but not trusted with privacy**: it is treated as an observer of the subject, not as a holder of their biometric.
- The **trust service is assumed to be correct and available, but not trusted with privacy** — it holds keys and status, and an attacker who compromises it obtains no biometric data.
- The **operator is assumed to be honest in what it records**, and is subject to the audit in Section 12.

### 4.3 Adversaries and required properties

| # | Adversary | Required property |
|---|---|---|
| A1 | Presents a photograph, a mask, a screen replay or a printed image | The reader detects the attack at a published rate per attack class (Section 9) |
| A1b | Injects a digital or synthetic presentation rather than presenting one physically | The reader detects injection attacks, to the extent the capture device and the platform can; a reader that cannot is **not** certified for a check whose threat model includes injection, and the deployment **shall** say so |
| A2 | Records a valid proof and replays it at the same reader | The proof is single-use within its validity window; the reader rejects a consumed proof |
| A3 | Records a valid proof and presents it at a **different** reader | The proof is bound to a reader identifier; a reader rejects a proof naming another reader |
| A4 | Obtains a proof for **one purpose** and uses it for another | The proof is bound to a purpose; a reader rejects a proof naming another purpose |
| A5 | Uses a grant after the subject withdrew it | Revocation reaches the reader within the bound in Section 10; the reader fails closed on a stale status |
| A6 | Steals the device and coerces the subject's biometric | The broker requires the subject's own biometric to exercise a grant; the platform's anti-coercion and stolen-device protections apply; grants are visible to the subject |
| A7 | A malicious application requests a grant | Grants are created only by the operating system's security subsystem; an application can request but not create; a request for a scope the user has not authorised is refused |
| A8 | The operator builds a shadow biometric database | The audit in Section 12 detects retained presentations, reusable identifiers, and joined logs; the operator's certification is withdrawn |
| A9 | An attacker breaks the classical signature **later**, and replays cached proofs | The cryptographic profile in Section 11 uses a post-quantum-capable signature, and grant lifetimes are bounded below the migration horizon |
| A10 | A subject's grant is inherited by whoever obtains the device | A grant is bound to the device key; losing the device revokes its grants (Section 10.4) |

## 5. The delegation grant

### 5.1 Fields

A delegation grant **shall** carry the following:

| Field | Requirement |
|---|---|
| `grant_id` | **Shall** be unique and **shall** never be reused |
| `delegatee_ref` | **Shall** identify the delegating broker by a stable pseudonym, not by the subject |
| `subject_ref` | **Shall** identify the subject by a pseudonym. **Shall not** be a directly identifying value |
| `reader_scope` | **Shall** name a single reader or a named reader class. **Shall not** be a service category |
| `purpose` | **Shall** be one of a registered purpose: payment, access, age verification, attendance, identity confirmation. **Shall not** be empty |
| `attributes` | **Shall** list what the reader may learn. **Shall** be one of: no disclosure, an age band, a scoped identity reference |
| `not_before`, `not_after` | **Shall** be present. A grant **shall not** have an unbounded `not_after` unless the subject has explicitly chosen a standing grant, which **shall** be surfaced to the subject as standing |
| `max_uses` | **May** be present. If present, the grant **shall** be invalid once the count is reached |
| `proof_key` | **Shall** be the public key of the key that signs proofs for this grant, or a reference to it in the trust service |
| `attestation` | **Shall** be a device attestation binding the delegating broker's key to the platform's assurance level |

### 5.2 Rules on scoping

5.2.1 A grant **shall** be exercisable only at a reader within its `reader_scope`, and only for its `purpose`.
5.2.2 A grant **shall not** be exercisable for a purpose other than the one named, even by the same reader, even for the same operator.
5.2.3 Where the purpose is payment, the grant **may** be further bound to a transaction, an amount and a merchant, and a reader **shall** reject a proof that exceeds such a binding.
5.2.4 A grant **shall not** be created by an application. The subject's operating system is the only party able to create one.
5.2.5 The subject **shall** be able to see, at any time, every grant that exists for their device, and every use of one.

### 5.3 The grant as a capability

5.3.1 A delegation grant **shall** be modelled as a **capability**: an unforgeable assertion of authority, held by the broker, delegable, attenuable, and expiring — the model developed in the W3C Community Group's ZCAP work and already in use for delegated authority in the OAuth and WIMSE working groups. A conforming implementation **should** represent a grant in that form rather than defining a parallel mechanism.

5.3.2 A grant **shall** be a capability chain: the broker holds a root capability representing the subject's authority to delegate, and each grant is a child of it, with a `parentCapability` link. A reader **shall** verify the chain to the root before accepting a proof, and **shall** reject a chain that is broken, that is not anchored in a trust service's registry, or that has been revoked anywhere along it.

5.3.3 A grant **shall** carry an **expiry**, and an expired grant **shall** be rejected without exception. A grant with no expiry **shall not** conform.

5.3.4 A grant **may** be **attenuated** — narrowed relative to its parent — and a reader **shall** verify that the attenuation has not widened the authority: the scope, purpose, attributes and lifetime of a child **shall** each be a subset of its parent's.

5.3.5 The chain **shall** be verifiable **offline**, against published trust anchors, so that a Mode B reader **shall not** need to contact the trust service to validate the chain itself. Contact is needed only for status, and only within the bound of Section 10.3.

5.3.6 A capability representation **shall not** weaken any requirement of this specification. In particular, a proof that is a valid capability **shall still** satisfy Section 6 — the reader binding, the purpose binding, the freshness and the single-use rule are properties of *this* protocol and are not supplied by the capability machinery.

5.3.7 The capability model supplies the **authority** half of a delegated check. The **presence** half — proving that the person in front of the reader is the person the grant names — is not supplied by it, and remains the open question of Section 8.

### 5.4 The consent ceremony

5.4.1 The operating system **shall** present, at the moment of grant creation: the reader the grant would cover, the purpose, the attributes the reader would learn, the lifetime, and the consequence of the grant being a standing one.
5.4.2 The consent ceremony **shall** be initiated by an explicit user action, and **shall not** be inferable from a wider permission.
5.4.3 Consent **shall** be per grant. A grant covering several readers of one class **shall** be presented as such, naming the class and its size where known.
5.4.4 A grant **shall not** be created by default, by default-on, or as a side effect of another grant.

## 6. The delegation proof

### 6.1 Fields

A delegation proof **shall** carry:

| Field | Requirement |
|---|---|
| `grant_id` | The grant the proof exercises |
| `reader_id` | **Shall** identify the reader the proof is bound to |
| `purpose` | **Shall** be the purpose the proof is bound to |
| `challenge` | **Shall** be the reader's fresh challenge the proof answers |
| `presence_result` | The result of the delegated presence check (Section 8) |
| `attributes` | Only those permitted by the grant |
| `issued_at`, `expires_at` | The validity window. **Shall** be short: a proof's lifetime **shall not** exceed the revocation bound in Section 10.3 |
| `counter` | The use count, where the grant is use-limited |
| `signature` | A signature over the canonical encoding of the preceding fields, by the grant's proof key |

### 6.2 Binding rules

6.2.1 A reader **shall** reject a proof whose `reader_id` does not match its own identifier.
6.2.2 A reader **shall** reject a proof whose `purpose` does not match the operation it is performing.
6.2.3 A reader **shall** reject a proof whose `challenge` does not match a challenge it issued and has not already consumed.
6.2.4 A reader **shall** reject a proof outside its validity window, using the reader's own clock, which **shall** be within the tolerance stated in Section 9.4.
6.2.5 A reader **shall** reject a proof whose signature does not verify under the key registered for the grant, or whose grant is not published in the trust service.
6.2.6 A reader **shall** reject a proof whose `attributes` exceed the grant's permitted attributes, and **shall** ignore any attribute not permitted rather than acting on it.

### 6.3 What a proof must not contain

6.3.1 A proof **shall not** contain a biometric template, a reference image, an embedding, a feature vector, or any value from which one can be derived.
6.3.2 A proof **shall not** contain a subject identifier that is stable across readers.
6.3.3 A proof **shall not** contain the subject's personal data beyond the `attributes` the grant permits.
6.3.4 A proof **shall** be a one-time artefact: a reader **shall** record the identifier of every proof it accepts, for the length of the proof's validity window, and **shall** reject a repeated one.

## 7. Verification modes

### 7.1 Mode A — online, broker-mediated

The broker is reachable and performs the delegated presence check itself.

```
Reader                          Broker (device)                Trust service
  |                                  |                             |
  |-- CHALLENGE(reader_id, purpose,  |                             |
  |   nonce, ts) ------------------>|                             |
  |                                  |-- STATUS(grant_id) ------>|
  |                                  |<-- status ----------------|
  |                                  |                             |
  |   (subject's own biometric       |                             |
  |    check inside the device)      |                             |
  |                                  |                             |
  |<-- Delegation Proof --------------|                             |
  |   (verify signature, bindings,   |                             |
  |    presence result)              |                             |
  |                                  |-- USE(grant_id) ---------->|
```

7.1.1 The reader **shall** issue a fresh, unpredictable challenge for every attempt, of at least 128 bits of entropy.
7.1.2 The broker **shall** refuse to answer a challenge older than a configurable maximum age, and **shall not** answer the same challenge twice.
7.1.3 The broker **shall** answer only after the subject has been verified by the device's own biometric, and **shall** record which grant it exercised.
7.1.4 The reader **shall** fail closed: if the broker is unreachable, the check **shall** fail. It **shall not** fall back to an offline proof unless the grant is issued in Mode B.

### 7.2 Mode B — offline, pre-issued proof

The broker pre-issues a short-lived proof; the reader verifies it against cached status and published trust anchors, with no network call to the broker.

7.2.1 A Mode B proof **shall** satisfy every requirement in Section 6.
7.2.2 A Mode B proof's validity window **shall not** exceed the revocation bound in Section 10.3.
7.2.3 The reader **shall** verify against a status no older than the bound in Section 10.3, and **shall** fail closed if it has none.
7.2.4 A reader operating in Mode B **shall** display, or otherwise record, the age of the status it verified against, so that the operator can report the revocation exposure it accepted.

### 7.3 Choosing a mode

7.3.1 The mode **shall** be a property of the grant and **shall** be visible to the subject, because it determines what "withdrawn" means in practice.
7.3.2 A grant **shall not** be usable in both modes simultaneously, because a Mode A proof and a Mode B proof of the same grant would have different replay properties.

## 8. The delegated presence check

**This section is the unsolved core of the specification.** It states what shall be achieved, sets out the candidate mechanisms, and gives the criteria for choosing between them.

### 8.1 Requirements

A delegated presence check **shall**:

- **P1 — Presence.** Establish that the person physically present at the reader is the subject of the named grant.
- **P2 — No disclosure.** Reveal no template, no reference image, no feature vector, and no identifier that would let the reader recognise the subject on a later visit.
- **P3 — Freshness.** Be bound to the reader's current challenge, so that a captured check cannot be replayed.
- **P4 — Liveness.** Be based on a presentation that passed a presentation-attack check meeting Section 9.
- **P5 — Measurable.** Have a published false-accept rate per attack class, and a published false-reject rate.
- **P6 — Offline-capable.** In Mode B, complete with no network access to the broker.

### 8.2 Why the usual answer does not apply

A conventional biometric check answers "does this sample match this template". Here the template is on a device that is not present, and the sample is at a reader that does not hold the template. The two halves are in different places, which is the entire difficulty, and it is why every existing standard that solves the problem does so by requiring the device to be present.

### 8.3 Candidate mechanisms

**M1 — Fuzzy vault / secret sharing.** The broker transforms the template into a *fuzzy commitment* (a set of points mixed with chaff) that locks a secret. The reader captures a live sample, finds the matching subset, reconstructs the secret by interpolation, and proves knowledge of it.

- *Privacy:* strong. The reader learns only "the vault opened".
- *Cost:* the reader performs a full match against a fuzzy structure; error rates are tunable but the structure leaks a little information about the template through its chaff distribution, which must be analysed.
- *Revocation:* the vault can be rotated, but a captured vault is a captured vault until it is retired.
- *Status:* the mechanism is **already an ISO-sanctioned technology example** — ISO/IEC 24745:2022 Annex C, "Technology examples for biometric information protection", lists the fuzzy vault in Table C.1 as a concrete way to construct a pseudonymous identifier with auxiliary data (term 3.35). The mechanism is also the subject of a **granted Visa patent family** ("Delegated biometric authentication"). An implementer **shall** obtain advice before using it.

**M2 — Renewable and revocable transformed reference with secure matching.** The broker issues a *renewable biometric reference* — a transformed, protected representation that the reader can match against directly — in the sense of ISO/IEC 24745, including its distributed comparison models. A fresh reference is derived for each grant, so a grant is revoked by simply ceasing to use its reference.

- *Privacy:* depends entirely on the transformation and on per-reader diversification; the specification **shall** require that references for different grants be unlinkable, and that a reference not be invertible to the template.
- *Cost:* the reader matches against a transformed reference, so the error rate is the transformation's error rate, and it is typically worse than a direct match; the reference is larger than a template.
- *Revocation:* **the strongest of the four**, because revocation is intrinsic: the reference stops working.
- *Status:* this is the family ISO/IEC 24745 already governs, which makes it the natural home for an ISO landing zone. Notably, **ISO/IEC 24745 never uses the word "delegation"** — its Clause 8 models A–K describe only *where a reference is stored and where the comparison happens*, and the three models added in 2022 (I, J, K) are the **distributed comparison** models, in which the comparison is not co-located with the stored reference. The standard supplies the substrate and is silent on the question this specification is about.

**M3 — Broker-mediated match with a transient sample (Mode A only).** The reader transmits the live sample to the broker, which matches it against the template and returns the signed proof.

- *Privacy:* the weakest. The template stays on the device, but a live biometric sample transits a network and reaches a server.
- *Cost:* simplest to build; no new mathematics.
- *Revocation:* strong, because the broker can refuse to answer.
- *Status:* available today with existing components. It is the fallback, and a specification that only permitted it would deliver the privacy promise of the proposal only partially.

**M4 — Zero-knowledge match over a transformed reference.** As M2, but the reader additionally proves to a verifier that a match occurred, **without revealing the reference or the sample** — a zero-knowledge proof of proximity between the sample and the reference, with the reference committed at enrolment.

- *Privacy:* the strongest of the four, and the only one that lets the reader show a result to a third party without showing the evidence.
- *Cost:* substantial. It requires a proof circuit over the matching function, and the matching function is usually not a clean arithmetic relation, which is why real implementations use a "fuzzy" match inside a proof or a general-purpose ZK machine.
- *Revocation:* as M2.
- *Status:* the direction the W3C VC Confidence Methods draft has already taken with its `BiometricVectorConfidenceMethod` and client-side zero-knowledge match flow. Building on that draft is cheaper than starting over.

### 8.4 Evaluation criteria

The choice between the candidates **shall** be decided by measurement against published criteria, and the standard **shall not** name a single mandatory mechanism until that measurement exists.

| Criterion | M1 | M2 | M3 | M4 |
|---|---|---|---|---|
| P2 — no disclosure at the reader | Strong | Depends on transformation | Weak (sample transits) | Strongest |
| P4 — liveness | Independent of the mechanism | Independent | Independent | Independent |
| P5 — measurable false-accept rate | Tunable | Transformation-dependent | Best (direct match) | Transformation-dependent |
| P6 — offline capable | Yes | Yes | **No** | Yes |
| Revocation | Rotation | **Intrinsic** | Strong | Intrinsic |
| Patent freedom | **Constrained** | Free | Free | Free |
| Implementation cost | High | Medium | Low | High |
| Standardisation home | Patents | **ISO/IEC 24745** | Existing components | W3C VC |

### 8.5 Requirement on the mechanism, whatever it is

8.5.1 Whatever mechanism is selected, the delegated presence check **shall** be bound to the reader's challenge and **shall** be single-use.
8.5.2 It **shall** be evaluated against the presentation-attack species of ISO/IEC 30107-3 Annex A, and **shall** publish a false-accept rate for each.
8.5.3 It **shall not** permit the reader to derive a value that identifies the subject across grants. Where a mechanism cannot guarantee this, it **shall** be restricted to the no-attribute-disclosure mode.
8.5.4 A **conformance test suite** **shall** exist for the selected mechanism before any implementation claims conformance, and **shall** include replay, cross-reader and cross-purpose attack cases.

8.5.5 Where the selected mechanism places a transformed reference at the reader, the requirements of Section 12.1.2 **shall** apply in full, and a reader that cannot meet them **shall not** hold that reference — it **shall** fall back to a mode in which the reader holds no biometric-derived value at all.

## 9. Presentation attack detection

### 9.1 Where the check happens

The check **shall** be performed by the reader, on the presentation it captures. It **shall not** be delegated to the operator's server as a precondition of the check succeeding, because a server-side check that requires the phone would reintroduce the presence requirement.

### 9.2 Requirements

9.2.1 The reader's presentation-attack detection **shall** conform to ISO/IEC 30107-1, -2 and -3, and the reader **shall** be tested under the mobile-device profile of ISO/IEC 30107-4 where the capture device is a mobile device.

9.2.1a The reader's threat model **shall** distinguish three attack families, because a single liveness test does not cover them: **presentation attacks** (ISO/IEC 30107), **deepfake and synthetic attacks** (ISO/IEC 26655), and **digital injection attacks** (ISO/IEC 25456). A reader claiming only the first **shall** state that limitation in its certification, and a deployment **shall not** represent a reader's presentation-attack performance as evidence against the other two.
9.2.2 The reader **shall** publish a false-accept rate for each attack species in ISO/IEC 30107-3 Annex A that it claims to resist, measured under ISO/IEC 19795-1.
9.2.3 The reader **shall** report demographic variation in its error rates, measured under ISO/IEC 19795-10, and **shall not** be more than 25% worse for any demographic group than for the population as a whole — the threshold NIST SP 800-63-4 applies.
9.2.4 Where the reader is used in a remote, non-interactive setting, its presentation-attack performance **shall** meet the requirements NIST SP 800-63-4 §3.11 sets for remote use.
9.2.5 The reader **shall not** retain the presentation. It **shall** be held only for the duration of the check and **shall** be discarded, and the implementation **shall** be capable of demonstrating this to an auditor.
9.2.6 The reader **shall not** use the presentation for any purpose other than the check, including model training.

### 9.3 Degraded operation

9.3.1 A reader that cannot perform a compliant presentation-attack check **shall** not be certified for a delegation check, and the grant **shall** say so where the reader is known to be degraded.
9.3.2 A reader **shall not** silently downgrade to a weaker check. If the check cannot be performed, the verification **shall** fail.

### 9.4 Clock tolerance

9.4.1 A reader **shall** accept a proof whose validity window falls within a stated clock tolerance of its own clock. The tolerance **shall** be published in the reader's certification, and **shall not** exceed the proof's own validity window.
9.4.2 A reader **shall** monitor its clock synchronisation and **shall** report a reader whose clock has drifted beyond tolerance.

## 10. Status, revocation and expiry

### 10.1 The status mechanism

10.1.1 The trust service **shall** publish the status of every grant: active, revoked, expired or suspended.
10.1.2 The status **shall** be retrievable by a reader that is not in contact with the subject's device.
10.1.3 The status record **shall** contain no personal data. It **shall** identify a grant, not a person, and **shall not** permit the reader to derive a subject identity or to correlate a subject's grants with one another.
10.1.4 The status mechanism **shall** support a reader that is intermittently connected, by defining a maximum age for a cached status.

### 10.2 Revocation

10.2.1 The subject **shall** be able to revoke a grant at any time, from the same place the grant was created.
10.2.2 Revocation **shall** take effect at the next check at any online reader.
10.2.3 Revocation **shall not** delete the audit record of prior use. A revoked grant **shall** be retained as a record that it existed, and **shall** be marked as no longer exercisable.
10.2.4 A trust service **shall** be able to revoke a grant on the operator's request, for a documented reason — for example a suspected compromise — and **shall** publish the reason category.
10.2.5 A trust service **shall** be able to suspend a grant pending investigation, and **shall** distinguish a suspension from a revocation in the published status.

### 10.3 The offline revocation bound

10.3.1 A Mode B reader **shall** refuse a proof whose cached status is older than the maximum status age.
10.3.2 The maximum status age **shall** be published as part of the standard's profile, and **shall be set by measurement rather than by assumption** — a deployment that cannot meet it **shall** use Mode A.
10.3.3 An operator operating Mode B readers **shall** report, in its transparency disclosure, the maximum status age it actually enforces, and **shall** report every verification that succeeded against a status older than the target.
10.3.4 The bound **shall** be tight enough that the window in which a withdrawn grant can still open a door is **short enough to be explained to a subject in one sentence**. If it is not, the deployment is the wrong shape for Mode B.

### 10.4 Device loss

10.4.1 A device **shall** report the loss of its device key to the trust service, and the trust service **shall** revoke every grant bound to that key.
10.4.2 A grant **shall not** be transferable to another device. Where a subject moves to a new device, a new grant **shall** be created, and the old one revoked.

## 11. Cryptographic profile

### 11.1 Grant and proof signing

11.1.1 The grant **shall** be signed by the trust service or by the delegating broker under a key the trust service has registered, using a signature algorithm from the profile in 11.3.
11.1.2 The proof **shall** be signed by the grant's proof key.
11.1.3 Signatures **shall** cover the reader binding, the purpose binding, the challenge and the validity window. A signature that does not cover these **shall** be rejected by a reader.

### 11.2 Key establishment

11.2.1 The reader-to-broker session **shall** use the hybrid key exchange of the profile in 11.3.
11.2.2 A reader **shall not** use a session that is protected only by classical key exchange, where the profile is in force.

### 11.3 Post-quantum profile

11.3.1 The grant signature **shall** be a **composite** signature: a classical component and an ML-DSA component, presented as a single public key and a single signature under a single algorithm identifier, so that existing X.509, CMS and COSE stacks treat it as one atomic algorithm.

11.3.2 The composite signature **shall** be verified by checking both components. A reader **shall not** accept a grant on the strength of the classical component alone.

11.3.3 The reader-to-broker session **shall** use a hybrid key exchange combining a classical component with ML-KEM.

11.3.4 The profile **shall** note the security property of a composite signature: it provides existential unforgeability but **not** strong unforgeability. A reader **shall** therefore treat a re-signed grant as a **new** grant for the purposes of a use counter, and **shall not** count it as a replay.

11.3.5 A grant's cryptographic lifetime **shall not** exceed the post-quantum migration horizon, and a grant **shall** be re-issuable under a post-quantum key without the subject re-consenting, because its scope has not changed.

11.3.6 The long-term archival format for grant histories and audit records **shall** specify a post-quantum-secure hash.

11.3.7 Where the unlinkable mode of Section 13 is used, the implementation **shall** state the post-quantum security of its unlinkability primitive, and **shall not** claim post-quantum unlinkability it has not demonstrated.

### 11.4 Randomness

11.4.1 Challenges **shall** be generated by a cryptographically secure random number generator and **shall** be at least 128 bits.
11.4.2 The broker **shall not** answer a challenge twice, and **shall** persist enough state to enforce this across a restart.

## 12. Privacy, data minimisation and audit

### 12.1 The reader's storage

12.1.1 A reader **shall not** store the **enrolled template**, a reference image, or an unprotected feature vector.

12.1.2 **Where the selected mechanism places a transformed reference at the reader** — mechanism M2 or M4 of Section 8.3 — the reader **may** hold that reference, and **shall** meet all of the following:

- a. it **shall** be a *renewable biometric reference* within the meaning of ISO/IEC 24745, and **shall** meet that standard's requirements for irreversibility and unlinkability;
- b. it **shall** be **specific to a single grant**, so that references held at different readers for the same subject are unlinkable;
- c. it **shall** be **bound to the reader** that holds it, so that a reference cannot be lifted from one reader and used at another;
- d. it **shall** be replaced when the grant is re-issued, and **shall** cease to be usable when the grant is revoked;
- e. it **shall not** be derivable into the enrolled template by the holder of the reference.

12.1.3 A reader **shall not** store a stable identifier derived from the subject's biometric presentation, and **shall not** store a subject identifier that is stable across readers.
12.1.4 A reader **shall** store the identifier of every proof it accepts, for the length of that proof's validity window, for replay protection, and **shall** delete it afterwards.
12.1.5 A reader **shall not** store a log that can be joined to reconstruct the subject's movements. Where a reader must keep an event log, it **shall** be pseudonymous per reader and per period, and **shall not** be joinable with another reader's log or with the trust service's records.

### 12.2 The operator's record

12.2.1 The operator **shall** record the event — a delegated check succeeded, at this reader, at this time, under this grant — and **shall not** record the biometric.
12.2.2 Where the purpose requires an identity, the operator **shall** record the scoped identity reference from the grant, and **shall not** retain the grant's other fields beyond the operational need.
12.2.3 The operator **shall** not reconstruct a subject profile by joining its own log across readers for a subject. Where a lawful purpose requires longitudinal records, it **shall** be a declared purpose of the grant, and the grant **shall** say so.

### 12.3 Audit

12.3.1 An independent auditor **shall** be able to verify, from a reader's or operator's records, that no template or reusable identifier is retained, that grants are scoped as they claim, that grants expire, that Mode B readers respect the status-age bound, and that logs are not joined.
12.3.2 The trust service **shall** publish aggregate transparency figures: counts of grants, revocations, status queries, and verification outcomes, with no personal data.
12.3.3 The subject **shall** be able to obtain the record of every check performed under their grants, resolved to the pseudonymous subject reference, and **shall** be able to challenge an entry.

## 13. Privacy modes

### 13.1 The three modes

13.1.1 A grant **shall** be issued in exactly one of three modes, chosen by the purpose:

| Mode | The reader learns | Used for |
|---|---|---|
| **No disclosure** | Only that an authorised person is present | Access where identity is not needed |
| **Attribute** | A stated fact, without the identity behind it — for example an age band | Age verification |
| **Identity** | A scoped identity reference | Hotel check-in, clinic reception |

13.1.2 The mode **shall** be recorded in the grant and **shall** be visible to the subject.

### 13.2 Unlinkability

13.2.1 In no-disclosure mode, the reader **shall not** be able to correlate two checks. The implementation **shall** use a construction that provides this — an unlinkable token in the sense of the Privacy Pass issuance protocols, or a per-grant randomised reference with the same property.
13.2.2 A reader **shall not** attempt to correlate checks by any means, and the audit in Section 12.3 **shall** look for such an attempt.

### 13.3 Attribute minimality

13.3.1 In attribute mode, the reader **shall** receive the attribute and **shall not** receive the identity, the date of birth, or any value from which the identity could be derived.
13.3.2 An attribute **shall** be a band, not a point, where a point value would allow re-identification.

## 14. Conformance

### 14.1 Conformance classes

| Class | Requires |
|---|---|
| **Core** | Grant issue and revocation per Section 5 and 10; proof format and binding rules per Section 6; status retrieval; single-use enforcement; pass/fail verification semantics |
| **Core + PAD** | Core, plus a certified presentation-attack check per Section 9 with published rates |
| **Core + PAD + Mode A** | Core + PAD, plus the online mode of Section 7.1 |
| **Core + PAD + Mode B** | Core + PAD, plus the offline mode of Section 7.2 with the status-age bound enforced and reported |
| **Full** | Core + PAD, plus at least one of Mode A or Mode B, plus the three privacy modes of Section 13, plus the post-quantum profile of Section 11.3, plus participation in the audit of Section 12.3 |

### 14.2 How conformance is assessed

14.2.1 A **reader** is certified by an accredited body against the reader requirements of Sections 6, 9, 10.3 and 12.1, using the test suite of Section 14.3.
14.2.2 A **delegating broker** is certified by the trust service against Sections 5, 7, 10 and 11.
14.2.3 An **operator** is assessed by the audit in Section 12.3 and **shall** publish the result.
14.2.4 A **trust service** is assessed for neutrality, correctness of the status mechanism, and the non-disclosure requirements of Section 10.1.3.

### 14.3 The conformance test suite

The suite **shall** include, at minimum:

| Test | Expected result |
|---|---|
| A valid proof at the bound reader and purpose | Accepted |
| The same proof at another reader | Rejected — reader binding |
| The same proof for another purpose | Rejected — purpose binding |
| The same proof with a stale challenge | Rejected — freshness |
| The same proof presented twice | Rejected — single use |
| A proof with a revoked grant, at an online reader | Rejected |
| A proof with a revoked grant, at an offline reader, past the status age | Rejected — fail closed |
| A proof signed only with the classical component of the composite | Rejected — 11.3.2 |
| A proof with a re-signed grant | Accepted, but counted as a **new** use — 11.3.4 |
| A presentation attack of each species in ISO/IEC 30107-3 Annex A | Rejected, at or better than the published rate |
| A reader retaining a template or a stable identifier | Certification failure |
| An operator joining logs across readers | Certification failure |
| A grant used for a purpose it does not name | Rejected — Section 5.2.2 |

## 15. Open items

The following must be settled before this specification can be proposed as final. Each is stated here rather than left implicit, because a specification that hides its open questions is a specification that will be rejected at review.

| # | Open item | How it should be resolved |
|---|---|---|
| O1 | **Which delegated presence mechanism** (M1–M4) | An open evaluation against the criteria in Section 8.4, run under a standards body's auspices, with published results. M1 is patent-encumbered and M4 is the most promising but hardest; the choice is a research result, not a drafting decision. |
| O2 | **The maximum status age** for Mode B | Measurement in real deployments. The value cannot be set by drafting. |
| O3 | **Whether a composite signature satisfies the requirements of every target credential format** | Implementation testing against X.509, CMS and COSE, since a single-algorithm composite is the premise. |
| O4 | **The registry of purposes and attributes** | A governance question, not a technical one: a registry needs a maintainer, an admission rule, and a change process. |
| O5 | **The relationship to ISO/IEC 30108** | ISO/IEC 30108-1:2026 excludes authentication-protocol integration from its scope, so the series is complementary. Whether a Part 3 is the right home, or whether a separate document is, is a question for SC 37. |
| O6 | **How the delegated presence check interacts with an existing identity proof** | Where a delegated check is combined with a document check, which one is authoritative, and how a mismatch is handled. |
| O7 | **The threshold at which a subject's age is asserted** | Attribute mode needs a source for the attribute and a rule for who may assert it. |
| O8 | **Whether offline Mode B is worth requiring at all** | It imposes a revocation bound that is a genuine safety cost. If the measurement in O2 shows the bound cannot be made small enough, Mode B may be dropped from the standard rather than kept with a weak bound. |

## Annex A (informative) — Worked example

A resident grants their apartment building's gateway a standing access delegation, in no-disclosure mode, valid for one year.

1. The resident's building app asks the operating system to create a grant. The operating system shows: *Building Y — all ground-floor and residential gates, access, no identity disclosed, one year, standing*. The resident confirms.
2. The operating system creates `grant_id` G, `reader_scope` = Building Y's reader class, `purpose` = access, `attributes` = none, `not_after` = one year, and registers G with the trust service. The trust service publishes G as active.
3. The resident approaches a gate. The gate captures a presentation, runs its certified presentation-attack check, and issues a challenge.
4. The operating system performs its delegated presence check (mechanism per Section 8), produces a Mode A proof bound to that gate and that challenge, and returns it.
5. The gate verifies the signature and the bindings, queries the status, sees G active, consumes the proof, and unlocks.
6. The gate records: *a delegated access check succeeded at this gate at this time under grant G.* It does not record who. The resident sees the event in their grant list, and can revoke G at any time, after which the next check fails.

## Annex B (informative) — How this maps to existing standards

| This specification | Existing work it builds on |
|---|---|
| Section 5, the grant object | W3C Verifiable Credentials 2.0 credential structure; the `confidenceMethod` and `assuranceLevel` vocabulary of the VC Confidence Methods draft |
| Section 6, the proof | VC Data Integrity; the `validFrom`/`validUntil` fields; a status list for the grant's status |
| Section 7, Mode A | W3C VC presentation flows; OpenID4VP 1.0 |
| Section 7, Mode B | ISO/IEC 18013-5 offline verification against issuer-signed certificates |
| Section 8, M2 and M4 | ISO/IEC 24745:2022, in particular the distributed comparison application models and the renewable biometric reference framework; the `BiometricVectorConfidenceMethod` of the VC Confidence Methods draft |
| Section 9 | ISO/IEC 30107-1/-2/-3/-4; NIST SP 800-63-4 §3.11 |
| Section 10 | FIDO Metadata Service status model; VC Bitstring Status List 1.0; RFC 5280 revocation mechanisms |
| Section 13 | IETF Privacy Pass, RFC 9576/9577/9578, for the unlinkable mode |
| Section 11.3 | NIST FIPS 203/204/205; the LAMPS composite signature specification; the hybrid key exchange of RFC 9954 and RFC 10024 |
| Section 5.1, `attestation` | FIDO device attestation; platform device-attestation services |

## Annex C (informative) — What this specification does not do

- It does not make biometric verification acceptable. It makes a specific privacy-preserving *form* of it specifyable. Whether a given deployment should use biometrics at all remains the operator's decision and, in many jurisdictions, a data-protection assessment.
- It does not require a reader to be a FIDO relying party, a web origin, or a participant in any existing ecosystem.
- It does not create a market for delegated presence. The patent position in M1, the certification cost of Section 9, and the trust-service governance in Section 14.2 are all costs, and the business case is a separate matter from the specification.
- It does not settle whether the delegated presence check is solved by a mechanism that is already patented. That is the O1 research question, and it is the one that matters most.
