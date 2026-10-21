Türkçe: [README.tr.md](README.tr.md)

# Delegated Biometric Authentication: Pay at the Register and Enter the Building by Face, Without the Phone and Without Sharing Your Biometrics

## Summary

Biometric authentication is convenient, but people do not want to hand their face to every organisation. A biometric template is a permanent credential: unlike a password, it cannot be changed after it leaks, and a database of faces is a target for every party who ever collects one. So people refuse, or they accept a photo of their face on a form and never learn who keeps it.

But people already trust the biometric system on the phone in their pocket. That is the unlock on their phone, the app they open every day, the payment they make. If those trusted systems offered **delegated biometric authentication** — a way to delegate a face check to a named reader, for a named purpose, for a limited time, revocable at any moment — then:

- **A person pays at a cash register by face, with no phone and no card.** The phone stays in their pocket. The register verifies them face-to-face and the payment completes.
- **A person enters their building or flat without a card, a key and a phone.** The resident's entry app has delegated their face to the building's gateway, and the building never receives, stores or even sees a biometric template.
- **The reader never gets the biometric.** The phone holds the template in its secure hardware, the reader checks a short-lived signed proof instead, and the proof is useless anywhere else.

**In short: your phone's biometric system can vouch for you at a register or a gate while it stays in your pocket, and no organisation ever receives your biometric data.**

The proposal turns a per-app toggle users already know — press and hold an app, switch on "Require Face ID" — into a **per-service delegation the operating system manages for you**, for readers that have never been integrated with the phone at all: a point-of-sale terminal, an apartment gate, a turnstile.

## Problem

- **Biometric data is permanent, passwords are not.** A leaked password can be changed; a leaked face template cannot. The user has to trust that a collector will protect it forever, from a breach, from an insider, from a subpoena, and from a change of ownership.
- **The trust is already placed elsewhere.** People trust the phone's Face ID or fingerprint with their whole digital life. That trust does not extend to the bank's app, the gym's app or the building's app, because each of those handles the biometric data separately.
- **Every new holder is a new breach surface.** A café that accepts a face, an insurer that stores a photo, a landlord that keeps a fingerprint log: each one becomes a database nobody asked to hold.
- **Today, "use your face" means handing over the face.** Existing biometric payment and access systems either upload a template or a reference image to a server, or ask the user to enrol with a service that keeps it. The user cannot see the difference and cannot avoid it.
- **Cardless still means carrying something.** Contactless cards, phone taps and QR codes all work, but they all need a token in the hand or the pocket, and they all fail in the same way: a shared card is passed around, a phone battery dies at the gate, a screen is cracked.
- **Access for the phone-absent case is weak.** A person who leaves the phone at home, in a bag, or with a guest still needs to get in. Today that means a key, a code, a fob, or a receptionist.
- **Delegation is not solved by today's cross-device flows.** Existing cross-device sign-in still needs the registered device present, doing the verification, in the room. Delegating *a biometric check* to a third-party reader, with the phone absent, is a different problem.

## Proposed Solution

1. **The phone becomes a trusted broker.** The operating system, not a company, holds the biometric template inside secure hardware. The phone is the party that says "this person is the person who delegated to you".
2. **Delegation is granted per reader, from the phone, by the user.** The user picks the services that are allowed to delegate their face — a payment operator, their building, a transport operator — exactly like switching on "Require Face ID" for an app. The operating system does the work, so the reader's vendor never has to integrate with the phone.
3. **Each grant is scoped, time-limited, revocable and auditable.** A grant names the reader, the purpose, the limit and the expiry. The user can see every grant, every use and every revocation, and can withdraw a grant at any time; the next verification at that reader fails immediately.
4. **The reader verifies a proof, never a template.** The reader receives a short-lived signed statement that a delegated biometric check succeeded, bound to that reader and that operation. The statement cannot be replayed at another reader, cannot be copied, and carries no biometric data.
5. **The biometric check happens where the user is.** The reader performs the face check live, in front of the user, with presentation-attack detection so a photo or a mask is not accepted. The template stays in the phone; the reader sends a challenge, the phone answers it, and the reader verifies the answer.
6. **The reader is not the only holder of the trust anchor.** The grant is registered with a trust service, so a reader can check that a delegation is genuine and still valid without the phone being online or the user being asked to install anything.
7. **Not every use case needs to identify the person.** Where the reader only needs to know "an authorised user", the grant can be issued as an **unlinkable, single-use token** so the reader cannot tell whether the same person came yesterday and today. Where the reader needs to know who, the grant carries a scoped credential the reader can verify without learning anything else.
8. **Nothing is enforced on the user by default.** Delegation is always opt-in, always visible and always revocable. There is no central biometric database, because there is no central biometric to keep. A user who delegates nothing keeps today's behaviour exactly.
9. **One open standard, not vendor silos.** The same grant, proof, verification and revocation rules work for every operating system, every reader and every operator, so an operator does not rebuild the flow per vendor.

## How It Works

![Delegated biometric authentication: the user grants a face delegation from the phone to a point-of-sale terminal and to an apartment gate; later the user walks up to each reader with no phone and no card; the reader checks the person's face live with liveness detection, sends a challenge, the phone answers it from its secure hardware, and the reader verifies a short-lived signed proof; the biometric template never leaves the phone and the building never receives biometric data](assets/delegated-biometric-authentication.svg)

| Situation | Today | With this proposal |
|---|---|---|
| Paying at a cash register | Phone tap, card, or a face that is uploaded to a service that keeps it | Look at the reader; the phone stays in the pocket and the payment completes |
| Entering an apartment building | Key, code, card, fob, or a phone that must be present and charged | Look at the gate; no card, no key, no phone |
| The building or bank asks for biometrics | A template, a photo or a signature is handed over and stored | Nothing is handed over; only a scoped, expiring proof of a delegated check |
| The user changes their mind | The data is already with the organisation | The grant is withdrawn; the next check at that reader fails |
| The same person uses a reader every day | A visible trail that identifies them to that operator | An unlinkable token, where identity is not needed |
| A reader's vendor wants biometric integration | A custom integration with each phone maker | One standard; the operating system does the work |
| Someone tries to use a photo | Often only the vendor's own confidence threshold | Presentation-attack detection per an international standard, on the reader |
| A stolen phone | The thief may have the user's face | The phone is locked by the same biometric that would delegate; grants are visible and revocable |
| A new reader is installed | Integration with every phone ecosystem | The reader accepts the standard and joins the trust service |

## Detailed Use Cases

The full catalogue — nineteen cases, each with what it does **not** solve — is in **[USE-CASES.md](USE-CASES.md)** ([Türkçe](USE-CASES.tr.md)). The cases group into four kinds of reader:

| Group | The reader is | Cases |
|---|---|---|
| **Paying** | A till | Cashless, cardless payment at a point of sale; small-value payments and top-ups; public transport; age verification at the point of sale |
| **Getting in** | A gate | Building and flat entry; visitor and contractor access; short-term rental; hotel check-in and keyless rooms; workplace access and attendance |
| **Proving** | A desk or kiosk | Hospital and clinic identification; civic identity and voting; age and capability checks; proof of presence for insurance and warranty |
| **Working and travelling** | Anywhere | School and campus attendance; passport-less border crossing; crossing between jurisdictions that do not share a system; airline and rail boarding; shared and temporary workspaces; emergency and welfare access, and access for people who cannot use a reader |

Three patterns hold across them, and they are the reason to read the catalogue rather than skim it:

- **The privacy gain is largest where the operator's need is smallest.** Transit gates and shared workspaces need to know "authorised", not "who". Hotel and hospital reception need the opposite, and gain less.
- **The legal difficulty is roughly the inverse of the technical difficulty.** Building access is technically easy and legally contested; a voting check is technically easy and nearly impossible legally; a border check is a misfit with the model entirely.
- **The cases that would benefit most are the ones with the hardest fallbacks.** Every entry in the catalogue names a case the model does not cover. A deployment that removes the fallback removes the thing that made the fallback necessary.

Two cases are worth naming here because they are the sharpest test of the model, and they pull in opposite directions. **A passport-less border crossing** — the "seamless borders" idea, where a traveller crosses a frontier on a live check rather than on a document — is the most natural fit for the whole proposal, because there is no reason for a border authority to keep a template when the traveller's device can answer the question, and because the traveller is crossing without a phone in hand precisely when the phone-absent property matters most. It is also the case with the highest stakes and the least room for error. And **a voting check** is technically the easiest case in the set and legally close to impossible: a delegated check establishes that *a* person is authorised, not that a *specific* person is on the roll, and a scheme that blurred that distinction would be a scheme that could be abused to vote twice. The proposal's honest position on both is written up rather than glossed over.

## Consent & Onboarding: The "Require Face ID" Analogy

People already know how to ask the operating system to demand a biometric before an app opens: press and hold the icon, switch on **Require Face ID**, and the system takes care of the rest. Nobody has to trust a vendor's own biometric code, and nobody has to hand over a template.

To be precise about what Apple built, because the difference matters: since iOS 18, **Require Face ID** is a Home Screen long-press action that gates access to *that app on the user's own device* — it re-authenticates on launch and hides the app's content from Spotlight, notifications and Siri suggestions. It does not delegate a face check to any third party, and it is not per-reader. It is a device-local, per-app gate.

This proposal extends that gesture from apps to **readers**, and the operating system is the party that does the work:

- **The user grants, the operating system enforces.** The grant lives in the operating system's security subsystem, not in an app. An app can ask; it cannot grant on its own.
- **The grant names the reader.** Not "payment apps" in general, but "this payment operator at this merchant" or "the gateway of this building". A grant for one reader does not work at another.
- **The grant names the purpose.** A delegation to pay is not a delegation to enter, and neither is a delegation to prove age. A grant for one purpose cannot be used for another.
- **The grant expires.** It can be given for a day, a month, a year, or until the user revokes it. A long-lived grant that the user cannot see is the same problem as a permanent biometric handover, in a smaller costume.
- **The user can see everything.** A list of every delegation, every reader, every use and every revocation, in the same place the operating system already shows permissions. Revocation takes effect at the next check.
- **The operating system refuses the nonsense cases.** A grant cannot be taken by a malicious app, cannot be phished, and cannot be inherited by whoever picks up the phone. A stolen phone fails closed: without the owner's own biometric, no delegation can be exercised.

This is the part that makes "phone absent, still unforgeable" work. The reader only ever honours a delegation that a person personally granted, and the person can withdraw it at any time.

## Trusted Brokers Without a Central Honeypot

The model deliberately has **no central biometric database**, because the interesting asset never leaves the phone:

| Party | Holds | Does not hold |
|---|---|---|
| The user's phone | The biometric template, in secure hardware | — |
| The operating system | The grant registry, the signing key, the template | — |
| The reader (register, gate) | A public key, a short-lived proof, a liveness result | The template, a reusable image, a biometric identifier |
| The operator (bank, building, transit) | A delegation identifier, an audit log, a payment record | The template |
| The trust service | Which grants exist and whether they are still valid | The template, and no reference to any person's face |

Every party holds a **public key, a credential or a record** — never a face. The template is the one thing that cannot be rotated, and it is the one thing that never moves. A compromise of a reader, an operator or a trust service exposes keys and logs, not irreplaceable biometric data. That is the whole privacy argument.

## Operating Model

The model can run in two ways. Each device maker can run its own broker, or the makers and operators can join one shared, neutral delegation service.

| | Each operating system runs its own broker | One shared, neutral delegation service |
|---|---|---|
| A reader vendor | Implement once per operating system | Implement once |
| A building operator | Deal with every operating system separately | Deal with one |
| A new operating system | Starts from zero | Joins an existing trust anchor |
| Interoperability across borders | Low | High, which matters for travel and for cross-border operators |
| Main risk | Fragmentation, lock-in, duplicated effort | Capture by one operator |

**Recommended: one open standard and one neutral trust service, with brokers on top.** The grant format, the proof format, the verification rules and the revocation rules are the same for everyone, so a reader written once works with every phone and an operator deals with one system. The operating systems keep their own brokers, secure hardware and templates — that is where the trust and the risk already sit — and a neutral trust service issues the public trust anchors and the revocation list. Competing brokers can still compete on hardware, battery, speed and price, on top of a standard that lets them interoperate.

The trust service is non-profit or utility-like, publishes its rules and fees, and admits every broker and operator that meets published criteria. It never receives a biometric template and does not need to: it holds keys, identifiers and a revocation list.

A research-level [technical analysis](TECHNICAL-ANALYSIS.md) describes how to make this work reliably: the guarantees the system must meet, the components, the two verification modes, how revocation reaches readers, how liveness is checked, how a reader stays unlinkable, what the failure modes are and which existing standards to reuse.

A draft [technical specification](SPECIFICATION.md) turns that into normative requirements — the grant object, the proof, the two verification modes, the delegated presence check, presentation-attack detection, revocation, the privacy modes, the post-quantum profile, and a conformance test suite. Its Section 8 states the one part that is genuinely unsolved, with four candidate mechanisms and the criteria for choosing between them.

A separate [standardization note](STANDARDIZATION.md) sets out which existing standards cover which part of this, where they genuinely fall short, and the proposal for filling the gap — including the post-quantum layer that a long-lived delegated identity credential needs. Two findings from that work shape the whole proposal: the idea is not new (a granted patent family and academic papers already describe delegating a biometric check to a third-party device, so an open standard is needed to make the mechanism royalty-free and interoperable rather than proprietary), and the real gap is narrower than "biometrics are not standardised" — keeping the template on the device is *already* standardised, and what is missing is a way for a third-party reader to prove that an absent grantor authorised it.

## Similar Practices Elsewhere

Parts of this already exist, but as far as this proposal's author knows, none combines them all: a per-reader consent grant, a template that never leaves the phone, a phone-absent verification, a reader that verifies a proof rather than a template, and a revocation that reaches readers that are not online with the user.

| Practice | What it does | Closest part of this proposal |
|---|---|---|
| **FIDO2 and passkeys** | Cryptographic authentication with a device-bound key; user verification happens on the device and the relying party never sees a biometric. | The proof-based, template-free verification and the device trust anchor |
| **FIDO cross-device (hybrid) and the Proximity Exchange Protocol** | Signs in to a desktop from a nearby phone without typing a code; the phone is still the authenticator doing the check. | Signing for a device you are not holding, but with the phone still required |
| **Apple's "Require Face ID" per app** | A per-app biometric gate the operating system applies on the user's behalf, from the Home Screen long-press menu. | The consent gesture this proposal extends from apps to readers — but Apple's version is per-app and device-local, not per-reader |
| **Android lock-screen app authentication** | The system can require the device's own credential when an app is opened. | The same operating-system-as-broker idea, per app |
| **Amazon One palm and hand-vein** | Palm-based entry into Amazon's own stores, offices and clinics with no phone and no card. **Amazon discontinued it in January 2026**, citing limited customer adoption; retail palm payment ended on 3 June 2026 and the stored palm data was deleted. ([ID Tech][amz]) ([American Banker][amz2]) | Phone-absent biometric entry — built on new hardware in every store, and the biometric left the hand. Its failure is the argument for this proposal |
| **İş Bankası "face payment" with Visa, at CarrefourSA (Türkiye, June 2025)** | A Turkish bank and card scheme let a customer register a face in the banking app, then pay at a supermarket with **no card, phone, wallet, QR code or PIN** — a face scan only. ([AA][isb]) ([CNBC Borsa][isb2]) | The payment use case, already live in Türkiye — but the face is registered with the bank |
| **Toss "Face Pay" (South Korea)** | Phone-free face payment on the operator's own terminals, with 3-D face matching and liveness, tested against the presentation-attack standard; reported at around 240,000 businesses with a stated goal of one million stores by the end of 2026. ([Biometric Update][toss]) | The most successful phone-absent face payment at scale — and it is 3-D matching at the terminal, not a delegated phone identity |
| **Alipay PL1 and Tencent palm payment** | Palm scanners combining palm print and palm vein, tied to the payment account, used in shops, metro turnstiles **and building entry scanners**. ([Biometric Update][alipay]) | Pay by palm and enter by palm — with the enrolment held by the operator |
| **Lotte Duty Free "Lotte HandPay" (South Korea, September 2025)** | Palm-vein payment at Gimpo International Airport, replacing passport, boarding pass and card, with cards registered through the airport's own app. ([Biometric Update][lotte]) | The travel and airport use case, without the privacy model |
| **Touchless fingerprint and face access terminals** (Suprema, Verkada) | Biometric access readers for doors. Verkada's access station does all face processing on the reader and stores a non-reversible, encrypted template. ([Verkada][verkada]) | The reader hardware, and template-on-device — but the *reader* owns the template, not the person |
| **Apple and Google Wallet** | Device-held, per-merchant credentials and payment authorisations. | A device-held, per-reader credential, and the payment authorisation model |
| **W3C Verifiable Credentials and the EUDI Wallet** | A device-held, user-consented credential that a verifier checks without the issuer seeing the presentation; VC 2.0 became a W3C Recommendation in May 2025. | A device-held, scoped, revocable credential bound to one verifier |
| **Privacy Pass and Apple's Private Access Tokens** | Unlinkable, single-use tokens that prove eligibility without revealing who you are, deployed in production. ([RFC 9576][pp]) ([Cloudflare][cf]) | The unlinkable-token mode where identity is not needed |
| **ISO/IEC 30107 presentation attack detection** | An international standard and test methods for detecting spoofed biometric presentations. | The liveness requirement at the reader |
| **ISO/IEC 27553 and 24745 biometric information protection** | Standards requiring that biometric data not leave the device in local modes, and covering irreversibility, unlinkability, renewability and revocability. | The rules for a template that never leaves its owner |

**Why it matters now.** Two facts make the case better than any argument could.

The first: **the new-hardware model shipped, scaled, and was shut down.** Amazon One processed 36.7 million items across 17.7 million shopping sessions in its final year — real scale, real convenience — and Amazon still ended it, citing limited customer adoption. The reason is visible in the design: it required a purpose-built palm scanner in every store, and it kept the palm image on Amazon's servers. That is the model people are most wary of, and it is the model that failed.

The second: **the in-phone model shipped too, in Türkiye.** İş Bankası, Visa and CarrefourSA put face-only payment into supermarkets in June 2025, with no card and no phone. It works. But the face is registered with the bank, so the bank holds a biometric template — the same trade, just with a different holder.

The alternative is not to use biometrics at all. It is for the phone's existing, already-trusted biometric system to delegate, per reader, on the user's behalf, and to keep the template where it already lives. **What this proposal adds:** the operating system as the broker, so a reader vendor integrates once; a per-reader, per-purpose, expiring grant the user can see and revoke; a template that never leaves the phone; a reader that verifies a proof and checks for a spoof; revocation that reaches readers; unlinkable tokens where identity is not needed; and one open standard with a neutral trust service, so the model does not fragment into vendor silos.

[amz]: https://idtechwire.com/amazon-discontinues-amazon-one-palm-biometric-payment-system-due-to-limited-customer-adoption/
[amz2]: https://www.americanbanker.com/payments/news/the-future-of-palm-biometrics-after-amazons-pullback
[isb]: https://www.aa.com.tr/tr/isdunyasi/sirketler/is-bankasi-yuz-tanima-teknolojisini-odeme-yontemleri-arasina-dahil-etti/695827
[isb2]: https://www.cnbce.com/borsa/is-bankasi-yuz-tanima-teknolojisi-ile-odeme-donemini-baslatiyor-h14074
[toss]: https://www.biometricupdate.com/202601/toss-gets-lift-from-biometric-retail-payments-plans-2026-us-ipo
[alipay]: https://www.biometricupdate.com/202505/tencent-considers-bringing-palm-biometric-payment-to-thailand-alipay-pushes-the-pl1
[lotte]: https://www.biometricupdate.com/202509/lotte-duty-free-pioneers-palm-biometric-payments-in-korean-airport
[verkada]: https://www.verkada.com/blog/introducing-the-af64-access-station-pro/
[pp]: https://www.rfc-editor.org/rfc/rfc9576.pdf
[cf]: https://developers.cloudflare.com/privacy-pass/

## Implementation & Phasing

1. **Open standard:** device makers, reader vendors, operators and standards bodies agree on the grant format, the proof format, the verification rules, the revocation model and the liveness requirement. The [standardization note](STANDARDIZATION.md) proposes where each of these should be standardized.
2. **Reader pilot in one closed setting:** a building access pilot, where the operator is the user's own landlord and the risk is contained, and where the value — no keys, no cards, no template in the building's database — is easiest to see.
3. **Payment pilot:** a payment operator, with the unlinkable-token mode first, so the operator can prove it works without learning who paid.
4. **Revocation and audit at scale:** revocation lists, a public transparency report, and the audit that operators and privacy regulators can run.
5. **Liveness certification:** reader certification against the presentation-attack standard, so "it accepted a photo" is a certification failure rather than a news story.
6. **Age and attribute use cases:** the same grant mechanism carrying an age or attribute statement rather than an identity.
7. **Policy support:** alignment with data-protection law, so that a delegated check is treated as the data-minimising alternative it is, and so public deployments are not penalised for choosing it.

An initial design, to be adapted: a real implementation should adapt the trust model, the revocation bounds, the operator's role and the governance to its market and legal system.

## Stakeholders & Benefits

- **People who refuse to hand over their face:** they can use biometrics in more places than today without creating a new holder of their biometric data.
- **Phone users:** one place, already trusted, that manages every delegation, with the same gesture they already use for apps.
- **Building operators and landlords:** no keys, no cards, no lost-key call-outs, no biometric database to breach, and no badge sharing.
- **Retailers and payment operators:** fewer checkout disputes, faster throughput, and a payment flow that cannot be cloned from a photo, without the cost of a biometric database and the data-protection exposure that comes with it.
- **Transport operators:** gate throughput without a per-passenger biometric record.
- **Privacy regulators and the public:** a model where the irreplaceable asset stays with its owner, and where a delegated check is auditable and revocable.
- **Device makers:** a differentiating, privacy-positive feature that makes their phone the centre of identity, on the operating system's own terms.
- **Reader vendors:** one standard instead of per-ecosystem integrations, and a device-agnostic product.

## Risks & Mitigations

| Risk | Mitigation |
|---|---|
| A reader accepts a photo or a mask | Presentation-attack detection per an international standard, reader certification, and a published false-accept rate per model |
| The phone is stolen and the owner's face is used | The delegation is exercised only by the owner's own biometric, plus a separate device unlock; grants are visible and revocable; a stolen phone is a lost phone, not a delegated credential |
| A delegation is misused for a different purpose or reader | The grant names the reader and the purpose, and the proof is bound to both; it cannot be replayed elsewhere |
| Grants pile up and users lose track | Grants are listed in one place, expire by default, and are reviewed; an unused grant is removed rather than held open |
| The operator builds a shadow biometric database anyway | The standard requires that a proof carries no biometric data and no reusable identifier, and the audit checks it; the operator gains nothing from keeping one |
| A revoked grant still works at a reader that is offline | Revocation is bounded and published; readers cache the last known status, and the bound is stated in the standard rather than left implicit |
| Unlinkable tokens are too weak for some use cases | The grant is issued in one of two modes per purpose; the identity mode is scoped to what the reader needs and nothing more |
| The model becomes one company's monopoly | One open standard, a neutral trust service, published rules and fees, and no lock-in through a proprietary template or identifier |
| Biometric systems become a single point of failure | Delegation is one authentication option among several; a reader that also accepts a card, a code or a passkey remains usable, and a failed delegation never locks a user out of a service |
| A person's biometric is captured by a reader under the guise of a delegated check | The reader has no template to store, and the live capture is discarded after the check; the standard forbids retaining the presentation |
| Standards work is done twice elsewhere | The proposal layers on existing standards where they fit and states the gap precisely, so the new work is the missing part rather than a parallel universe |
| A stolen or replayed proof is used at the wrong reader | Short-lived proofs bound to the reader, the operation and the challenge, with replay protection at the reader |
| Someone claims the idea is new, and a granted patent covers it | The [standardization note](STANDARDIZATION.md) cites the patent family and the earlier academic work explicitly; the proposal is for a royalty-free, standardised path to the same guarantees, not for ownership of the idea |
| The grant's signature is broken in future and cached proofs validate | Post-quantum signatures on the grant, short grant lifetimes, rotation and a re-issue path — built into the standard rather than retrofitted |

**Keywords:** biometric authentication, delegated authentication, face ID, passkey, privacy, data minimisation, secure element, payment, point of sale, access control, building entry, liveness detection, presentation attack detection, verifiable credentials, unlinkability, open standard, post-quantum, trust service

## Commercial Use

Commercial implementations of this model are subject to the [commercial license](COMMERCIAL-LICENSE.md):

- **Who needs a license:** companies that operate a delegating broker (a device operating-system or identity-wallet vendor that lets a user delegate a biometric check to a named reader); access-control, point-of-sale, transit and building-operations operators that run a delegated-authentication service for their readers; and companies that sell software or services implementing it.
- **Royalty base:** the revenue these companies keep from the delegated-authentication operation, after any shares passed on to the parties operating the readers.
- **Hardware makers are exempt** for selling readers, phones or other devices that implement the published standard without operating a delegation or verification service of their own. End users are not licensees.
- **Public mandatory deployments** are royalty-free, and companies can take up the standard commercial license on their own without waiting.

## Support This Work

If you find this idea useful, you can support the author's open work through GitHub Sponsors.

<iframe src="https://github.com/sponsors/merihilgor/button" title="Sponsor merihilgor" height="32" width="114" style="border: 0; border-radius: 6px;"></iframe>

[Sponsor merihilgor on GitHub](https://github.com/sponsors/merihilgor)

## Origin

Originally proposed by Merih İlgör (October 2026); published here as an open project idea. Change history: [CHANGELOG.md](CHANGELOG.md).

## License

This work is licensed under CC BY-NC-SA 4.0 for non-commercial use; see [LICENSE](LICENSE). Commercial use requires a revenue-share agreement; see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md). Modified versions may not be sold or transferred to third parties as new ideas, and patent-like rights are reserved; see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md#derivatives-improvements-and-idea-protection).
