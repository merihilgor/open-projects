Türkçe: [TMF-OPEN-API-MAPPING.tr.md](TMF-OPEN-API-MAPPING.tr.md)

# Secure Contract Flow: Mapping to TM Forum Open APIs (Implementation Note)

> **Non-binding implementation note.** The [proposal](README.md) describes the what and the why and does not prescribe any technology. This note is for teams that want to build the flow on [TM Forum Open APIs](https://www.tmforum.org/oda/open-apis/). It shows what maps directly, what needs an extension, and what has no counterpart at all. It was checked against the published **TMF651 Agreement Management API v4.0.0** schema. Check it again against the version you target (v5 exists).

## Summary

- **The basic model fits.** A contract is a TMF651 `Agreement`, a contract type is an `AgreementSpecification`, and TMF651 even has a `documentNumber` field for the unique document number.
- **The state model fits, because TMF651 does not define one.** `status` is a free string (the spec only gives "in process, approved and rejected" as typical values), so the lifecycle states of this proposal can be defined as the implementation's own state model.
- **The core safeguards are missing.** The version lock, the pre-review negotiation, the comprehension check, the notary step, the typed links between contracts (amendment, sub-contract, renewal), termination details and audit/consent rules have no native counterpart. They need extensions (`@type`, `@baseType`, `@schemaLocation`, `characteristic`), companion APIs and rules enforced by the system itself.

## 1. Direct mappings

| Proposal | TMF Open API (TMF651 v4 unless stated) | Note |
|---|---|---|
| Contract | `Agreement` | |
| Contract type or template | `AgreementSpecification` | Template per contract type (real-estate sale, power of attorney, lease…) |
| Unique document number | `Agreement.documentNumber` | Typed as **integer**; a formatted or check-digit number needs an extension field |
| Version | `Agreement.version` | A string only; see gaps for version history |
| Contract text and terms | `agreementItem[].termOrCondition[]` (`description`, `validFor`) | Items are designed around product offerings; for legal contracts, use terms only |
| Parties | `Agreement.engagedParty[]` (`RelatedParty` with `role`) → TMF632 Party, TMF669 Party Role | |
| Notary, witnesses, lawyer | `engagedParty[]` with a role such as `notary` | Agreement has no separate `relatedParty`, so non-contracting roles share this list |
| Effective period, expiration date | `Agreement.agreementPeriod` (`startDateTime`, `endDateTime`) | |
| Completion | `Agreement.completionDate` | Typed as a time period, not a single date |
| Signatures | `Agreement.agreementAuthorization[]` (`date`, `state`, `signatureRepresentation`) | See gaps: no signer reference, no biometric value |
| Status (in review, locked, active, expired, cancelled, terminated, completed…) | `Agreement.status` | Free string, so the values can be defined freely |
| Linked contracts | `Agreement.associatedAgreement[]` (`AgreementRef`) | Untyped; see gaps |
| Annexes and attachments | TMF667 Document Management (`Document` with `attachment`), referenced via an extension | `AgreementSpecification` has `attachment`, but `Agreement` does not |
| Notifications (expiry, termination, any status change) | `AgreementStateChangeEvent`, `AgreementAttributeValueChangeEvent` (hub/listener) | |
| Creating, amending, querying | `POST/GET /agreement`, `GET/PATCH/DELETE /agreement/{id}` | See gaps on PATCH and DELETE |

## 2. Lifecycle events

| Event | Mapping | Fit |
|---|---|---|
| Draft and pre-review | `Agreement` with `status` = e.g. `inReview`; versions via `version` | Partial: there is no negotiation or proposal object |
| All-party approval | `agreementAuthorization[]` entries with `state` = approved | Partial: no link to which party approved |
| Version lock | `status` = `locked`, plus a content hash in `characteristic` | **Gap:** nothing in TMF prevents later changes |
| Signing | `agreementAuthorization[]` with `signatureRepresentation` | Partial: wet or digital is covered; biometric, the certificate and the signer need an extension |
| Attachments and annexes | TMF667 `Document` linked by an extension reference | Partial |
| Amendment or addendum | New `Agreement` (or new `version`) linked by `associatedAgreement` | **Gap:** the link has no type ("amends") |
| Sub-contract | Child `Agreement` linked by `associatedAgreement` | **Gap:** no parent/child type and no "cannot exceed parent" rule |
| Assignment or change of party | `PATCH engagedParty` | **Gap:** no record of who transferred what to whom, or when |
| Renewal or extension | New `agreementPeriod` or a linked agreement | Partial: renewal type and reminders need an extension and a scheduler |
| Expiration | `agreementPeriod.endDateTime` and `status` = `expired`, plus a state change event | Good, but the status change must be triggered by the system |
| Cancellation (mutual) | `status` = `cancelled` plus approvals in `agreementAuthorization` | Partial: no reason or consequences (refunds, penalties) |
| Termination (one-sided) | `status` = `terminated` | **Gap:** no grounds, notice date, notice period or terminating party |
| Suspension | `status` = `suspended` | **Gap:** no suspension period or cause; deadlines can't be recalculated |
| Completion | `completionDate` and `status` = `completed` | Good |
| Dispute | `status` = e.g. `disputed` | **Gap:** no history or evidence model (see audit) |

## 3. What is missing (absence mapping)

| Element of the proposal | Exists in TMF? | Suggested approach |
|---|---|---|
| Value threshold (yearly-reviewed limit) | No | Business rule outside the API; contract value as a `characteristic` |
| Online negotiation (proposed versions, comments, who changed what) | No | Extension resource (e.g. `AgreementVersion` / `ChangeProposal`) or a document-collaboration service |
| Version lock (immutable text) | No | Content hash plus a system rule that rejects PATCH on locked fields; locked text kept as an immutable TMF667 `Document` |
| Comprehension check (questions, answers, result, postponement) | No | Extension resource linked to the agreement and the party; only the result is stored, not personal answers beyond what the law requires |
| Notary act (identity verification, capacity judgment, notary's own record) | No | Notary as an `engagedParty` role plus an extension for the notarization act; identity via TMF720 Digital Identity or a national e-ID |
| Signer of each signature | No (`AgreementAuthorization` has no party reference) | Extend `AgreementAuthorization` with a party reference, certificate and signature type (wet, digital, biometric) |
| Typed links between contracts (amends, sub-contract of, renews, replaces) | No (`AgreementRef` is untyped) | Extension: relationship type on `associatedAgreement`, similar to `specificationRelationship` on the spec |
| Termination details (grounds, notice date, notice period, terminating party) | No | `characteristic` or an extended `Agreement` subtype |
| Suspension period and cause | No | `characteristic` with a time period |
| Status transition rules (which change is allowed when) | No (no state model) | Implementation's own state machine, published as an API profile |
| Proportionality (which events need the notary) | No | Workflow rules; orchestration e.g. with TMF701 Process Flow |
| Audit trail (every view and change) | No | Event store or audit log; TMF688 Event Management for distribution |
| Third-party status check with consent | No | Consent via TMF644 Privacy Management, or a national consent service; a read-only status endpoint |
| Retention and deletion rules | No | Policy outside the API; disable `DELETE /agreement/{id}` for signed contracts |
| Formatted document number | Partial (`documentNumber` is an integer) | Extension field for the formatted number |

## 4. Points of conflict

- **`DELETE /agreement/{id}`:** a signed contract must never be deleted. The API profile should forbid it, and cancellation or termination should be a status change.
- **`PATCH /agreement/{id}`:** it allows any field to change. After locking, the system must reject changes to the text, parties and terms, and every change must go through a new version or a linked agreement.
- **Product-centric items:** `AgreementItem` is built around `productOffering` and `product`. For legal contracts, use `termOrCondition` and leave the product references empty.

## 5. Beyond TMF

TMF Open APIs come from telecom and fit an enterprise contract platform well. For a national e-government and notary system, the legal layer matters at least as much: e-signature regulation (for example eIDAS and its ETSI standards), legal document formats, national notary law and data protection law. TMF can be the data and integration model, but these set the rules.

## Sources

- [TMF651 Agreement Management API v4.0.0 schema (tmforum-apis on GitHub)](https://github.com/tmforum-apis/TMF651_AgreementManagement)
- [TMF651 in the TM Forum Open API directory](https://www.tmforum.org/oda/open-apis/directory/TMF651)
- [TM Forum Engage: "TMF651 agreement management: what states to use"](https://engage.tmforum.org/discussion/tmf651-agreement-management-what-states-to-use)
- [TMF667 Document Management API User Guide v4.0.0](https://www.tmforum.org/resources/guidebook/tmf667-document-management-api-user-guide-v4-0-0/)
