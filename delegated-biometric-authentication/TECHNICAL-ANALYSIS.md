Türkçe: [TECHNICAL-ANALYSIS.tr.md](TECHNICAL-ANALYSIS.tr.md)

# Delegated Biometric Authentication: Technical Analysis (Research Level)

> **Non-binding technical analysis.** The [proposal](README.md) describes the what and the why. This note shows one way to build the model so that it works reliably, and names existing standards that can be reused. It is a research-level design, not a specification. A real implementation should adapt it to its own scale, markets and laws. Commercial implementations are subject to the [commercial license](COMMERCIAL-LICENSE.md).

## 1. What must work flawlessly

The model fails if any of these fails. Every component below exists to guarantee one of them.

| # | Guarantee | Failure it prevents |
|---|---|---|
| G1 | The biometric template never leaves the device that holds it | A permanent, unchangeable credential in someone else's database |
| G2 | A proof is accepted only by the reader it was issued to, only for the operation it was issued for, and only once | A stolen or copied proof being replayed at another door or till |
| G3 | A delegation exists only because the user granted it, and the grant is visible, scoped and revocable | Delegation the user never consented to; consent the user cannot withdraw |
| G4 | A revoked grant stops working at every reader, within a stated bound | A withdrawn delegation that still opens doors |
| G5 | A reader distinguishes a live face from a photo, a mask or a replay | Biometric authentication defeated with a printed image |
| G6 | Where identity is not needed, the reader cannot tell repeat visits apart | A reader building a shadow surveillance record of the people it serves |
| G7 | A reader learns only what the purpose requires, and no more | Re-identification of a user from a narrow check, or the operator accumulating a profile |
| G8 | The system fails closed: an unavailable trust service, an expired grant or a failed liveness check means "no", never "yes" | A verification that succeeds because a component was down |
| G9 | A delegated check is auditable end to end without revealing any biometric data | An unverifiable claim of who entered, and no way to settle a dispute |
| G10 | The credentials in the system survive the arrival of a quantum computer | Long-lived delegated identity credentials that can be forged later |

## 2. Components

```
                 ┌──────────────────────────┐
                 │  User's device           │
                 │  ┌────────────────────┐  │
                 │  │ Biometric template │  │  ← never leaves the secure element
                 │  │ (secure element)   │  │
                 │  └─────────┬──────────┘  │
                 │  Grant registry, key     │
                 └────────────┼─────────────┘
                              │ signs
        ┌─────────────────────▼──────────────────────┐
        │  Delegating broker (the OS / wallet)      │
        │  - consent UI, grant list, revocation      │
        │  - challenge responder                    │
        └─────────────────────┬──────────────────────┘
                              │ registers grants, publishes status
                 ┌────────────▼─────────────┐
                 │  Trust service           │
                 │  - grant registry        │
                 │  - revocation list       │
                 │  - trust anchors         │
                 └────────────┬─────────────┘
                              │ status, anchors
   ┌──────────────────────────▼───────────────────────────┐
   │  Reader (POS terminal, gate, kiosk)                  │
   │  - camera, presentation-attack detection             │
   │  - challenge generation, proof verification           │
   └──────────────────────────┬───────────────────────────┘
                              │ proof (no biometric data)
                 ┌────────────▼─────────────┐
                 │  Operator               │
                 │  (bank, building, transit)│
                 │  - audit log, records    │
                 └──────────────────────────┘
```

### 2.1 The delegating broker

The broker is the party that holds the template and speaks for the user. In practice it is the device operating system, or a wallet application running on the operating system's secure hardware.

- **The template lives in a secure element.** It is enrolled there and never readable by the operating system, the broker application or anyone with access to the ordinary file system. This is the same trust boundary that protects the phone's own unlock, which is why the user is already comfortable delegating through it.
- **The broker holds a device key.** A private key that does not leave the secure element signs proofs. The public key is published in a certificate that the trust service lists, so a reader can check that a proof came from a registered device.
- **The broker is the consent authority.** The user grants and revokes grants. No application can grant on the user's behalf, so a malicious or compromised app cannot create a delegation.
- **In Mode A, the broker is the only party that sees the face.** The camera at the reader captures the presentation; the broker or the secure element matches it. The image itself is discarded once the match completes. The reader never receives a template, a reference image or a reusable embedding. **In Mode B**, the broker does not see the face at verification time; the reader matches the live presentation against a transformed reference the broker delivered off the verification path (Section 8.2 and Section 8.6). That reference is not a template, and the broker is not contacted at the gate.
- **The broker is revocable.** A lost or stolen device can be de-registered at the trust service, which invalidates every grant it issued.

### 2.2 The grant registry

A grant is the object that makes delegation real. It is what the user created, what the reader checks, and what the operator audits.

| Field | Meaning |
|---|---|
| Grant identifier | Unique, never reused |
| Subject | The pseudonym of the delegating device, not the person |
| Reader | The reader, or reader class, the grant applies to |
| Purpose | The operation permitted: payment, entry, age proof, attendance |
| Attributes | What the reader may learn: an identity reference, an age band, or only "authorised" |
| Valid from / until | The window during which the grant may be exercised |
| Max uses | Optional cap, for example one use for a single ticket |
| Status | Active, revoked, expired, suspended |
| Delegation counter | How many times the grant was exercised |

**A grant names a reader, not a category of service.** "This building's gateway" is a grant; "access apps" is not. The reader identity is what makes a proof non-transferable, because a proof is bound to a specific reader.

**A grant names a purpose, not a capability.** Payment, entry, age proof and attendance are separate grants with separate scopes, so a delegation for one can never be exercised for another even if the same device and the same reader are involved.

**A grant expires by default.** A grant with no expiry is a standing credential, which is the same problem the whole proposal exists to solve, in a smaller costume. Where a user wants a standing arrangement — a home, a workplace — the grant is long-lived but still visible, still revocable, and still limited in what it permits.

### 2.3 The trust service

The trust service is the neutral party that makes the model work across device makers and reader vendors. It holds no biometric data and needs none.

- **It registers grants and publishes their status.** A reader can check whether a delegation exists, is active and covers this reader and purpose, without contacting the device.
- **It publishes revocation information.** When a grant is revoked, the device reports it, and the trust service publishes the status through a mechanism readers can consume without being online with the user. The mechanism is the same shape as a certificate revocation list or a status request, and the freshness bound is stated in the protocol rather than left implicit.
- **It publishes trust anchors.** The public keys of registered brokers, and the certificates that bind a broker key to a device.
- **It never sees a face.** It holds identifiers, keys and status. If the trust service is compromised, the attacker gets keys and metadata, not biometric data. This is the property that distinguishes it from the biometric database the proposal is designed to avoid.
- **It is neutral and non-profit or utility-like.** It publishes its rules and fees, admits every broker and operator meeting published criteria, and does not rank devices or readers. A trust service that could deny a broker admission would be a gatekeeper, and the model would inherit the lock-in it was designed to avoid.

### 2.4 The reader

The reader is where the user actually is: a point-of-sale terminal, a gate, a turnstile, a reception desk. It is deliberately dumb about identity and strict about proofs.

- **It captures a live presentation and checks it.** A camera, a depth or multi-spectral sensor where available, and a presentation-attack check that rejects a photo, a mask, a screen replay and a printed image.
- **It generates a challenge.** The challenge is unique per verification, so a captured proof cannot be replayed.
- **It verifies the proof.** Signature, chain, validity window, reader binding, purpose binding, grant status, and the reader's own challenge response.
- **It keeps no enrolled template or reusable biometric identifier.** The capture is held in memory, used for the match, and discarded. The standard forbids retaining it, and the certification regime checks. In Mode B, where the delegated-presence mechanism is one that places a *transformed* reference at the reader — the renewable-reference and zero-knowledge mechanisms (M2, M4) — the reader holds that reference instead, under the conditions the [specification](SPECIFICATION.md) sets (Section 12.1.2): renewable, grant-specific, reader-bound, revocable, with a bounded reference age (Section 10.5), and not derivable back into the template. That reference is not a template, but it is a new breach surface that did not exist before the grant, and a deployment that cannot enforce all five conditions should not use Mode B.
- **It keeps no reusable identifier.** A reader that stores a stable hash of the user's face is building a biometric database, and it violates the model. The reader's record of a verification is a reference to a grant and a proof, not to a person.

### 2.5 The two verification modes

The central design choice is how the reader learns that a live biometric check succeeded, given that it never sees the template.

**Mode A — online, broker-mediated.** The reader sends a challenge to the broker over a secure channel; the broker performs the biometric match in the secure element and returns a signed assertion that the challenge was answered by the delegating device, with the grant's scope. The reader verifies the signature and the grant's validity.

- *Requires:* the broker to be reachable. The phone can be in the pocket, in another room, or at home, as long as it has connectivity.
- *Gives:* the strongest binding, because the challenge is answered live and the match happens in the one place the template is allowed to exist.
- *Costs:* a network round trip on every verification, and a dependency on a service being up.
- *Fits:* payment, building entry, hotel check-in, clinic reception — cases where a network is available and a few hundred milliseconds are acceptable.

**Mode B — offline, pre-delivered reference.** The broker derives a renewable, irreversible, unlinkable transformed reference from the on-device source, signs it with the device key, and registers it with the trust service at grant creation and at each refresh (Section 8.6). At verification the reader captures a live presentation, runs presentation-attack detection, and matches the presentation against the cached reference, with no network call to the broker. There is no challenge-response round trip and no Section 6 proof.

- *Requires:* the reader to have a cached status within the status-age bound and a cached reference within the reference-age bound.
- *Gives:* operation without connectivity to the broker, which matters for gates in basements, rural transit, and anywhere a network is unreliable.
- *Costs:* two bounds become visible. A revoked grant may still work at a reader that has not seen the revocation, until the status-age bound. A face that no longer matches the phone's current source may still open a door, until the reference-age bound. Both bounds must be small and stated (Section 10.3 and 10.5).
- *Fits:* doors, turnstiles, offline checkpoints.

**The choice is per reader and per grant, not per platform.** A building can run Mode B on its gates and Mode A for its staff entrance. A grant can be issued in whichever mode the reader supports, and the mode is visible to the user, because it changes what "revoked" means in practice.


### 2.5.1 The pre-delivered reference: the reader validates biometric data against an artifact from the phone

The design of Mode B (Section 7.2) is that **the biometric reader — a gate, a turnstile, a kiosk with a camera and an infrared sensor — validates the live biometric presentation against an artifact that the phone delivered once, at grant creation.** The phone is not contacted at verification time. There is no real-time hook, no challenge-response round trip, no network dependency between the reader and the device.

**What the phone delivers is not a signature over camera data.** A cryptographic signature verifies a fixed message: the signed bytes must be byte-identical to the verified bytes. A camera or infrared capture is not a fixed message — it varies with lighting, head angle, expression, sensor noise, and over time. No signature scheme, classical or post-quantum, can match a fresh capture against a pre-signed value, because the capture is not the same bytes that were signed. This is a property of signatures, not a limitation of any particular algorithm.

**What the phone delivers is a transformed biometric reference, signed by its device key.** The phone derives a *renewable, irreversible, unlinkable* reference from the biometric template held in its secure element — a protected representation that a live presentation can be matched against, but from which the template cannot be recovered. The phone signs this reference with its device key (the signature that authenticates the reference to the reader and the trust service) and registers it **once**, at grant creation. At every subsequent verification, the reader:

1. captures a live presentation (camera, infrared, depth — per the reader's sensor suite and its presentation-attack certification, Section 9),
2. extracts features from the capture,
3. matches the features against the pre-delivered reference,
4. accepts or rejects based on the match score and the reference's status.

The phone is not involved in steps 1–4. The "signature" in the user's framing is the device key's signature over the reference; the reference itself is the artifact the biometric is matched against. Both are needed: the signature authenticates the reference; the reference is what the biometric is compared to.

| Phase | Who | What |
|---|---|---|
| **Grant creation (once)** | Phone → Trust service / Reader | A transformed reference, signed by the device key, bound to this grant, this reader class, this purpose. Registered and published. |
| **Verification (every time, no phone)** | Reader | Capture (camera + IR + liveness) → extract features → match against the pre-delivered reference → check reference status → accept or reject |
| **Revocation** | Phone → Trust service → Reader | The reference is retired in the status. A reader that has not seen the retirement accepts the old reference until the status-age bound (Section 10.3). |

**This is mechanism M2 (Section 8.3): renewable and revocable transformed reference with secure matching.** The reference is the "signature" that the phone pre-delivers. The biometric match at the reader is the "verification." The two are distinct operations — one is cryptographic (authenticating the reference), the other is biometric (matching the live presentation to the reference) — and both are required.

**The fuzzy vault (M1) is the same model with a stronger binding.** In M1, the biometric *is* the private key: a fuzzy commitment of the biometric is pre-delivered, and a matching biometric unlocks the vault and recovers a secret that the reader uses to prove knowledge. This is the closest thing to "the phone signs something once and the reader validates biometric data against it later" — the biometric itself is the key. But M1 is subject to a granted Visa patent family ("Delegated biometric authentication," US11321445B2 and US12282539B2, with European and Chinese counterparts), and the specification flags this as a blocking risk. M2 is the patent-free equivalent and has ISO/IEC 24745 as its standardisation home.

**The trade-offs of pre-delivering the reference, stated plainly:**

- **The reference is a biometric-derived artifact at the reader.** It is not the template, but it is a new breach surface that did not exist before the grant. The specification's Section 12.1.2 requires it to be irreversible, unlinkable, reader-bound, grant-specific, and revocable. A reader that violates any of these conditions is a certification failure, and a deployment that cannot enforce all five should not use M2 — it should use Mode A, where the phone is reachable and no reference is placed at the reader.
- **Revocation is bounded, not instant.** A retired reference may still work at a reader that has not seen the retirement, until the status-age bound. This is the price of removing the real-time phone connection, and it is the same cost every offline model pays.
- **The match is fuzzy.** Error rates are typically worse than a direct template match, because the transformation introduces noise. The transformation's error rate is the system's error rate, and it must be published per attack class.

**In short:** the phone pre-delivers a signed, transformed reference. The reader's camera and infrared sensor capture the live presentation and match it against that reference. The phone is not contacted. This is the design of Mode B, and it is the mechanism that makes phone-absent biometric verification possible.

### 2.6 Liveness and presentation-attack detection

This is where the model is won or lost, because a biometric check that accepts a photograph is not an authentication at all.

- **The reader performs the check, not the operator.** The check happens where the user is standing, on hardware the operator controls but does not own the biometric of.
- **The standard is an existing one.** The check follows a recognised presentation-attack detection standard, and readers are certified against it with a published false-accept rate per attack class. A reader that accepts a printed photo should be a certification failure, not a news story.
- **Depth and multi-spectral sensing help but are not sufficient.** They raise cost and effort for the attacker; they do not replace the standard, and they must not be the only defence.
- **The user is warned when the check is weak.** Where a reader cannot run a compliant check, the grant should say so, and the operator should not be able to claim the same assurance as a certified reader.
- **The presentation is discarded.** The capture used for the check is not retained, not logged, and not used to train anything. This is what stops a reader from quietly becoming a template store.

### 2.7 Unlinkability and data minimisation

Two grants can be issued for the same reader and purpose, and they should look different.

- **Unlinkable mode.** Where the reader only needs "an authorised user", the grant can be issued as a single-use or short-lived **unlinkable token**: the reader verifies it and cannot correlate it with a previous presentation. The reader learns nothing about the person.
- **Identity mode.** Where the reader must know who the person is — a hotel, a clinic, a bank — the grant carries a scoped credential. The reader learns the identity and nothing else: not the grant list, not other readers, not other purposes, not the biometric.
- **Attribute mode.** Where only a fact is needed — over eighteen, a season ticket holder, a resident — the grant carries the fact and not the identity behind it. A retailer receives "over the threshold" and not a birthdate, and cannot ask for more.
- **Reader-side correlation is the real risk.** A reader that keeps a log of verification times and a stable grant identifier can reconstruct a person's movements regardless of what the protocol carries. The standard should require that reader records are either per-transaction or pseudonymised per period, and the operator's audit should be able to detect a log that has been joined back together.
- **The operator's record is a record of an event, not of a body.** "A delegated check succeeded at this reader at this time under this grant" is a legitimate record. "This person was here" reconstructed from a joined log is not, and the distinction is what the audit checks.

### 2.8 Revocation

Revocation is the mechanism that separates a delegation from a permanent credential, so it has to actually work.

- **The user revokes.** From the same place the grant was made. The broker marks it revoked and reports it to the trust service.
- **The trust service publishes.** The status appears in the revocation list and in the status response, so any reader that asks gets the answer.
- **Online readers learn immediately.** Mode A readers query on every verification, so a revoked grant fails on the next attempt.
- **Offline readers learn within a bound.** Mode B readers work from a cached status and fail-safe on expiry. The bound is the maximum age of a cached status the protocol permits, and it should be short — minutes, not hours — because a door that opens for a revoked grant is exactly the failure users fear.
- **Revocation is not deletion.** The grant record and the audit trail remain, because a building needs to show that an access happened. What disappears is the *ability to use it*. A user who withdraws a delegation keeps the record of having made it; that is the user's own record, and the operator's, and neither contains a biometric.

### 2.9 The audit

An audit is what turns "trust us" into something checkable.

- **Each verification produces a signed record.** The grant, the proof, the reader, the challenge, the outcome and the time.
- **The operator publishes aggregate figures.** Numbers of verifications, outcomes, failure reasons and revocation counts — with no per-person data.
- **An independent auditor checks conformance.** That readers keep no biometric data, that grants are scoped as claimed, that grants expire, that offline readers respect the freshness bound, and that the logs have not been joined into a movement profile.
- **The trust service publishes its own transparency figures**, so a reader vendor or an operator can be compared.
- **A user can see their own trail.** Grants, uses and revocations, in the operating system's permissions view.

### 2.10 Post-quantum considerations

A delegated identity credential is long-lived by nature. A building access grant, a hospital delegation, a payment standing arrangement and a transport pass may all be issued years in advance and remain valid for years. That is the textbook case for quantum-resistant cryptography, and it is why the [standardization note](STANDARDIZATION.md) treats it as a first-class layer rather than an upgrade.

- **Signatures are the long-lived part.** A signature made now must still verify in ten years, so the signature scheme needs post-quantum strength at the time of issuance.
- **Key exchange is the session part.** The session between reader and broker is short-lived and can rotate; classical key exchange is acceptable there, but hybrid construction is cheap insurance during the transition.
- **The migration is a problem of the whole chain.** The trust service, the broker, the reader and the operator all have to move together, because a reader that only accepts classical signatures blocks the post-quantum readers and vice versa. A standard that names the algorithm profile up front lets the migration be planned instead of discovered.
- **"Harvest now, decrypt later" applies to the audit trail, not to the template.** The template is never transmitted, so it is never harvested. The records that matter — grant histories, audit trails, proof transcripts — are the long-lived ciphertexts that need post-quantum protection.
- **Rotation must be designed in, not bolted on.** A credential that can only be replaced every ten years is a long-lived secret. Shorter-lived proofs and rotatable device keys reduce the exposure window.

### 2.11 Data protection law

The model is a data-minimising construction, and the law should treat it as one.

- **The template is not processed by the operator or the reader.** It stays in the device. Where a reader performs the match, the capture is processed locally and discarded, which is a much smaller processing footprint than a remote biometric comparison.
- **A biometric template is a special category of data** under the EU's General Data Protection Regulation and under Turkey's Law on Protection of Personal Data, and the "delegation" here is best understood as the data subject choosing the *means* of verification rather than the controller choosing it — a distinction the legal analysis should make explicit.
- **The lawful basis still has to be named.** A delegation does not by itself create a lawful basis for the operator; it supplies a technical control. The operator's basis, its retention rules and its data-subject rights are unchanged by the protocol.
- **Data minimisation becomes demonstrable.** The strongest argument for this model in a supervisory-authority conversation is that the operator holds a credential and a log, and provably holds no enrolled template, so a breach of the operator is a breach of keys rather than a breach of irreplaceable personal data.
- **The newer biometric rules in the EU apply to remote biometric identification systems.** A delegated check is not a remote biometric identification system in that sense, but an implementer should state the analysis rather than assume it.

## 3. Failure modes

| Failure | Detection | Response |
|---|---|---|
| A reader accepts a photograph | Certification testing against the presentation-attack standard; incident reports | Reader loses certification; grants to that reader suspended pending re-certification |
| The template is exfiltrated from a device | Secure-element tamper detection; attestation | Device re-enrolment; the leaked template is useless because it is not accepted by any reader |
| A proof is replayed at another reader | Proof bound to reader and challenge; duplicate challenge detection | Proof rejected; incident reported to the operator and the trust service |
| A proof is replayed at the same reader | Single-use tracking within the proof validity window | Proof rejected as consumed |
| A malicious app creates a grant | Grants exist only in the operating system's security subsystem | Grant refused; app removed from the platform |
| A revoked grant still works at an offline reader | Stale cached status beyond the freshness bound | The reader fails closed; the bound is tightened; a stale reader is taken out of service |
| The operator builds a shadow biometric database | Conformance audit; storage inspection | Breach of the standard; certification and grant registration withdrawn |
| A reader joins its logs into a movement profile | Log-structure audit | Breach of the standard; the operator's certification is withdrawn |
| The trust service is unavailable | Health checks; readers fail closed | Verification is refused, not permitted; no degraded accept path exists |
| The broker is unavailable (Mode A) | Health checks | The reader refuses the check; a fallback credential (card, code, passkey) may be used if the operator offers one |
| A broker's key is compromised | Key attestation; certificate revocation | The device is de-registered; all its grants are revoked; affected readers are notified |
| A grant is used beyond its purpose | Purpose binding in the proof; audit of grant scope | Use refused; incident logged |
| A grant outlives its purpose (a building access grant used for a payment) | Cross-purpose detection in the audit | Grants cancelled; the operator and the user are notified |
| The user cannot tell who exercised a grant | Audit records bound to a pseudonym the user can resolve | The user can see the trail; the operator's granularity is set by the grant |
| A reader's clock is wrong and rejects valid proofs | Tolerance windows in the protocol; error-rate monitoring | Clock synchronisation requirement in the certification |
| Long-lived credentials are harvested and decrypted in future | Post-quantum signature profile required from day one | Not recoverable after the fact; the algorithm profile must be in the standard before deployment |

## 4. Phased build

1. **Grant and proof specification:** the grant object, the two verification modes, the proof format, the challenge-and-response, the binding rules and the revocation model. Written to be implementable by a reader vendor without a device vendor's help.
2. **One closed pilot:** building access, in a single building, where the operator is the user's own landlord. The value is easy to see and the risk is contained: one site, one grant, one reader type.
3. **Liveness and reader certification:** the presentation-attack testing regime, the published false-accept rates, and the certification that makes "it accepted a photo" a certification failure.
4. **Online mode at scale:** payment and transport, with Mode A, where connectivity is assumed and the operator is a professional trust party.
5. **Offline mode and the revocation bound:** Mode B, with the freshness bound set by measurement rather than by guesswork, and the failure-closed behaviour tested under stale caches.
6. **Unlinkable and attribute grants:** the privacy-maximising modes, in the use cases where they fit best — transit, age proof, and access where identity is not needed.
7. **Post-quantum migration:** the algorithm profile, the hybrid transition, key rotation and the long-term archival format for grant histories.
8. **Public audit and transparency:** the conformance audits, the published aggregate figures, and the trust service's own transparency report.

## 5. Existing standards and work to reuse

*This section is completed in the [standardization note](STANDARDIZATION.md), which maps each requirement to the nearest existing standard, states where they fall short, and proposes the missing piece. The short version: the pieces are covered, the combination is not.*

The three that this analysis leans on hardest:

| What the analysis needs | What already provides it |
|---|---|
| A template that never leaves the device, matched locally | FIDO2 local user verification; ISO/IEC 27553-1:2022 (local modes) requires that biometric data and derived data, except the verification outcome, do not leave the device; ISO/IEC 24745:2022 for irreversibility, unlinkability and renewability |
| A scored, certified liveness check at the reader | ISO/IEC 30107-1:2023, -2:2017, -3:2023 (which adds the relative impostor attack presentation accept rate), -4:2024 (mobile-device test profile); NIST SP 800-63-4 §3.11 for the remote PAD threshold |
| A scoped, revocable credential a reader can check | W3C Verifiable Credentials 2.0 (Recommendation, May 2025) and the VC Confidence Methods working draft, whose `BiometricVectorConfidenceMethod` already describes a client-side zero-knowledge match where no biometric data leaves the device |

Two pieces of work to watch, because they may change the landing zone: **ISO/IEC 30108-1:2026** (Identity Attributes Verification Services) is close in subject matter but excludes integration into authentication protocols from its scope, and **ISO/IEC 26655** (biometric deepfake attack detection) is in development, which matters because generative attacks are now part of the reader's threat model.

One prior art that must be respected: the fuzzy-vault mechanism this analysis describes for the delegated-presence check is the subject of a **granted Visa patent family** ("Delegated biometric authentication"). The standardization note treats this explicitly.

## 6. Open questions for a real implementation

- **How long should the offline revocation bound be?** It trades a building's ability to work through a network outage against the window in which a revoked grant still opens a door. A measurement programme, not a decision, should set it.
- **What is the right minimum device trust for a delegated check?** Some deployments will want a hardware-backed attestation; others will accept a platform-level one. Requiring the strongest everywhere would exclude the devices that need the feature most.
- **How should a reader handle a user with no delegation?** Fallback credentials matter, and the model should not become a reason to remove the card reader from a door.
- **What is the right relationship between a delegated check and a legal proof of identity?** In a hotel, a clinic or a polling station, the operator's legal duty may require more than a delegated check provides, and the boundary should be stated rather than blurred.
- **How do age and other attribute grants get issued?** The attribute has to come from somewhere authoritative, and that authority becomes a holder of the underlying evidence.
- **Who pays for the reader certification regime?** It is a public-good cost, and if it is not funded, readers will not be certified and the model will fail on liveness.
- **What happens when the operating system vendor and the operator disagree?** The standard should say who decides, and an appeal should exist, or the model's neutrality is only nominal.
