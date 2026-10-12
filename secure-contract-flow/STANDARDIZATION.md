Türkçe: [STANDARDIZATION.tr.md](STANDARDIZATION.tr.md)

# Secure Contract Flow: Path to a European Standard (Draft)

> **Working draft.** This note prepares the [proposal](README.md) for formal European standardization. It has two parts: a draft **CEN Workshop Agreement (CWA) project plan** that can be sent to a national standards body or to CEN, and a **skeleton for the technical specification** the workshop would write. Both are starting points. The procedures, forms and fees of the standards bodies change, so check them with the body you apply to before submitting.

## Which parts can become a standard

The proposal has a legal part and a process part. Only the process part fits a standard.

| Part of the proposal | Route | Why |
|---|---|---|
| Version lock, unique document number, pre-review and approval, comprehension check record, signing on the locked version, lifecycle records, status checks by third parties | **Technical standard** (CEN, CENELEC or ETSI) | These describe *how* a contract is reviewed, fixed, signed and tracked. A standard can define them without choosing any law. |
| Value threshold, mandatory notarization, the legal effect of a failed comprehension check, the notary's role | **Legislation** (national law; at EU level through consultations, petitions or initiatives) | Contract law and the notarial profession are largely national matters, and notarial systems differ across member states. A standard can only refer to them. |

The standard should therefore be **law-neutral**: it says what a conforming flow must do and record, and leaves *when* the flow is mandatory and *who* the executing official is (a notary, a registry officer or another authorized person) to national law.

## Part 1: Draft CWA project plan

A CWA is produced in an open workshop that anyone can join, usually within about a year, under the rules of CEN-CENELEC Guide 29. It is not a European Standard (EN), but it is an official CEN deliverable and can later become the basis of a Technical Specification or an EN. A national member must agree to run the workshop secretariat. For a proposer in Turkey, the natural first contact is **TSE**, a full member of CEN and CENELEC.

### 1. Proposed title

*Pre-reviewed, version-locked contract execution with comprehension verification: process requirements and records*

### 2. Background

High-value and complex contracts are often signed under pressure, without being read, and sometimes on a copy different from the one the parties reviewed. Several member states already use parts of a remedy: a mandatory period to read the draft before signing (Germany, Spain), a notarial test before a mortgage deed (Spain), remote notarial acts with digital identity (Estonia, France) and fully digital registers (Denmark). See [Similar Practices Elsewhere](README.md#similar-practices-elsewhere). These practices are not interoperable, and none links review, a version lock, a comprehension check and the later life of the contract under one identifier.

### 3. Objective

To specify a common, law-neutral process for contracts that national law or the parties place under a secured flow, so that:

- every party reviews the same text, and the text that is signed is provably the text that was approved;
- the signer's understanding of the key terms is checked and recorded, not just declared;
- the contract keeps one persistent identifier through annexes, amendments, sub-contracts, renewal, expiration, cancellation, termination, suspension and completion;
- authorized third parties can check a contract's current status;
- systems in different member states can exchange and verify these records.

### 4. Scope

**In scope:** terms and definitions; the process states and allowed transitions; requirements for the document identifier, pre-review, all-party approval, the version lock, the comprehension check record, method-neutral signing on the locked version, lifecycle events, third-party status checks, audit, retention, privacy and accessibility; conformance criteria.

**Out of scope:** value thresholds; which contracts must follow the flow; the legal effect of the records; the qualification and liability of executing officials; specific technologies, products or data formats beyond what interoperability strictly needs.

### 5. Related work and liaisons

The workshop should build on, and not duplicate, existing frameworks. To be confirmed with the relevant committees:

- **eIDAS** (Regulation (EU) No 910/2014, as amended by Regulation (EU) 2024/1183) and the European Digital Identity Wallet: electronic identification, signatures, seals and time stamps.
- **ETSI TC ESI** (Electronic Signatures and Infrastructures): signature creation and validation, trust services, registered delivery.
- **CEN/TC 224**: personal identification and related devices.
- **ISO/IEC JTC 1/SC 37** (biometrics) and **SC 27** (information security, privacy).
- **GDPR** (Regulation (EU) 2016/679) and the **European Accessibility Act** (Directive (EU) 2019/882).
- Notarial and legal bodies: **CNUE** (Council of the Notariats of the EU), **UINL**, national notarial chambers and bar associations.

### 6. Expected participants

Notarial chambers and notaries; land and company registries; ministries of justice and e-government agencies; trust service providers and e-signature vendors; banks and real-estate organizations; consumer organizations and organizations representing older people and people with disabilities; legal-tech companies; universities and researchers in law, usability and security.

### 7. Deliverable and work plan

One CWA, based on the skeleton in Part 2.

| Step | Content |
|---|---|
| Project plan | Draft this plan, collect initial supporters, agree the secretariat with a national body |
| Public announcement | The project plan is published for comment and the workshop is opened to participants |
| Kick-off | Approve the plan, elect the chair, set up drafting groups |
| Drafting | Process model, requirements, lifecycle, conformance; liaison with the bodies in section 5 |
| Public comment | The draft CWA is published for comment |
| Resolution and approval | Comments resolved, CWA approved by the workshop and published by CEN |

### 8. Resources

Participation in a workshop may carry a fee set in the business plan. Funding programmes for participation in European standardization (for example StandICT.eu calls, and the HSbooster.eu support service for research projects) can be checked when the plan is submitted.

### 9. Licensing, rights and route

**The author's position: free for the whole public, paid for a commercial edge.**

- **Public use for everyone: free.** When the state makes the flow mandatory, so that the whole public benefits and the weak are protected, the author asks no fee. In that case the standard should be freely accessible, and its development and governance should be paid from taxes, not from selling the standard.
- **Voluntary commercial use for a competitive edge: revenue share.** As long as the flow is not mandatory, a company that uses it to offer more trust than its competitors uses it as a marketing advantage for part of the market, not for the whole public. This use stays under the revenue share of the [commercial license](COMMERCIAL-LICENSE.md), until a state application makes the flow mandatory.
- **The same rule for the standards body.** A standards body that offers the result free to the whole public receives the contribution free. A body that sells it as a commercial product should acquire it from the author on paid terms.

**What this means for standardization.**

- **CEN does not pay contributors.** Participants in a workshop grant CEN the use of their contributions free of charge, and CEN and its national members sell the published documents. Under the position above, the author therefore contributes only to a deliverable that is **free to access**: a CWA made freely available under the workshop's business plan, or an ETSI deliverable, which ETSI publishes free of charge. The proposal in this repository keeps its own licenses ([LICENSE](LICENSE), [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md)).
- **The revenue share needs a hook.** A published standard can be implemented without the text of this repository, and copyright protects the text, not the idea. The revenue share for voluntary commercial use therefore rests on: (1) commercial use of the text and materials of this repository; (2) a **certification mark** for the flow, so that a company that wants to claim conformance as a mark of trust needs a license, while state applications use it free of charge; and (3) any patent the author may obtain. Standards bodies ask contributors to declare patents essential to the standard and whether they will license them on fair, reasonable and non-discriminatory (FRAND) terms (CEN-CENELEC Guide 8; ETSI has a similar policy).
- Before signing any contribution or IPR declaration, or filing a mark, the texts should be checked by a lawyer.

**Route.** The first step is a CWA through **TSE**, on condition that the CWA is freely accessible. If free access cannot be agreed, the signature-related parts go to ETSI TC ESI instead. Once the CWA is published, it can be proposed through TSE to a CEN technical committee as a Technical Specification or an EN, under the same free-access condition.

**Supporters.** A workshop needs participants from several member states. Before submitting the plan, collect letters of support from at least one notarial chamber, one registry or e-government agency and one consumer organization, ideally from different countries.

## Part 2: Skeleton of the technical specification

The text below is a structure, not a finished requirement set. In the final document, requirements use "shall", recommendations "should" and permissions "may".

### 1. Scope
Process requirements and records for contracts executed under a pre-reviewed, version-locked flow with comprehension verification, through their whole lifecycle. Law-neutral: applicability, thresholds and legal effects are set by national law.

### 2. Normative references
To be completed with the frameworks listed in Part 1, section 5.

### 3. Terms and definitions
- **Secured contract:** a contract handled under this specification.
- **Party:** a person or organization that takes on rights or obligations under the contract.
- **Executing official:** the person authorized by national law to oversee signing (for example a notary).
- **Document identifier:** the unique, persistent identifier of a secured contract.
- **Version:** one complete state of the contract text and its annexes.
- **Locked version:** the version approved by all parties, which can no longer be changed.
- **Comprehension check:** the questions put to each party on the key terms and consequences, and the record of the answers and the outcome.
- **Lifecycle event:** an event after signing that changes the contract or its status.
- **Status:** the current state of a secured contract.

### 4. Process model
States (for example: draft, in review, approved, locked, signed and in force, amended, suspended, expired, cancelled, terminated, completed) and the allowed transitions between them, each with its trigger, its required approvals and the record it produces.

### 5. Requirements

**5.1 Document identifier.** Unique, persistent, never reused; verifiable without revealing the contract content; linked records (annexes, amendments, sub-contracts) derive from it.

**5.2 Pre-review.** Every party can access every version; every change is attributed and kept in the version history; a minimum review time can be configured according to national law or the contract type.

**5.3 Approval and version lock.** Each party approves the final version individually; once all parties approve, the version is locked and any later change is detectable; annexes are locked with the main text.

**5.4 Comprehension check.** Questions cover the key terms and consequences in plain language; each party is asked separately; the record holds the questions, the answers and the outcome (passed, postponed); signing does not proceed after a postponement; the check is accessible to people with disabilities.

**5.5 Signing.** Method-neutral (wet, digital or biometric) as allowed by national law; every signature is provably bound to the locked version; the signer's identity and the executing official are recorded.

**5.6 Lifecycle events.** Each event type from the [Contract Lifecycle](README.md#contract-lifecycle) is recorded under the document identifier with its date, author and grounds; events that change rights or obligations repeat review, approval, lock and the comprehension check for what changed; simple status events need only identity verification.

**5.7 Third-party status check.** Authorized third parties can check the current status by document identifier, with the parties' consent or where the law allows, without receiving the content.

**5.8 Audit and retention.** Every access, change and status transition is logged in a tamper-evident way; records are kept for the retention period set by national law.

**5.9 Privacy and security.** Access limited to the parties, the executing official and those authorized by law; data minimization; biometric data used only for signing.

**5.10 Accessibility and assisted access.** Assisted review at the executing official's office for people without digital access or skills.

### 6. Conformance
Conformance classes (for example: the core flow only; the core flow with lifecycle management; the full flow with third-party status checks) and how conformance is assessed.

### Annexes (informative)
- A. Mapping to existing national practices (from [Similar Practices Elsewhere](README.md#similar-practices-elsewhere)).
- B. Mapping to existing data models, such as the [TM Forum Open API note](TMF-OPEN-API-MAPPING.md).
- C. Example comprehension check questions for a real-estate sale and a power of attorney.
