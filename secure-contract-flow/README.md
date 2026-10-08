Türkçe: [README.tr.md](README.tr.md)

# Secure Contract Flow: Pre-Reviewed, Version-Locked Contracts and a Real "Read and Understood" Check

## Summary

High-value and complex contracts are often signed under pressure, without being read, and sometimes on a different copy from the one the parties reviewed. This proposal introduces a **secure contract flow**:

- **A value threshold.** Private contracts about money or valuable assets above a yearly-reviewed limit are valid only when notarized.
- **Online pre-review.** Before signing, the contract goes through a pre-review stage on the national e-government portal. Every party can read it at their own pace, propose new versions and approve the final text.
- **A version lock.** The approved version is locked under a **unique document number**, and the notary prints exactly that version.
- **A real comprehension check.** At signing, the notary asks each party questions about the key terms, so "I have read and understood" becomes something actually verified.
- **Flexible signing.** Depending on what is available, the parties sign with a wet signature on paper, or with a digital or biometric signature on the digital document. The version lock and comprehension check apply the same way.

**In short: pre-approval before signing, and a comprehension check during signing.**

## Problem

- **High-value private contracts have no safeguard.** Contracts involving large sums of money or valuable assets can be made privately, with no independent check of what the parties agreed to.
- **People sign what they have not read.** Contracts can run to dozens of pages and use dense, ambiguous language. At the notary, people often write "I have read and understood" and sign a contract they never actually read.
- **The copy you read may not be the copy you sign.** When drafts are exchanged informally, a party can review one version and sign another, intentionally altered or not.
- **"Are you of sound mind?" is answered in a second.** Someone who has just had an accident, a bereavement or another severe shock may say "yes", yet be unable to read, grasp and judge a contract on the spot.
- **Powers of attorney carry the same risk.** People grant powers without knowing exactly what they authorize, and the same gap appears again in the mandate given to a lawyer afterwards.

## Proposed Solution

1. **Value threshold for private contracts.** Contracts about money or valuable assets get a legal ceiling for private agreements, set nationally and **reviewed every year** in line with inflation and social conditions (for example, around 500,000 Turkish lira at today's values). Above the ceiling, a contract is **not legally valid without notarization**.
2. **Online pre-review stage.** For contracts above the threshold, and for critical, long or semantically complex contracts, the draft is submitted to the national e-government portal or a similar central state system. Every party can download, read and examine it in detail, **with enough time**.
3. **Versioning between the parties.** The parties can propose new versions or edits through the system until they agree. Every version is kept with its history, so it is always clear who changed what.
4. **Mutual pre-approval and a unique document number.** When all parties approve the final version online, the system locks it and assigns a **unique document number**.
5. **The notary uses exactly that version.** Using the document number, the notary prints the locked contract or opens it as a digital document. Reading one copy and signing another becomes impossible.
6. **Comprehension check at signing.** The notary asks each party questions about the key terms and possible consequences of the contract they already reviewed. "I have read and understood" becomes an actual check, not a formality.
7. **A practical answer to "sound mind".** A person who had time to study the contract beforehand and can answer questions about it shows real understanding. That gives the notary a well-grounded judgment about their capacity. Someone in acute shock who cannot answer has signing postponed, not forced.
8. **The same flow for powers of attorney and lawyer mandates.** The person granting a power of attorney reviews exactly what they authorize beforehand and answers comprehension questions at signing. A similar step applies when that power is passed on to a lawyer.
9. **Signature method that fits the available means.** The signature can be a wet signature on paper, or a digital or biometric signature on the digital document, depending on what the notary and the parties can use. Whatever the method, the signature is applied only to the locked version and only after the comprehension check.

## How It Works

![Secure contract flow: below a yearly-reviewed value threshold a private contract is enough; above it the contract is uploaded to the national e-government portal, the parties review it and propose versions, all approve the final version, it is locked under a unique document number, the notary uses exactly that version, asks comprehension questions and only then takes the signature, whether wet, digital or biometric. The same flow applies to powers of attorney and lawyer mandates](assets/secure-contract-flow.svg)

| Situation | Today | With the secure contract flow |
|---|---|---|
| A long, complex contract | Read in minutes at the notary, or not at all | Reviewed beforehand, at each party's own pace |
| Changes during negotiation | Drafts circulate informally | Every version is kept with its history in one place |
| The copy that gets signed | May differ from the one that was read | Exactly the locked version, printed by its unique document number |
| "I have read and understood" | A sentence written by hand | Confirmed by answers to questions on the key terms |
| "Are you of sound mind?" | A one-word answer, even in shock | Judged from real understanding; signing is postponed if needed |
| A power of attorney | Granted without knowing its full scope | Reviewed beforehand and checked at signing |
| How the contract is signed | Usually a wet signature on paper | Wet, digital or biometric signature, always on the same locked version |

## Implementation & Phasing

1. **Pilot:** start with real-estate sales and powers of attorney, where the stakes and the number of disputes are high.
2. **All contracts above the threshold:** extend the flow to every private contract above the value ceiling.
3. **Lawyer mandates and other critical contracts:** apply the same pre-review and comprehension step to lawyers' mandates and to contracts designated as critical regardless of value.
4. **Yearly threshold review:** set up a regular, transparent mechanism to update the value ceiling for inflation and social conditions.

## Stakeholders & Benefits

- **Citizens:** they know what they sign, and the copy they read is the copy they sign.
- **Vulnerable people:** elderly people and people in shock or under pressure are protected from signing what they do not understand.
- **Notaries:** a clear, documented basis for confirming understanding and capacity.
- **Lawyers:** clients who understand the scope of the mandate they give.
- **Courts:** fewer disputes over "I didn't read it", altered copies or capacity at signing.
- **Banks, real estate and businesses:** more reliable contracts and fewer costly challenges later.

## Risks & Mitigations

| Risk | Mitigation |
|---|---|
| People without digital access or skills | Assisted access at the notary office, with staff helping to review the contract online |
| Privacy of contract contents | Only the parties and the notary can access the contract, and every view and change is logged |
| Extra time and cost | The flow applies only above the threshold or to critical contracts; everyday small agreements stay simple |
| The comprehension check feels like an exam or is unfair | Plain-language questions on the key terms and consequences only; accessible for people with disabilities |
| Security of digital or biometric signatures | Only certified signature methods, with identity verified by the notary; biometric data is used only for signing and protected by law |
| Pressure from the other party | Each party is asked the questions separately |
| The threshold loses meaning with inflation | Yearly review and indexing of the value ceiling |

**Keywords:** contract law, notary, informed consent, digital contract review, version control, unique document number, digital signature, biometric signature, power of attorney, legal capacity, consumer protection, e-government, legal tech

## Origin

Originally proposed by Merih İlgör (October 2026); published here as an open project idea. Change history: [CHANGELOG.md](CHANGELOG.md).

## License

This work is licensed under CC BY-NC-SA 4.0 for non-commercial use; see [LICENSE](LICENSE). Commercial use requires a revenue-share agreement; see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md). Modified versions may not be sold or transferred to third parties as new ideas, and patent-like rights are reserved; see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md#derivatives-improvements-and-idea-protection).
