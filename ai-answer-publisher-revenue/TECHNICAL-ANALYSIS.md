Türkçe: [TECHNICAL-ANALYSIS.tr.md](TECHNICAL-ANALYSIS.tr.md)

# Publisher Revenue in AI Answers: Technical Analysis (Research Level)

> **Non-binding technical analysis.** The [proposal](README.md) describes the what and the why. This note shows one way to build the model so that it works reliably, and names existing standards that can be reused. It is a research-level design, not a specification. A real implementation should adapt it to its own scale, markets and laws. Commercial implementations are subject to the [commercial license](COMMERCIAL-LICENSE.md).

## 1. What must work flawlessly

The model fails if any of these fails. Every component below exists to guarantee one of them.

| # | Guarantee | Failure it prevents |
|---|---|---|
| G1 | Every use of a source's content is counted once and only once | Under-payment, double payment |
| G2 | Each source's share of an answer is computed by published, auditable rules | Disputes, gaming |
| G3 | Ads and fees never change the answer or the choice of sources | Bought answers, loss of trust |
| G4 | Every ad is unmistakably marked as an ad | Hidden advertising, legal breach |
| G5 | Conversation content never reaches the ad side | Profiling, privacy breach |
| G6 | Payments match audited counts and are settled on time | Unpaid publishers |
| G7 | Fraud (fake sources, fake traffic, fake clicks) is detected and clawed back | Money flowing to fraudsters |
| G8 | The answer is never slowed down by the ad or fee system | Bad user experience |

## 2. Components

```
 Publisher ──terms──▶ Source registry ◀──verify── Trust-label bodies
     ▲                     │
     │ payouts             ▼ terms, ad inventory IDs, trust label
 Clearing & ◀──events── AI provider ──answer──▶ User
 settlement               │  │                     ▲
     ▲                    │  └─content IDs──▶ Ad selection ──labelled ad slot──┘
     └──── audit ◀── Usage ledger (signed events)
```

### 2.1 Source registry and terms
- **Registration with proof of control.** A publisher proves control of its domain, the same way sites prove control today, and registers a legal entity for payouts.
- **Machine-readable terms on the publisher's own site.** The terms state: ads allowed (yes/no), per-retrieval price, per-display price for cached content, attribution and link requirements, and whether training use is allowed (a separate licence). Existing work to reuse: the RSL licensing vocabulary and the robots.txt convention for discovery.
- **Authorized sellers file.** The publisher lists which ad platforms may sell its inventory into AI sessions. This follows the ads.txt / sellers.json model from IAB Tech Lab, which already fights inventory fraud on the web.
- **Signed registry entries.** Every entry carries the trust label, if any, as a signed, time-limited attestation from the certification body.

### 2.2 Content identity
- Each retrieved item gets a **content ID**: its canonical URL, a hash of the retrieved text and the retrieval time. A new version of the article gets a new hash.
- Passages get **segment IDs** (for example by paragraph), so attribution can work below article level.
- The cache stores the content ID together with the content. Every cached display therefore emits an event that carries the original content ID (see 2.6).

### 2.3 Identified retrieval
- AI crawlers and agents identify themselves with **signed requests** (HTTP Message Signatures, [RFC 9421][rfc9421]). This is also the basis of the "Web Bot Auth" proposals used by pay-per-crawl services.
- The publisher's server, or its CDN, returns a **signed retrieval receipt**: content ID, provider, time and price. Both sides keep it, so there are two independent records of the same event.

### 2.4 Usage ledger
- Events: `retrieval`, `cached_display`, `citation_shown`, `citation_click`, `ad_impression`, `ad_click`.
- Each event has a unique event ID (idempotency), a content ID, a session type (free / paid / API / agent), a time and a provider signature. It never contains user identity or conversation text.
- The ledger is **append-only and tamper-evident**, for example hash-chained daily batches whose roots are published. This lets auditors prove that nothing was removed afterwards (G1, G6).
- **Reconciliation:** provider ledgers are matched daily against publisher-side receipts and CDN logs. Mismatches above a threshold open an automatic dispute.

### 2.5 Attribution (G2)
- **Unit:** the generated answer is split into claims or sentences. Each claim is linked to the source segments that support it, and the citations shown to the user come from the same links.
- **Weight:** a source's share = its supported claims ÷ all supported claims in the answer. Claims supported by several sources are split equally. Claims supported by no source (general knowledge) are excluded.
- **Methods**, from cheapest to strongest: citation-based (the sources the model cites), passage-similarity matching between the answer and retrieved segments, and leave-one-out tests on a sample (regenerate without a source and measure the change). The standard should **fix the method and its version**, so that results are reproducible in an audit.
- **Limits, stated honestly:** attribution of generated text is approximate. That is why the rules are published, sampled audits are run, and appeals exist, rather than claiming a perfect measure.
- **Anti-gaming:** duplicate or near-duplicate content (syndicated copies) is clustered and paid to the original. Keyword-stuffed pages earn nothing extra, because weight comes from supported claims, not from mentions.

### 2.6 Free sessions: ad selection and display (G3, G4, G5, G8)
- **Order of operations:** the answer is generated and fixed first. Only then is the list of content IDs it used passed to ad selection. The ad side never sees the conversation, and the answer side never sees the ads.
- **Ads never enter the model's context.** The ad creative is rendered by the client in a separate slot. This prevents the ad from influencing the answer and stops prompt injection hidden in ad text.
- **Selection:** candidates are the ads that the cited sources' authorized sellers offer for that content, filtered by advertiser opt-in, brand-safety category (IAB Tech Lab content taxonomy) and sensitive-topic blocks. The ad is chosen by an auction among eligible ads, with the bid request containing only content categories and coarse context. The OpenRTB protocol can be extended for this.
- **Latency:** ad selection runs in parallel with answer streaming and has a hard time limit. If it is late, no ad is shown. The answer is never delayed (G8).
- **Frequency caps:** per answer (for example one ad block), per session and per day, enforced by the client and checked in the ledger.
- **Revenue split:** the auction price minus the platform fee is split between the AI provider and the cited sources by their attribution weights. The split percentages are part of the standard and are published.

### 2.7 Ad labelling (G4)
Every ad shown in an AI session must:
- carry a fixed, visible label ("Ad" / "Sponsored", "Reklam" in Turkish) in the user's language;
- show the advertiser's name and, where required, who paid for it;
- be visually separated from the answer (own frame, different background) and never appear inside the answer text or citations;
- in voice mode, be announced as an advertisement before it is spoken, in a different voice or after a clear audio cue;
- in agent mode, never be turned into an action (an agent does not buy, book or click because of an ad);
- offer "why am I seeing this ad?" (the content it was matched to).

This follows existing disclosure law: the EU Digital Services Act requires platforms to show, for each ad, that it is an ad, on whose behalf it is shown, who paid and the main targeting parameters (Regulation (EU) 2022/2065, Art. 26, [EUR-Lex][dsa]). Turkish consumer law prohibits covert advertising (Law No. 6502, Art. 61(4), [Mevzuat][tr6502]). The US FTC treats ads formatted to look like ordinary content as deceptive ([FTC][ftc]).

### 2.8 Paid, API and agent sessions: per-use fees
- **Per retrieval:** the price in the publisher's terms, paid when the content is fetched live.
- **Per cached display:** when the answer uses cached content, a per-display price applies instead. The cache cannot be used to avoid payment, because every display emits a `cached_display` event with the original content ID.
- **Price protection:** publishers set their own prices. The standard can define a default price and a monthly cap per provider and article, so that a single viral article does not become unaffordable and small AI providers stay viable.
- **Pre-check:** before retrieval, the provider checks the terms from the registry (cached for a short time). If the price is above its own limit, it does not use the content and does not cite it.

### 2.9 Measurement and fraud (G7)
- **Impression definition:** an ad counts only if it was rendered and visible on screen, following industry viewability standards (Media Rating Council guidelines). A click counts only if it is a valid, human, de-duplicated click.
- **Fraud controls:** verified publisher identity and payout accounts; authorized sellers lists; signed provider events; anomaly detection (sudden spikes, impossible ratios, data-center traffic); invalid-traffic filtering; holdback of part of new publishers' earnings for a review period; claw-back of fraudulent payouts.
- **Independent audit:** an accredited auditor checks the ledgers, attribution samples, neutrality tests and settlements, and publishes a summary each year.

### 2.10 Neutrality testing (G3)
- **Separation by design:** source selection and answer generation have no access to ad or price data. Paid and free sessions use the same source-selection rules.
- **Replay audit:** the auditor reruns a sample of queries with the ad and fee system switched off and compares sources and answers. Systematic differences are a breach.
- **Price-blindness check:** sources with high per-use prices must not be cited less often than their relevance justifies, unless the provider declares that it does not use them at all. Cheaper must not mean "more quoted".

### 2.11 Clearing and settlement (G6)
- Monthly netting per publisher across all providers: ad shares + per-use fees − claw-backs.
- Statements with event counts per content ID and session type, downloadable by the publisher.
- Minimum payout threshold with carry-over, so that small publishers are paid when they reach it rather than losing small amounts.
- Disputes: automatic for reconciliation mismatches, manual for attribution, with an appeal to an independent panel.

### 2.12 Privacy (G5)
- Ad requests contain only content IDs, content categories, language, country and session type. They never contain conversation text, user ID or a profile.
- Reports are aggregated. Small counts are suppressed or noised so that individual users cannot be inferred.
- Data protection law applies: GDPR in the EU, KVKK in Turkey. Advertising profiling based on sensitive data is excluded altogether.

### 2.13 Trust labels
- Certification bodies issue signed, time-limited attestations, recorded in the registry. Existing process standards such as the Journalism Trust Initiative (CWA 17493) can be the basis.
- The label is shown next to the citation, with a link to the certification and its expiry date.
- Withdrawal and expiry propagate to providers within a fixed time.

## 3. Operating model: provider ad networks or one shared platform

Two ways to run the same standard:

| | **A. Each AI provider runs its own ad network** ("Gemini Ads", "ChatGPT Ads", "Claude Ads"…) | **B. One shared, neutral platform that AI providers and publishers join** |
|---|---|---|
| Advertisers | Must buy separately on each network | Buy once, reach every member provider |
| Publishers | Register and reconcile with each provider | Register once, one statement, one payout |
| Small AI providers | Hard to build their own ad sales | Join and earn from day one |
| Competition | Strong providers dominate; risk of self-preferencing | Neutral; providers compete on answer quality |
| Neutrality and audit | One auditor per network, results hard to compare | One audit framework and one ledger standard |
| Speed of launch | Fast for large providers, which already run ad systems | Slower; needs governance and members |
| Risk | Fragmentation; small publishers left out | A single point of failure or capture; antitrust scrutiny |

**Recommendation: a shared standard with a neutral clearing layer, open to both.**
- **One open standard** covers terms, content IDs, the event schema, attribution rules, labelling and audit. It is mandatory in both models.
- **One neutral clearing and registry**, governed by publishers, AI providers, advertisers and civil society, is shared by all. It holds the publisher registry, reconciles ledgers and pays publishers.
- **Ad sales can be competitive.** A provider may run its own ad network (model A), or join a shared exchange (model B), or both. In every case it reports into the same ledger standard and pays through the same clearing. Publishers and advertisers therefore deal with one system, while providers keep room to compete.
- **Safeguards against capture:** the clearing body is non-profit or utility-like, has an independent board, publishes its fees, and must admit any provider or publisher that meets the published criteria.

## 4. Failure modes

| Failure | Detection | Response |
|---|---|---|
| Provider under-reports uses | Daily reconciliation with publisher receipts and CDN logs | Automatic dispute, back-payment with interest, audit escalation |
| Cache used to avoid fees | `cached_display` events missing while citations are shown | Breach of the standard, back-payment |
| Double counting | Duplicate event IDs | Idempotent ledger drops duplicates |
| Ad shown inside the answer or unlabelled | Client conformance tests and audit screenshots | Ad revenue withheld, public notice |
| Ad text tries to steer the model | Ads never enter the model context | Not possible by design |
| Content farm games attribution | Duplicate clustering, claim-based weights | No extra earnings; repeated abuse removes registration |
| Click fraud | Invalid-traffic filters, anomaly detection | Clicks voided, earnings clawed back |
| Trust label withdrawn | Registry update | Label removed from answers within the fixed time |
| Ad or fee system down | Health checks | Answer served without ads; fees recorded and settled later |

## 5. Phased build

1. **Standard draft:** terms vocabulary, content ID, event schema, labelling rules, attribution rules v1 (citation-based).
2. **Pilot:** news publishers and two or three AI providers in one or two countries; per-use fees and labelled ads; daily reconciliation; one independent auditor.
3. **Attribution v2:** passage-level matching and sampled leave-one-out audits.
4. **Shared clearing:** the neutral registry and clearing open to all providers and publishers.
5. **Trust labels and wider content:** certification attestations; research and reference publishers.

## 6. Existing standards and work to reuse

| Need | Existing work |
|---|---|
| Machine-readable licensing terms | RSL (Really Simple Licensing); robots.txt conventions |
| Authorized sellers, anti-fraud | IAB Tech Lab ads.txt and sellers.json |
| Ad auction and bid requests | IAB Tech Lab OpenRTB |
| Content and brand-safety categories | IAB Tech Lab content taxonomy |
| Crawler identity | HTTP Message Signatures (RFC 9421), Web Bot Auth proposals |
| Viewability and measurement | Media Rating Council guidelines |
| Ad disclosure | EU DSA Art. 26; Turkish Law No. 6502 Art. 61; FTC policy on deceptively formatted ads |
| Trust certification | Journalism Trust Initiative (CWA 17493) |
| Privacy | GDPR, KVKK |

[rfc9421]: https://www.rfc-editor.org/rfc/rfc9421
[dsa]: https://eur-lex.europa.eu/eli/reg/2022/2065/oj
[tr6502]: https://www.mevzuat.gov.tr/mevzuat?MevzuatNo=6502&MevzuatTur=1&MevzuatTertip=5
[ftc]: https://www.ftc.gov/legal-library/browse/commission-enforcement-policy-statement-deceptively-formatted-advertisements
