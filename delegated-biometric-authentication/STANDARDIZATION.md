Türkçe: [STANDARDIZATION.tr.md](STANDARDIZATION.tr.md)

# Delegated Biometric Authentication: Path to an Open Standard (Draft)

> **Working draft.** This note prepares the [proposal](README.md) for formal standardization. It has three parts: an honest **gap analysis** mapping every requirement to the nearest existing standard, a **proposal** for the piece that is missing, and a **route** for getting it into a standards body. The normative text a workshop would write is drafted separately as the [technical specification](SPECIFICATION.md), and this note is its rationale. It is a starting point, not a finished specification. The procedures, forms and fees of the standards bodies change, so check them with the body you apply to before submitting.

## The question this note answers

The author was asked to add a standards proposal **only if the current standards do not already cover the project**, and to include post-quantum security if a proposal is filed.

**They do not cover it** — on the public record of the standards bodies, their working drafts and their published documents. The gap is real, and it is narrower and more interesting than "biometrics are not standardised" — they are, thoroughly. Three existing standards each cover one third of this problem and none covers the seam between them. This note states the seam precisely, because a proposal that misstates the gap is worse than no proposal. (A caveat on the strength of that claim, and on the one standards catalogue that is not fully public, is in the closing note of Part 7.)

There is a second finding that shapes everything below: **the mechanism this project needs already exists, and it is patented.** The proposal is therefore not "nobody has thought of this" — it is "the idea is old, the mechanism is locked in one company's portfolio, and the ecosystem needs a royalty-free, standardised, interoperable path."

## Part 1: Gap analysis

### 1.1 The requirements, one by one

The project needs a reader (a till, a gate) to verify a person face-to-face, with the phone absent, the biometric template never leaving the phone, a grant that is scoped per reader and revocable, and no reusable identifier left with the reader.

| # | Requirement | Covered today? | Nearest existing standard | Where it breaks |
|---|---|---|---|---|
| R1 | **The template never leaves the device** | **Yes, fully** | FIDO2 local user verification; ISO/IEC 27553-1:2022 (local modes) requires that biometric data and derived data, except the verification outcome, do not leave the device; ISO/IEC 24745:2022 (irreversibility, unlinkability, renewability) | Nowhere. This is the one requirement that is *already* solved and standardised — and it is the property the whole proposal is built on. |
| R2 | **Liveness at the reader** | **Yes, largely** | ISO/IEC 30107-1:2023 (framework), -2:2017 (data formats), -3:2023 (testing and reporting), -4:2024 (mobile-device test profile); NIST SP 800-63-4 §3.11 | Nowhere. The standard is scored and certified. This proposal should cite it, not reinvent it. |
| R3 | **A scoped, revocable credential bound to one verifier** | **Partly** | W3C Verifiable Credentials 2.0 (Recommendation, 15 May 2025); EUDI Wallet / eIDAS 2.0 | VC credentials are revocable via status lists, but they assume the **holder presents them**. The phone-absent case has no holder. |
| R4 | **The phone is entirely absent** | **No — hard break** | FIDO cross-device (hybrid), CTAP 2.3; ISO/IEC 18013-5 mobile documents; EUDI Wallet | Every one of these needs the phone present as the authenticator. Nothing in the stack can attest that *a human was physically present at reader X* when the phone is 200 metres away in a pocket. |
| R5 | **The reader is not a web relying party** | **No — hard break** | FIDO2 / WebAuthn Level 3 (W3C Recommendation, 25 August 2026) | FIDO2 is architecturally origin-bound: a registration and challenge ceremony, an RP ID, a web origin. A point-of-sale terminal or a door is not an origin, and there is no FIDO profile for "an unattended, often-offline reader as relying party". |
| R6 | **Revocation reaches readers that are not online with the user** | **No — hard break** | FIDO Metadata Service 3.1.1 (Proposed Standard, 5 January 2026); VC Bitstring Status List 1.0 | MDS is a network-fetched blob, and its rogue list is **per-authenticator** and requires the ECDAA key — useless for a per-user, per-reader grant. Status lists are network-fetched too. Neither is a bounded-propagation story for an intermittently-connected third-party reader. |
| R7 | **The grant itself as a first-class, revocable object** | **No — hard break** | ISO/IEC 24745:2022 application models A–K, including the distributed models I, J and K added in 2022; ISO/IEC 30108-1:2026 (Identity Attributes Verification Services) | 24745 is a **framework and requirements** standard. It classifies the solution; it does not specify it. It has no wire format, no consent-grant object, no reader verification procedure, and no revocation propagation. ISO/IEC 30108-1:2026 is close in subject matter but its scope explicitly limits biometric functionality to remote backend services and places **integration into authentication protocols out of scope** — so it is a near-miss, not a home. |
| R8 | **Unlinkability across readers** | **Largely, with one caveat** | ISO/IEC 24745:2022 §7.2 and application models; IETF Privacy Pass (RFC 9576 architecture, RFC 9577 HTTP authentication scheme, RFC 9578 issuance protocols) for unlinkable tokens | Covered for credentials and for tokens. But the Privacy Pass working group has found that its own unlinkability primitive is **not post-quantum-unlinkable** — see Part 4.3. |
| R9 | **Offline operation** | **Partly** | ISO/IEC 18013-5 supports offline verification against issuer-signed certificates; Privacy Pass tokens are offline-redeemable | FIDO2 is the outlier — it is always online. Offline operation is the interesting case for gates, and it is where R6 bites hardest. |

### 1.2 The seam, stated precisely

> **Three existing standards each cover one third of this problem and none covers the seam.**
>
> **FIDO2** gives device-bound presence with local verification — the template genuinely never leaves the device — but it requires the device to be present and the verifier to be a web relying party.
>
> **ISO/IEC 30107, 24745 and 27553** give presentation-attack detection, template protection, irreversibility, unlinkability and on-device handling — but they say nothing about *delegation*. They classify the solution; they do not specify it.
>
> **Verifiable Credentials** give scoped, revocable, privacy-preserving credentials — but they assume the holder presents them, and the holder is precisely what is absent.
>
> **The seam is: a scoped, revocable grant asserted about a person to a third-party reader, with the phone absent.**

Narrowed to the three sub-gaps that are genuinely unsolved:

1. **G1 — Presence without proximity.** A reader-local liveness check proves that *a live face is here*. It does not prove that *the person who granted the delegation is here*. Every existing standard answers the first and assumes a device-bound key answers the second. Delegation breaks that assumption, because the key is not there.
2. **G2 — The grant as a first-class object.** A grant that is asserted *about* a person by a device that is *not present*, to a reader *outside the ecosystem*, revocable within a bounded window, is not an object any current standard defines.
3. **G3 — Bounded revocation to arbitrary readers.** Revocation must reach a reader that is intermittently connected and that knows only a pseudonymous grant identifier, without a central database of "who delegated what" — which would recreate the honeypot the project exists to avoid.

### 1.3 Where the standard should land, and why not a new one

The proposal should **not** invent a credential format. W3C has already built the seam it needs to be filled:

- The Verifiable Credentials 2.0 family (Recommendation, 15 May 2025) defines `confidenceMethod` and `assuranceLevel` in `credentialSubject`.
- The **VC Confidence Methods** working draft (10 September 2026) already defines a `BiometricVectorConfidenceMethod` with a **client-side zero-knowledge match flow in which no biometric data leaves the device**, together with a `BiometricMatchingModel` — with collaboration with CLR Labs in France on zero-knowledge biometric matching.
- The same working group has been **deliberately retiring the term "holder binding"** (confusing to state governments) in favour of "confidence method", and its own discussion covers **delegation cases** explicitly.
- Its `assuranceLevel` is a Feature at Risk, and several sections are still marked TBD — **this is exactly the hole to fill.**

**The proposal is therefore: a `DelegatedPresence` confidence method and claim type for VC 2.0, plus a reader, presentation-attack and revocation-propagation profile.** Not a new data model — a missing confidence method and its profile.

One vocabulary note to handle gracefully: OpenID4VP 1.0 (final) still uses "Holder Binding or Key Binding", including a biometrics-based variant, while W3C has moved to "confidence method". W3C and OpenID currently use different words for the same seam. A proposal should bridge them rather than pick a side.

### 1.4 The near misses, and one important precedent to concede

A standards reviewer will raise several pieces of work that come close, and a proposal that does not address them will be dismissed. It should address all of them.

**ISO/IEC 30108-1:2026 — Identity Attributes Verification Services (the former BIAS series).** Published 12 January 2026 in ISO/IEC JTC 1/SC 37, defining biometric services used for identity assurance over a services-based framework. This is the closest ISO deliverable in subject matter to "delegated presence verification". But its scope **limits biometric functionality to remote backend services and places integration into authentication protocols out of scope** — and a delegated check to an out-of-ecosystem reader is exactly an integration into an authentication protocol. A near-miss, not a home, but a document to cite and to work alongside. Three further SC 37 items are worth naming: **ISO/IEC 25049-1/-2 (Customizable biometric web services)**, at Approved Work Item stage in 2026 and generalising the German BSI TR-03121-2 high-level biometric service architecture; **ISO/IEC 26655 (Biometric deepfake attack detection)**; and **ISO/IEC 25456 (Injection attack detection)**, expected in 2027. Together with ISO/IEC 30107 these complete the reader's threat model: presentation attack, then deepfake, then digital injection.

**FIDO's own delegation concept — a precedent to concede, not to claim.** FIDO published *"FIDO for SCA Delegation to Merchants or Wallet Providers"* in March 2021, giving a precise legal and technical account of delegating strong customer authentication to a merchant or wallet, grounded in the EBA's Opinion EBA/OP/2020/10, and requiring **dynamic linking** so the delegation is bound to the transaction. **A proposal must not claim to invent the word "delegation" in this field.** What FIDO delegated was *transaction authority*, not *presence*, and the phone was still present. The useful inheritance is threefold: the concept exists, delegation requires dynamic linking (so a grant must be bound to reader, purpose and amount), and the `FIDO:/` URI scheme is the transport.

**ZCAP — the authority half is already standardised, and the proposal should build on it.** The W3C Community Group's **ZCAP** (JSON Web Token Proof-of-Possession Authorization Capabilities) specification, at draft v0.4.0, is the most complete machinery anywhere for *delegated, scoped, attenuated, expiring, offline-verifiable authority*: capability chains, `parentCapability` links, attenuation, mandatory expiry. It is **not biometric at all** — no liveness, no presence, no template. But it is precisely the *authority* half of what this proposal needs, and a proposal that defined its own grant object from scratch would be told it had reinvented capability delegation. **The grant should be a capability in the ZCAP sense**, which removes a large amount of specification work and makes the proposal stronger rather than weaker.

**Three more that a reviewer may raise.** **IEEE 2410-2021 (Biometric Open Protocol)** is a real near-miss: the client holds the biometric, the server receives a homomorphically encrypted payload, and plaintext never reaches the server. **ITU-T X.1089** (Telebiometrics authentication infrastructure, 2008) and **ITU-T X.1083 / ISO/IEC 24708** (BioAPI interworking) are legacy remote-biometric-service standards that a reviewer may cite for completeness. **BSI TR-03121-2 / TR-03122-2** is the German public-sector biometric service architecture that ISO/IEC 25049 generalises.

**Two more uses of the same word for a different object**, worth naming so the proposal is not accused of reinventing: **EMVCo's agentic-payments framework** registers "consumer-authorised intent and delegated authority" — delegation of *payment intent*; and **FIDO delegated cross-device** — delegation of *signing*. The object this proposal needs is different from all of them: delegation of *a biometric presence check to a reader that is not the device*.

### 1.5 The substrate is already ISO-blessed — and demonstrably silent on delegation

Two findings about **ISO/IEC 24745:2022** that strengthen the proposal considerably.

**First, the technology is not new.** Annex C, *"Technology examples for biometric information protection"*, and its Table C.1 already list the **fuzzy vault** as a concrete example of how to construct a pseudonymous identifier with auxiliary data — described at term 3.35 in terms of "a helper data approach, fuzzy commitment scheme or fuzzy vault". A delegated presence check built on a fuzzy vault would therefore be building on an **existing ISO-sanctioned technology example**, not inventing a mechanism. That is the difference between a proposal and a research programme.

**Second, 24745 says nothing about delegation.** The word does not appear in it at all. Its Clause 8 application models A–K are strictly *storage-versus-comparison location* models — where the biometric reference is stored and where the comparison happens — and the three models added in 2022 (I, J and K) are the **distributed comparison** models, in which the comparison happens somewhere other than where the reference is held. That is close to the shape this proposal needs, and it is the nearest thing in any standard to "the reference is not where the check is".

**The conclusion is sharper than "24745 does not help".** ISO/IEC 24745 provides the substrate, blesses the mechanism as a technology example, and models distributed comparison — and is *demonstrably* silent on the question of who is authorised to ask, which is the whole of the delegation problem. A standard that fills that silence would extend 24745 rather than compete with it.

### 1.6 The negative result: nothing in the field does this today

The gap survives an exhaustive check, and the check itself is worth publishing.

**IETF, swept exhaustively.** A full-text search of the datatracker across every working group returns only **two** documents with "biometric" in the name — an expired individual draft about deriving a key from biometric traits, and a slides deck on Wi-Fi sensing. **No IETF document delegates a biometric check to a third-party device.** The "delegated authority" cluster in OAuth and WIMSE — delegated authorization, agent delegation, WIMSE, Web4 delegated authority, ZCAP — is uniformly about *authority* and never about *biometric presence*.

**Every commercial product either has the phone present, or delegates to a human rather than to a reader.**

| Product | Phone / holder | A third-party reader verifies? |
|---|---|---|
| **Suprema "Face Template on Mobile"** (BioStation 3) | Phone **present** — the template and the match are on the user's own phone, which sends only the *result* to the terminal | The terminal is a separate reader, but the person is standing at it **with their phone** |
| **KeyShare "Delegated Agent Mode"** | Citizen's phone **absent**; keys in a cloud wallet; an authorised human agent's device is the interface | The verifying party is the service, not a public reader; the agent acts *for* the citizen |
| **Dentity "Agentic Identity"** | Wallet **present** | No public reader; delegation of authority to an AI agent |
| **Proxy Checkout** | Parent's phone **present** | Cart approval, not a reader |
| **VeryAI** | Own phone, **present** | Not a third-party reader |

**Suprema's Template-on-Mobile is the closest commercial analogue and the most dangerous thing for a novelty claim** — template on device, third-party reader, all in one product. But the user is standing there with their phone, which is the one condition the proposal changes. And it is a product, not a standard.

## Part 2: The mechanism is already patented

This must be stated plainly, because it changes what a standard is for.

**Visa International Service Association holds a granted patent family titled "Delegated biometric authentication"** — inventors Sunpreet Singh Arora and Maliheh Shirvanian, priority 1 October 2019, granted in the United States as [US11321445B2](https://patents.google.com/patent/US11321445B2/en) (3 May 2022) and [US12282539B2](https://patents.google.com/patent/US12282539B2/en) (22 April 2025), with European and Chinese counterparts ([EP4038523B1](https://patents.google.com/patent/EP4038523B1/en), [CN114556339A](https://patents.google.com/patent/CN114556339A/en)).

The mechanism it describes is **precisely the missing piece of G1**:

- The user's phone generates a **fuzzy-vault "data vault"** — biometric points derived from the template through an encoding function, mixed with random chaff points — that **locks an interaction token**.
- The phone sends the **vault, not the template**, to a public device such as a hotel terminal, a library kiosk, an ATM or a building security terminal.
- The public device captures a fresh template locally, finds the matching subset, reconstructs the encoding function by polynomial interpolation, unlocks the token — and **discards the template**.
- The patent also covers **revocation, recurring delegation with vault replacement, and limited-use, limited-time and limited-amount tokens**; and it names the exact use cases: hotel smart TV, library terminal, ATM, apartment building security terminal.

The academic prior art predates it: Chabanne, Keuffer and Lescuyer, *"Delegating Biometric Authentication with the Sumcheck Protocol"* (WISTP 2016), and Gritti, Önen and Molva, *"Privacy-preserving delegable authentication in the Internet of Things"* (ACM SAC 2019).

**What this means for the proposal.**

- The idea is not new, and a standards proposal must not pretend otherwise. It should cite the patent and the academic work.
- The mechanism is currently reachable only through a proprietary, licensed path. An open standard that specifies the same guarantees — delegated presence, a revocable scoped grant, no template at the reader — gives implementers a **royalty-free** route to the same property.
- This is the same licensing logic this repository already holds: **free for the whole public, paid for a commercial edge, and a standards body that publishes the result freely gets the contribution free.** A standard that specifies the mechanism openly *is* the contribution, and it is a much stronger contribution than a document that only describes the problem.
- Whoever works on this should obtain advice from a patent attorney before making any public statement about its own mechanism, and before filing anything. Nothing in this repository is a freedom-to-operate opinion.

## Part 3: The proposal

### 3.1 Layering — where each piece belongs

The user of this proposal chose a **layered, multi-body** route. Each piece goes where it belongs, rather than squeezing everything into one document.

| Layer | Body | What is standardised there | Why there |
|---|---|---|---|
| **Delegation-grant object and wire protocol** | **IETF** | The grant format, the challenge-and-response, the proof format, the reader-binding and purpose-binding rules, the status and revocation messages | A wire protocol is what IETF does, and it is where the Privacy Pass and post-quantum hybrid work already lives. A new working group in the existing security area is cheaper and more likely to succeed than a new specification in a new place. |
| **Biometric template handling, presentation-attack detection, evaluation** | **ISO/IEC JTC 1/SC 37** (with SC 27 for protection) | Reuse and tighten ISO/IEC 30107-3 and -4, 24745, 27553 and 39794; add the requirements a delegated reader needs, including the requirement that the presentation be discarded | The biometric parts are already standardised here, in the committee that owns them. This proposal should not reopen them. |
| **Credential and confidence-method format** | **W3C Verifiable Credentials Working Group** | The `DelegatedPresence` confidence method, the claim type, the status-list integration | VC 2.0 is a Recommendation and the Confidence Methods draft already has the biometric section. This is the smallest, most specific piece of work and the natural first one. |
| **Deployment ecosystem, reader certification, cryptographic profile** | **FIDO Alliance** | Reader conformance, the allowed-cryptography profile, the post-quantum algorithm list | FIDO already certifies authenticators and biometric components, already publishes an allowed-cryptography list with a post-quantum section, and already owns the reader-adjacent ecosystem. It is where adoption lands. |
| **Payment-side authorisation** | **EMVCo** | Delegated payment intent and authority, alongside the emerging agentic-payments framework | Delegated payment authorisation is being standardised here now, and a delegated biometric check that authorises a payment has to fit it. |
| **Reader liveness certification** | **ISO/IEC SC 37** with national accreditation bodies | Who certifies a reader, and how | Certification is what turns "it accepted a photo" into a certification failure. |

**Why not one document.** A single standard spanning IETF, ISO, W3C, FIDO and EMVCo is not achievable in one workshop and would not be adopted. A layered proposal lets each body take the piece it owns, and lets a deployment start with the W3C and FIDO pieces alone.

### 3.2 What the standard must specify

**The delegating broker.** The operating system or wallet holds the template in a secure element, holds a device key that does not leave it, and is the only party that can create or revoke a grant. An application can request a grant; it cannot create one.

**The grant object.** Unique identifier, the delegating device as a pseudonym, the reader it applies to, the permitted purpose, the attributes the reader may learn, validity window, optional use cap, status, and a delegation counter. A grant names a *reader* and a *purpose*, not a service category — that is what makes a proof non-transferable.

**Two verification modes.**
- *Online, broker-mediated:* the reader challenges the broker over a secure channel; the broker matches in the secure element and returns a signed assertion. Strongest binding, needs connectivity.
- *Offline, signed proof:* the broker pre-issues a short-lived, reader-bound, purpose-bound proof; the reader verifies it against published trust anchors and a cached status, with no network call.

**The delegated presence check (G1).** This is the heart of the proposal and the part that no standard in the public record covers. The reader must establish that the person in front of it is the person who granted the delegation, without a template and possibly without a network. Candidate mechanisms, none of them settled:

- A **zero-knowledge biometric match**, following the direction already taken in the VC Confidence Methods `BiometricVectorConfidenceMethod` — the reader proves a match without learning a template or a reusable identifier.
- A **fuzzy-vault or secret-sharing construction**, of the kind the Visa patent describes. Powerful, but patent-encumbered, so a standard would need either a licence or a different construction.
- A **challenge-bound attestation with a trusted capture device** — assumes hardware the reader may not have.
- A **differential-privacy or zero-knowledge attribute proof** for the case where the reader only needs "over eighteen" or "authorised", not an identity.

A standard that leaves this open is still worth filing: it states the requirement, the threat model and the evaluation criteria, and lets the work proceed.

**Presentation-attack detection.** The reader performs the check, to ISO/IEC 30107-3 and -4, with a published false-accept rate per attack class, and discards the capture. The standard forbids retaining the presentation.

**Revocation propagation (G3).** A status mechanism that reaches an intermittently-connected third-party reader within a stated bound, keyed on the grant rather than the device, and that carries no personal data — so that publishing it does not create the database this project exists to avoid. This is where the design of the status list matters more than the cryptography.

**Unlinkability modes.** A grant issued as a single-use or short-lived unlinkable token where identity is not needed; a scoped identity credential where it is; an attribute-only claim ("over the threshold") where only a fact is needed. The reader's own storage must also be constrained, because a reader that joins its logs can reconstruct a movement profile regardless of what the protocol carries.

**Data minimisation requirements.** The reader retains no template, no reusable identifier and no joinable log; the operator retains a record of an event, not of a body. These are checkable, and they are the point.

**A conformance and certification regime.** Reader certification against the presentation-attack standard, a published false-accept rate, and a public conformance test suite. Without it, liveness is a claim rather than a property.

### 3.3 What already exists and must not be reinvented

| Need | Existing work |
|---|---|
| Local user verification where the template never leaves the device | FIDO2 / WebAuthn Level 3; ISO/IEC 27553-1:2022; FIDO Biometrics Requirements |
| Presentation-attack detection and testing | ISO/IEC 30107-1/-2/-3/-4; NIST SP 800-63-4 §3.11 (remote PAD required, false-presentation-accept rate threshold) |
| Template protection, irreversibility, unlinkability, renewability | ISO/IEC 24745:2022; ISO/IEC 39794 interchange formats; ISO/IEC 27553-2:2025 for remote modes |
| Scoped, revocable, user-consented credentials | W3C VC 2.0; EUDI Wallet and eIDAS 2.0; OpenID4VCI 1.0 and OpenID4VP 1.0 |
| Unlinkable tokens | IETF Privacy Pass (RFC 9576, 9577, 9578); Apple's Private Access Tokens; the Private Access Control Tokens work with the major browsers |
| Cross-device proximity proof | FIDO CTAP 2.3; the FIDO Proximity Exchange Protocol working draft |
| Delegation of a transaction authority, with dynamic linking | FIDO's 2021 SCA-delegation white paper; EMVCo 3-D Secure decoupled authentication; the EMVCo agentic-payments framework |
| Biometric services over a services framework | ISO/IEC 30108-1:2026 (IAVS), noting its scope excludes authentication-protocol integration |
| Post-quantum algorithms and migration | NIST FIPS 203 / 204 / 205; RFC 9954 and RFC 10024 for hybrid key exchange in TLS 1.3; the LAMPS composite ML-DSA specification (RFC-to-be 10073) |
| Identity proofing rules | NIST SP 800-63-4; the eKYC practice in the market |

## Part 4: Post-quantum as a first-class layer

A delegated credential is long-lived by nature, and this is a genuine reason to plan for post-quantum cryptography from the start. But the honest version of the argument is narrower than the usual one, and the difference matters.

### 4.1 What is and is not at risk

**Not at risk: the template.** The template never leaves the device, so it is never exposed to "harvest now, decrypt later". It is protected by the secure element, not by mathematics. No post-quantum scheme is needed to protect it, and claiming otherwise would be a false argument.

**At risk: long-lived cached verification artifacts.** A grant is designed to be valid for months or years, and a reader in the field may cache a proof for weeks. A future adversary able to break the classical signature can **forge a grant retroactively**, and every cached proof then validates. This is the harvest-now-decrypt-later equivalent for this design, and it is a sound reason to prefer post-quantum signatures on the grant.

**At risk: key establishment.** NIST's own ordering puts quantum-resistant key establishment first, ahead of the general 2030 and 2035 dates — the broker-to-reader session, the reader-to-trust-service channel, and any TLS protecting a delegation handshake must migrate first.

**Not the framing: "signatures must last decades".** Authentication is not subject to harvest-now-decrypt-later in the same way encryption is; an algorithm needs to be secure at the time of use. The right question is not "how long must the signature last" but "how long may a verification artifact be cached, and how long does a grant outlive its issuance relative to the migration horizon".

### 4.2 The proposed cryptographic profile

| Element | Profile | Authority |
|---|---|---|
| **Grant signature** | **Composite**: classical (Ed25519 or ECDSA P-256) **plus ML-DSA-44**, presented as *one* public key and *one* signature with a single algorithm identifier, so today's X.509, CMS and COSE stacks treat it as one atomic algorithm rather than a protocol that must learn about hybrids | NIST FIPS 204; the IETF LAMPS composite signature work, whose specification is in RFC Editor final review as RFC-to-be 10073 |
| **Reader-to-broker channel** | Hybrid key exchange, `X25519MLKEM768` | RFC 10024 (Recommended = Yes), RFC 9954 |
| **Compact alternative** | FN-DSA | **Pending** — FIPS 206 does not yet exist. Cite as under development, not as available |
| **Hash-based fallback** | SLH-DSA for long-lived, low-volume grants | NIST FIPS 205 |
| **Reader attestation** | As in the FIDO Allowed Cryptography List, which already lists ML-KEM-512/768/1024 and ML-DSA-44/65/87 in its post-quantum section | FIDO Alliance |

**Composite, not hybrid-by-convention.** The distinction matters more than it looks. A "hybrid" signature that is really two signatures concatenated needs every protocol in the path to know it is hybrid, and to verify both halves in the right order with the right message representative. A **composite** signature carries a single key and a single signature, so a grant credential can be issued into existing infrastructure — an X.509 certificate, a CMS structure, a COSE object — without any of them changing. The LAMPS composite specification defines eighteen such algorithms, and the security caveat to carry into the standard is that a composite signature gives existential unforgeability but **not** strong unforgeability, which matters for a long-lived grant: a re-signed grant must not be accepted as a fresh one.

**Migration timeline.** NIST's transition draft deprecates 112-bit RSA, ECDSA and ECDH after 2030 and disallows them after 2035, and the EU's coordinated roadmap targets member-state post-quantum roadmaps by the end of 2026 and full migration by the end of 2035. A device operating system shipped today is still in the field in 2035. **So the grant's cryptographic lifetime must be set below the migration horizon, or the grant must be re-issued under post-quantum signatures** — a requirement the standard should state, because it changes the product design.

**Key rotation and re-issue.** Grants must be rotatable and re-issuable, and a device update must be able to re-issue outstanding grants under a new post-quantum key without the user re-consenting, because the scope has not changed.

### 4.3 One caveat that applies to the unlinkable mode

The Privacy Pass working group found that its unlinkability primitive is **not post-quantum-unlinkable**: a quantum attacker can solve the discrete logarithm on the token's tag and link presentations. The group's proposed remedies are a different construction committed via KZG, a Legendre-style pseudorandom function, or a post-quantum-secure hash. **A standard that offers an unlinkable mode should say so explicitly**, rather than implying that today's unlinkability is durable.

## Part 5: Governance and ratification

**Who should be at the table.** Device operating-system and secure-hardware vendors; reader and point-of-sale terminal manufacturers; access-control and payment operators; biometric-technology vendors; the standards bodies named in Part 3.1; data-protection authorities and their technical counterparts; academic researchers in biometric cryptography and privacy; and civil-society groups with a track record on data minimisation.

**What is decided in the open.** The threat model, the requirement set, the conformance criteria, the evaluation methodology for presence checking, and the test vectors. Not the commercial terms of any implementation.

**Neutrality.** The trust service is non-profit or utility-like, publishes its rules and fees, admits every broker and operator meeting published criteria, and does not rank devices or readers. A trust service that could refuse a broker's admission would be a gatekeeper, and the model would inherit the lock-in it was designed to avoid.

**A published appeal.** Where a certification, a conformance decision or an access decision is contested, there is a published appeal route. Certification regimes without appeals become closed shops.

## Part 6: Licensing, rights and route

**The author's position: free for the whole public, paid for a commercial edge.** This is the position already held across this repository.

- **Public use for everyone: free.** Where a state makes the flow mandatory, so that the whole public benefits, the author asks no fee. The standard should then be freely accessible, and its development and governance should be paid from taxes, not from selling the standard.
- **Voluntary commercial use for a competitive edge: revenue share.** As long as the flow is not mandatory, a company using it to offer more trust than its competitors is using it as a marketing advantage for part of the market, not for the whole public. That use stays under the revenue share of the [commercial license](COMMERCIAL-LICENSE.md) until a state application makes the flow mandatory.
- **The same rule for the standards body.** A body that offers the result free to the whole public receives the contribution free. A body that sells it as a commercial product should acquire it from the author on paid terms.

**What this means for standardization.**

- **CEN and ETSI do not pay contributors.** Participants in a workshop grant the body the use of their contributions free of charge, and the body sells the published documents. The author therefore contributes only to a deliverable that is **free to access** — an ETSI deliverable, which is published free of charge, or a workshop deliverable that is freely available.
- **The revenue share needs a hook.** A published standard can be implemented without the text of this repository, and copyright protects the text, not the idea. The revenue share therefore rests on: (1) commercial use of the text and materials of this repository; (2) a **certification mark** for a conforming flow, so that a company wanting to claim conformance as a mark of trust needs a licence, while state applications use it free of charge; and (3) any patent the author may obtain.
- **The patent position needs advice.** Given the granted patent family described in Part 2, anyone working on the delegated-presence mechanism should take patent advice before making public statements about a particular construction, and before filing. See the note at the end of Part 2.
- Before signing any contribution or IPR declaration, or filing a mark, the texts should be checked by a lawyer.

**Route.** The first and smallest step is the **W3C VC Confidence Methods** work: a proposal for a `DelegatedPresence` confidence method, with the biometric and delegation sections filled in and the zero-knowledge match flow specified. It is the narrowest piece, it has a live working draft, and it does not require a new body. The **IETF** layer follows, as a protocol specification in the existing security area, alongside the Privacy Pass and post-quantum hybrid work. The **ISO/IEC JTC 1/SC 37** layer is a revision of the existing parts rather than a new document. **FIDO** takes the reader-conformance and allowed-cryptography work, where its certification programme already sits. **EMVCo** takes the payment-side delegation, alongside the agentic-payments framework it is already developing.

**Supporters.** Before any of these steps, collect letters of support from at least one device or wallet vendor, one reader or terminal manufacturer, one operator, one data-protection authority or its technical counterpart, and one academic or civil-society group. A proposal that arrives without them will not be prioritised.

## Part 7: What must be added, and what already exists

This is the phasing a real standardisation effort would follow.

| Already exists — reuse, do not rewrite | Must be added — the actual work |
|---|---|
| Local user verification where the template never leaves the device (FIDO2, ISO/IEC 27553-1) | The **delegated presence** check: proving an absent grantor's authorisation at a third-party reader, without a template |
| Presentation-attack detection and testing (ISO/IEC 30107-1/-2/-3/-4) | The **grant object** and its wire format, with reader-binding, purpose-binding and scoping rules |
| Scoped, revocable credentials (W3C VC 2.0, EUDI Wallet, OpenID4VCI/VP) | The **DelegatedPresence** confidence method and the associated claim type |
| Unlinkable tokens (IETF Privacy Pass, Private Access Tokens) | **Bounded revocation propagation** to intermittently-connected third-party readers, with no personal data in the status list |
| Cross-device proximity proof (CTAP 2.3, the Proximity Exchange Protocol) | **Reader conformance and certification**, with published false-accept rates |
| Post-quantum algorithms (NIST FIPS 203/204/205, RFC 9954/10024) | The **post-quantum profile** for grants and sessions, and a re-issue path for existing grants |
| Payment authorisation (EMVCo 3-D Secure, agentic payments) | The **payment-side delegation** of a biometric check to an authorisation |

**A closing note on the strength of the claim.** The gap analysis above rests on the public record of the standards bodies, their working drafts and their published documents as of October 2026. The ISO/IEC JTC 1/SC 37 work-item catalogue is not fully publicly searchable, and several relevant items are in flight — ISO/IEC 30108-1:2026, 26655, 25049-1/-2 and 59794-1/-5 — so a live SC 37 item on delegation is not implausible. Before publishing, a targeted check of that catalogue should be made, and the claim in this note is best stated as **"no existing standard was found in public sources"**, not as an absolute. The other gap statements — about FIDO2's origin-bound verifier model, about Verifiable Credentials assuming a presenting holder, about the revocation mechanisms of FIDO MDS and VC status lists, and about ISO/IEC 30108-1's exclusion of authentication-protocol integration — follow from the published specifications and are stated with confidence.
