Türkçe: [USE-CASES.tr.md](USE-CASES.tr.md)

# Delegated Biometric Authentication: Use Cases

> **A working catalogue, not a pitch.** Each use case below states the situation, what happens today, what changes under this proposal, and — most importantly — **what the case does not solve**. The last column is where most proposals about biometric convenience are weakest, and it is the part that decides whether a deployment is defensible to a data-protection authority, an operator's board, or the person at the door.
>
> These are the ideas the [proposal](README.md) rests on. The mechanisms, guarantees and standards gaps are in the [technical analysis](TECHNICAL-ANALYSIS.md) and the [standardization note](STANDARDIZATION.md).

## How the cases are grouped

| Group | Cases | What they have in common |
|---|---|---|
| **Paying** | 1–4 | The reader is a till; the operator is usually a bank or a merchant; the decision is about money |
| **Getting in** | 5–9 | The reader is a gate; the operator usually holds the tenancy or the employment relationship; the decision is about access |
| **Proving** | 10–13 | The reader is a desk or a kiosk; the operator needs a fact or a confirmation; the value is in what the operator does *not* learn |
| **Working and travelling** | 14–19 | The reader is anywhere; the operator is an employer, a school, a carrier, a border authority; the hardest cases, where the benefit and the risk are largest together |

## Part 1: Paying

### 1. Cashless, cardless, phoneless payment at a point of sale

**The situation.** A person stands at a till with a shopping basket and no desire to take out a card or unlock a phone.

**Today.** Paying without a card means a phone: NFC tap, a QR code, a banking app. Each needs the phone in the hand, unlocked, charged, and connected. Some stores already accept a palm or a face, but they do it by collecting a template or a reference image on a server that keeps it — the person trades a permanent credential for a convenience. Contactless card sharing is common, and a phone with a dead battery at the till is a real, recurring failure.

**With this proposal.** The person delegates their face to a payment operator once, from the phone, in the same gesture as granting an app access. At the till they look at the reader. The reader checks the face live, detects a spoof, asks the trust service whether a valid delegation exists for this reader and this payment, completes the payment, and records no biometric. The phone can be in another room.

**What it does not solve.** The phone is still the anchor: whoever has the phone and can satisfy its biometric check can pay. For a shared terminal, the operator learns the amount and the time, and in no-disclosure mode it does not learn who. The security of the payment now depends on the liveness check at the till, so a till that cannot run a compliant check is not a candidate at all — this pushes certified readers into retail, which is a cost the proposal creates rather than removes.

### 2. Micro-payments, transit top-ups and small-value purchases

**The situation.** A kiosk sells a coffee, a top-up, a newspaper, a parking ticket — values so small that a per-transaction fee or a card read is disproportionate.

**Today.** These are cash or stored-value card transactions. Cash is increasingly rejected and is socially costly; stored-value cards are a fraud target because the value is portable.

**With this proposal.** A stored-value grant can be issued for a small, capped amount and a short window, so the delegation itself carries the value limit. A person delegates a ten-dollar standing authorisation to a specific merchant class; the reader draws down against it; the phone is never involved at the transaction. In no-disclosure mode, the merchant cannot tell whether the same person bought the same item yesterday.

**What it does not solve.** A standing value authorisation is a bearer instrument while it is live, and if it is bound to a device that the person does not have on them, the delegation grant becomes the thing worth stealing. The proposal mitigates this with short lifetimes, per-merchant scoping, and revocation, but it does not remove the exposure: a delegation grant is a small, short-lived thing to steal, and standards bodies should treat it as such.

### 3. Public transport and gated mobility

**The situation.** A passenger taps through a turnstile twenty times a week.

**Today.** Transit gates use cards, tickets, QR codes and phones. A single-user pass is often shareable. A phone-based pass needs the phone present and charged — exactly the situation in which a passenger needs the gate most, on a platform, in the rain.

**With this proposal.** The passenger delegates to the transport operator once; the gate verifies face-to-face with no device. Unlinkable tokens suit this case best, because the operator only needs "an authorised passenger", not an identity. The operator gains throughput and does not accumulate a database of which passenger travelled when.

**What it does not solve.** The public transport case is where unlinkability matters most and where it is hardest, because a transport operator has a legitimate interest in anti-fraud analysis that inherently wants to link journeys. The proposal says the operator must not join its logs; in practice, a national transport operator with a fraud team and a data analytics function will want to. **This case should be treated as the hardest to govern, not the easiest**, and a deployment should assume the operator will attempt linkage unless the technical restriction makes it impossible.

### 4. Age verification at the point of sale

**The situation.** A shop must confirm the buyer is over eighteen to sell alcohol or tobacco, or a venue must confirm it before entry.

**Today.** The buyer shows a passport, a driving licence, or a card. Each leaks far more than age: a name, a document number, a photograph, a date of birth, and often a copy the shop keeps. In many jurisdictions the shop is legally obliged to record the document number, which means a nineteen-year-old buying a beer is entered into a retailer's log alongside everyone else.

**With this proposal.** The person delegates an **age attribute** — not an identity — to the retailer. The reader checks the face live, the phone answers with a statement that the holder is over the threshold, and the shop records a yes or a no: no face, no birthdate, no document, no copy.

**What it does not solve.** The shop still needs a lawful basis and a way to handle a refusal, and an age band is not proof of anything else. A failed check at the till of a busy shop is a queue and a refused sale, so the operator will want a manual fallback, and the manual fallback is where the photocopy comes back. The proposal assumes the operator is willing to refuse rather than fall back — that is a business decision, not a technical one, and it is the single biggest practical obstacle in this case.

## Part 2: Getting in

### 5. Building, apartment and flat entry — no card, no key, no phone

**The situation.** A resident comes home without a key, without a card, and possibly without a charged phone.

**Today.** Keys, cards, fobs, codes, phones. Each can be lost, lent, copied, or shared. A building that installs fingerprint or face readers takes on the storage and breach risk of its residents' biometrics, which is precisely the risk residents refuse.

**With this proposal.** The resident's entry app asks the operating system to delegate their face to this building's gateway. The building receives a signed statement of a delegated check, never a template, and it can prove to a door that the person was verified. Residents can grant a guest, a cleaner or a contractor their own scoped, expiring delegation without handing over a key. The building's residents never place their faces in the building's database.

**What it does not solve.** Landlord liability and housing law. A landlord who installs a biometric door system is making a decision with legal weight, and many jurisdictions treat a biometric entry requirement as an employment or tenancy condition. A resident who refuses to delegate has to have a non-biometric path, and the building has to keep one. The proposal is a technical model; it does not make the legal question go away.

### 6. Visitor and contractor access without handing over a key

**The situation.** A plumber arrives. A nurse visits. A courier needs the second floor. Somebody has to let them in, and somebody has to be accountable afterwards.

**Today.** A key is handed over, or a code is shared, or a concierge lets someone in and writes it down. Codes are copied and never changed; keys are copied and never returned.

**With this proposal.** The visitor delegates to the *visitor's own phone* for that building, for a stated window, for the purpose of entry. The building issues the delegation; the visitor's phone holds the biometric; the building's log shows that a delegated check succeeded at that door in that window, under a grant that the building issued and can revoke. The building never asks the visitor for a biometric.

**What it does not solve.** A visitor who has no phone of their own, or no bank account, or no smartphone — the delegation model assumes a device. The proposal's own audience is people with modern phones, and that is a real equity limit. It also assumes the visitor can receive a delegation before they arrive, which is a scheduling problem the building has to solve.

### 7. Short-term rental and hospitality access

**The situation.** A guest arrives at an apartment they booked on an app and has never seen.

**Today.** A code on a keypad, or a lockbox, or a physical key handed over at a desk. A code is shared with everyone in the group and stays valid after it should not.

**With this proposal.** The guest delegates to the property before arrival, for the length of the stay, for that door. At the door they look at the reader. The property issues the delegation, the guest's phone holds the biometric, and the guest can end the delegation at checkout without the property needing to ask for anything back.

**What it does not solve.** The property still has to verify who the guest *is* against the booking, which is a document check, not a biometric one — and the document check is where the personal data actually goes. The delegation says "the person who booked is at the door", which is not the same as "this is the person from the booking". The gap between those two is a general identity-proofing problem and is not solved here.

### 8. Hotel check-in and keyless room access

**The situation.** A guest arrives at 11pm, in a queue, and has to show a passport to a person at a desk.

**Today.** The hotel reads a passport or ID card, sometimes scans it, sometimes photocopies it, and stores it for years. Room keys are cards that get lost. The front desk handles every guest manually.

**With this proposal.** The guest delegates to the hotel before arrival. At the desk they look at the reader; the hotel gets a verified identity statement and, where the law allows, a minimum set of attributes. The room door is unlocked by the same delegation, so there is no key to lose, no card to hand over, and no passport copy in a filing cabinet.

**What it does not solve.** Hotels are, in many jurisdictions, legally required to register every guest with the authorities, and that registration requires identity documents. A delegated check does not remove that duty, and in practice the hotel will still scan the document for the register. The proposal may reduce what the *property* keeps beyond the legal minimum, but it does not remove the legal minimum, and it should not be sold as if it did.

### 9. Workplace access, and attendance that people trust

**The situation.** A shift starts. The employee must prove they are the employee, and must prove they were there.

**Today.** Badges, PINs, fingerprint readers, face terminals. Fingerprint and face terminals collect the biometric. Badge sharing is endemic. The record of who was present is often the employer's private data, and the worker has little visibility into it.

**With this proposal.** The employer delegates access per employee, per site, per time window. Attendance is recorded from a delegated check the worker can see, revoke and correct. A shared badge disappears. The employer gets "this worker was present" without the face.

**What it does not solve.** **This is the most legally difficult case in the set, and it should be presented as such.** Employment law in most jurisdictions treats biometric monitoring at work as a fundamental-rights matter: consent obtained in an employment relationship is contested, and a worker who declines may be lawfully required to use another method, which in practice means a badge. The proposal gives the employer a *less intrusive* technical option, which is a genuine benefit in a data-protection impact assessment, but it does not change the employment relationship and it must not be presented as doing so.

## Part 3: Proving

### 10. Hospital and clinic patient identification at reception

**The situation.** A patient arrives. Staff ask for the date of birth, a card, or an ID, and ask again when the first answer is wrong.

**Today.** Identity errors in hospitals are a real patient-safety problem. The administrative workaround — a photo ID card, a wristband, a date of birth — is a weak identifier that fails in exactly the crowded conditions where mistakes happen.

**With this proposal.** The patient delegates to the clinic, the reader verifies them live, and the check is bound to the patient's own record. The clinic learns the person is who they say they are without collecting a copy of their identity document, and misidentification falls without creating a new health-data pool.

**What it does not solve.** A health setting is where a delegation grant should probably *not* be used at all, in many cases. Health data is among the most sensitive categories, the patient's condition may affect the biometric check (a mask, a bandage, a tremor), and a wrong identification at a hospital counter is a safety incident. A "prove you are who you say you are" check on a *staff* member, at a controlled door, is a safer application than one on a patient at a busy reception. The proposal should be read as making the *staff* and *visitor* case easy, and the patient case one that needs much more care than its simplicity suggests.

### 11. Civic identity and voting at the polling station

**The situation.** A voter reaches a polling station and must be identified against the electoral roll.

**Today.** Paper lists, ID cards, and in some places an electronic roll with a card and a PIN. Identity checks are manual and slow, which produces long queues and, in some countries, depress turnout. The election authority holds the roll.

**With this proposal.** The voter delegates to the polling station's readers before election day, with a scope that ends when the polls close and cannot be reused. The check confirms eligibility without the station learning more than it needs, and the delegation is short-lived by design.

**What it does not solve.** **Elections are the hardest case for legitimacy, not the easiest.** The verifier's job is to establish that a specific person is entitled to vote, and a delegated check establishes that *a* person is authorised — by whom, and under what identity, is a separate question. A scheme that lets a delegation stand in for a roll check would be a scheme that could be abused to vote twice, or by the wrong person, and an election authority would need to reason about that before adopting anything. The proposal's contribution here is narrow: it could replace *how the voter is identified at the desk* — the slow, manual, error-prone part — without touching the roll.

### 12. Age and capability checks for services and benefits

**The situation.** A service must confirm the applicant is over eighteen, or over sixty-five, or is a student, to decide eligibility.

**Today.** A passport, a driving licence, a utility bill, a screenshot of a student card. Each is a document that gets emailed, photographed, or stored in a case-management system, and each ends up in a workflow nobody designed for document retention.

**With this proposal.** The applicant delegates an **attribute** — an age band, a student status — to the service. The service receives the fact and not the identity behind it, and cannot ask for more. The document that proved it stays where it is.

**What it does not solve.** The attribute has to come from somewhere authoritative, and *that* authority becomes a holder of the underlying evidence. This case relocates the problem rather than dissolving it, and the relocation only helps if the attribute issuer is held to the same standard as the original document holder — which is a governance question with no technical answer.

### 13. Proof of presence for insurance, warranty and claims

**The situation.** An insurer needs to confirm that a loss occurred, or a manufacturer needs to confirm a device was damaged, or a warranty claim needs to establish the claimant is the owner.

**Today.** Photographs, videos, serial numbers, an assessor, a claim form signed on paper. The claim file accumulates images of the claimant and their property.

**With this proposal.** The claimant delegates to the insurer at the point of loss. The reader verifies them live, the insurer receives a verified-presence statement attached to the claim, and the file contains a delegation reference rather than a biometric. The insurer can distinguish a genuine live claim from a reused photo, which is a real fraud problem today.

**What it does not solve.** A delegated check proves the person is present, not that the loss happened. It addresses impersonation and photo reuse, and nothing about the substance of the claim. It also does not solve the insurer's need to tie the claimant to the *policy*, which is a document-matching problem.

## Part 4: Working and travelling

### 14. School, campus and examination attendance

**The situation.** A student arrives at a school or a university, or sits an examination.

**Today.** Registers, ID cards, a fingerprint reader, a face terminal — the last two collecting the biometric, and both creating a record of a minor's biometric in a system run by a school with limited security capacity.

**With this proposal.** The student delegates to the institution for a term, per building. Attendance is recorded without a biometric ever reaching the school's server. A student's biometric — which cannot be changed if it leaks — never becomes a school record, and the school's breach surface shrinks from "a database of children's faces" to "a list of delegations".

**What it does not solve.** Consent from a minor, or from a parent, which is governed by law that varies widely and is not settled by a technical model. There is also a well-known equity problem with facial recognition in schools: error rates differ across demographic groups, and the students most affected are often the ones least able to challenge a record. A delegated check inherits the same error profile unless the reader is certified and the rates are published.

### 15. Seamless borders — crossing a frontier without showing a passport

**The situation.** A traveller crosses an internal or external border without presenting a passport, because their own device answers the question.

**Today.** The passport is the instrument, and it is a heavy one. At an internal border within the Schengen area, routine passport presentation has largely been abolished — travellers are not normally required to show a document at an internal crossing, though spot checks remain lawful and temporary internal controls can be reimposed by any member state, in which case a valid passport or identity card must be shown. At external borders, the trend is the opposite: the EU's Entry/Exit System, which became fully operational in April 2026, registers non-EU nationals biometrically at the outer frontier, and biometric e-gates are spreading. So the direction of travel is *more* biometric enrolment at the border, not less, and the document is being joined by a template rather than replaced by one.

**With this proposal.** The traveller delegates to the border authority — or, more likely, to the carrier or the transit operator that operates the crossing — and crosses on a live check. The traveller's template never leaves their device, so the authority verifies the crossing without enrolling a template it would have to keep for years. No passport is presented, and no face is stored. The check is the traveller's own device saying "I am present, and I am the person who holds the delegation", which is a completely different privacy proposition from a database of every traveller's face at every crossing.

**Why this is the most natural fit in the whole set.** Three things line up at once. The reader is a third party — the authority or the operator — so it is exactly the case the model is built for. The template is the one thing nobody wants a state to hold, because a face enrolled at a border is held for the years a border record lives, and the traveller cannot change it. And the phone-absent property matters most here precisely because a traveller crossing a border is typically walking, in a queue, with a bag, and not holding a phone up to a camera. The everyday proposal — hold your phone to the reader — is at its weakest exactly where the privacy case is at its strongest, and the delegated model is the version that works.

**What it does not solve.** **This is the highest-stakes case in the set, and the one where the honest answer is that the technical model is the easy part.** A border crossing is a security function, and the reader's decision is not "is this person who they say" but "is this person who they say *and* are they allowed to be here". The delegation answers the first question; the second needs a watchlist, a visa status and a travel record that the delegation does not carry. A border authority would therefore be running **two** systems — a live presence check and a document-based eligibility check — and the question is whether the live check adds anything to a crossing where a document check already happens, or whether it adds a second thing to fail. There is also a legal layer this model cannot touch: international border agreements prescribe specific documents and specific verification sequences, and a live biometric check is not among them. And the failure mode is asymmetric in a way that building access is not: a false accept at a turnstile loses a sale, while a false accept at a border is a security incident with consequences that no published error rate can bound. **The right conclusion is that this case is the best demonstration of what the model is *for*, and the worst place to pilot it.**

### 16. Crossing an internal border, and a border between two jurisdictions that do not share a system

**The situation.** A traveller crosses between two countries that have no common identity infrastructure — a freight driver between two member states of a customs union, a commuter across a land border, a train crossing between national rail networks.

**Today.** A document check at a fixed point, or nothing at all. Where there is no shared system, every crossing is a bilateral negotiation about what counts as identification, and the traveller carries whatever both sides accept.

**With this proposal.** The traveller's device holds the delegation, and the crossing operator on either side can verify the same person with the same reader standard. The bilateral negotiation is replaced by a shared protocol: the two sides agree on the standard, not on a list of acceptable documents. The traveller crosses with a device and nothing else, and neither state holds a template.

**What it does not solve.** The two sides still have to agree on *who counts as a traveller* — that is a political and legal question about citizenship and residency, and no protocol settles it. The proposal makes the technical handshake cheap and standard; it does not make two jurisdictions agree. A sensible deployment starts with two operators that already share a system and adds a third, rather than starting with two strangers.

### 17. Airline and rail boarding

**The situation.** A passenger boards a flight or a train.

**Today.** A boarding pass, a barcode, a passport, an ID check at the gate, and a face comparison at the gate in some airports. The airline receives the passenger's identity document number and stores it for a defined period.

**With this proposal.** The passenger delegates to the carrier, the gate verifies live, and the airline receives a verified-presence statement rather than a document copy. For a rail operator with no legal need for identity — many regional services do not check — the delegation is the whole point: it gives a verified passenger without an identity record at all.

**What it does not solve.** Aviation security regimes are prescribed by regulation and by international agreement, and they specify document checks, passenger name records, and watchlist matching. A delegated presence check is not a substitute for any of these, and the security value of a live check is unrelated to the privacy value. This case is a plausible *addition* to boarding, not a replacement for it.

### 18. Shared and temporary workspaces, co-working and short-term offices

**The situation.** A freelancer or a small team works from a shared office for a day.

**Today.** A reception desk, a member card, a PIN, a QR code, a phone call. Access is granted by a person, and the operator keeps a record of who entered.

**With this proposal.** The team delegates to the workspace for the day. The reader verifies each member, the workspace knows the delegation was valid, and the operator keeps an event record rather than a biometric. A coworking operator — often a small business with no security team — has the smallest possible exposure.

**What it does not solve.** A shared workspace's real risk is a stranger being let in, and a delegation model does not address it: it verifies people who *have* delegations, and says nothing about people who do not. The receptionist's judgement is still load-bearing.

### 19. Emergency and welfare access, and access for people who cannot use a reader

**The situation.** A responder needs to enter a building in an emergency. A person with a tremor, a mask, a disability, or a dark-skinned complexion faces a reader that does not recognise them.

**Today.** Break-glass overrides, master keys, and a fallback list of people who are exempt from biometric entry. Exempt lists are rarely published, which means people discover their exemption when they are standing at a door.

**With this proposal.** The reader's error rates are published per demographic group and per attack class, the fallback path is part of the design rather than an exception, and the grant list is visible to the subject. A responder can hold a delegated grant that bypasses the normal check, and a person for whom the reader has a high false-reject rate can be enrolled in a path that does not rely on it.

**What it does not solve.** Nothing here fixes the underlying problem that face recognition performs unevenly. It makes the unevenness *measurable and therefore actionable*, which is necessary but not sufficient. **This is the case that the rest of the set should be read against:** a proposal that makes biometric access cheaper and easier, without naming the people for whom it fails, is a proposal that makes exclusion cheaper and easier too.

## Sources for the border cases

The border cases rest on public framework documents rather than on pilot reports, because no "seamless borders" pilot with passport-free crossing on a live biometric check was found in the public record. What the record does show is the legal and operational frame:

| Fact | Source |
|---|---|
| Routine passport presentation is not required at internal Schengen borders, but police spot checks remain lawful; a member state reintroducing temporary internal controls requires a valid passport or identity card | [Your Europe — travel documents for EU nationals](https://europa.eu/youreurope/citizens/travel/entry-exit/eu-citizen/index_en.htm) |
| Internal controls may be reimposed for 30 days, extendable to 6 months, then 2 and 2.5 years, with notification and a necessity and proportionality assessment | [The Schengen area](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=LEGISSUM:l33020&frontOfficeSuffix=/) · [Commission Implementing Decision (EU) 2025/315](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=OJ:L_202500315) |
| The EU's Entry/Exit System, which registers non-EU nationals biometrically at external borders, became fully operational on 10 April 2026 | [Smart Borders — European Commission](https://home-affairs.ec.europa.eu/policies/schengen/smart-borders_en) |

**The honest reading of that last row is the point of the case.** The direction of travel at the external border is *more* biometric enrolment, not less: the trend is to add a template to the document, and the Entry/Exit System extends that. The proposal offers the opposite — a crossing that verifies a person without the state keeping a face — and the reason it has not happened is not that nobody thought of it, but that the enabling legal instruments for it do not exist. **That is a standards and treaty gap, not a technology gap**, and it is a different kind of problem from the one the rest of this catalogue describes. No claim is made here that a seamless-borders pilot exists.

## What the set shows

Across nineteen cases, three patterns hold:

1. **The privacy gain is largest where the operator's need is smallest.** Transit gates and shared workspaces need to know "authorised", not "who", and the no-disclosure mode is the natural fit. Hotel reception and hospital reception need the opposite, and gain less.
2. **The legal difficulty is roughly the inverse of the technical difficulty.** Building access is easy technically and legally contested; a voting check is easy technically and nearly impossible legally; border control is neither.
3. **The cases that fail hardest are the ones that would benefit most from a fallback.** Every entry above states a case the model does not cover. A deployment that removes the fallback removes the thing that made the fallback necessary.

## What a first implementation should be

If this were being built rather than proposed, the order would be:

| Phase | Case | Why this one |
|---|---|---|
| 1 | **5 — building access**, one building, one operator, one reader type | The operator is the user's own landlord, so the risk is contained and the value — no keys, no cards, no template in the building's database — is easiest to see and to explain |
| 2 | **6 — visitor access** | Introduces delegation *to* a third party's phone, which is the mechanism's real test, without yet moving money |
| 3 | **4 or 12 — attribute mode** | Introduces a mode where the operator learns less than it does today, which is the case where the privacy claim is strongest and most measurable |
| 4 | **1 — payment** | Only after the above have shown that delegation works at all, because a payment failure is a public failure |

Deliberately not first: transport (3), workplace (9), voting (11) and the border cases (15 and 16). Each has a specific unresolved problem — operator linkage, employment consent, electoral legitimacy, or the fact that a border crossing is a security function rather than an access decision — and each would be the first thing to go wrong. **The seamless-borders case is worth stating separately: it is the best demonstration of what the model is for, and the worst place to pilot it.** A pilot that failed at a border would be a public failure of a security system, and no published error rate bounds the consequences.
