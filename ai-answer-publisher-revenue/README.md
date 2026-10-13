Türkçe: [README.tr.md](README.tr.md)

# Publisher Revenue in AI Answers: Source Ads in Free Sessions, Per-Use Fees in Paid Ones

## Summary

People increasingly get news and research from AI assistants instead of visiting the websites that produced them. The assistant reads the page, but the visit, the ad impression and the subscription offer never happen. The ad-funded model that pays reporters, editors and researchers stops working. This proposal moves that revenue into the AI session:

- **The source's ads go with its content.** When an assistant uses a page to answer a free-plan user, it can show, in a reasonable number, the ads that belong to that source. The revenue is shared with the source, with impression and click reports as in today's search ad networks.
- **Paid plans stay ad-free and still pay.** In paid or ad-free sessions, the AI provider pays the source a small fee each time it retrieves the content, or each time it shows content it has cached.
- **Every use counts, including cached use.** One article read once and shown in a thousand sessions earns per display or per click, not once per crawl.
- **One open standard, not private deals.** The same terms, measurement and clearing work for every AI provider and every publisher, large or small.
- **Verified sources come first.** Sources certified by an independent, multi-stakeholder process take part in the revenue programme and carry a visible trust label, which raises answer quality without the state deciding what is true.

**In short: when AI reads the source, the source gets paid, through its own ads in free sessions and a small fee in paid ones.**

## Problem

- **Clicks are disappearing.** When an AI summary answers the question, people rarely click through to the source. Ad impressions, subscriptions and loyal readers go with the click.
- **The work still costs the same.** Reporting, fact-checking, research and editing take time and money. If content is used but not paid for, fewer people can afford to produce it, and local and investigative journalism go first.
- **The value moves to the AI layer.** AI providers earn from free-plan ads, subscriptions and API fees built partly on content they did not pay for.
- **Current fixes are fragmented.** Some providers sign one-off deals with large publishers, others offer their own revenue-sharing programmes, and some sites simply block AI. Small publishers are usually left out, and blocking makes answers worse for everyone.
- **Blocking hurts quality.** If trustworthy sources block AI and unreliable ones do not, AI answers drift towards the content that is free to take, not the content that is right.
- **Readers cannot tell where an answer comes from.** Answers rarely show how reliable each source is.

## Proposed Solution

1. **Registered sources with their own terms.** A publisher registers once and states, in machine-readable form, how AI may use its content: with its ads, against a per-use fee, both, or not at all. Training use stays a separate licence and is not part of this flow.
2. **Ads from the source, in free sessions.** When an assistant answers a free-plan user with content from a registered source, it may show ads from that source's own ad inventory, matched to the content, not to the user. The number is capped, for example one ad block per answer, and ads are clearly labelled and kept outside the answer text.
3. **Advertiser consent.** An advertiser who bought space on a publisher's site chooses whether its ads may also appear in AI sessions that use that publisher. Brand-safety rules from the original placement still apply.
4. **Per-use fees in paid sessions.** In paid, ad-free sessions, and in API or agent use, the AI provider pays the source a small fee per retrieval, or per display when it serves cached content. The user sees no ads.
5. **Fair attribution.** An answer often draws on several sources. Each source's share of the revenue or fee follows its share of the answer, and every source used is cited with a link, so the reader can still visit.
6. **Reporting and audit.** Sources get impression, click and use reports. An independent auditor checks the counts, and both sides publish aggregate transparency figures.
7. **Clearing for everyone.** A neutral clearing body collects from AI providers and pays sources, with a low entry threshold so that small and independent publishers can join.
8. **Trusted-source labels.** Sources certified by an independent, multi-stakeholder process that checks how journalism is produced (editorial independence, corrections, ownership transparency) carry a trust label in answers and take part in the revenue programme. Uncertified sources are not banned. They are only unlabelled and outside the programme.
9. **No influence on answers.** Revenue, ads and fees must never change what the answer says or which sources are chosen. This is written into the standard and checked in the audit.
10. **Privacy by design.** Ads are contextual. Conversations are not used to profile users for advertising, and ad reports contain no personal data.

## How It Works

![Publisher revenue in AI answers: a user asks an AI assistant; the assistant uses content from registered sources and cites them; in a free session the source's own ads are shown in a labelled block next to the answer and the ad revenue is shared with the source; in a paid session no ads are shown and the AI provider pays the source a small fee per retrieval or per cached display; a neutral clearing body counts uses, audits them and pays each source by its share of the answer](assets/ai-answer-publisher-revenue.svg)

| Situation | Today | With this proposal |
|---|---|---|
| A free-plan user reads a news summary | The source earns nothing; any ads belong to the AI provider | The source's own ads appear next to the answer; the ad revenue is shared |
| A paid-plan user gets the same summary | The source earns nothing | The AI provider pays a small per-use fee; the user sees no ads |
| An article is cached and shown in many sessions | Paid at most once, if at all | Paid per display or click, like an ad impression |
| An answer combines several sources | Unclear who contributed what | Each source is paid by its share of the answer and cited with a link |
| A small local publisher | No deal, no leverage | Joins the same standard terms as large publishers |
| A reader wants to know if a source is reliable | No signal in the answer | Certified sources carry a trust label |
| A publisher's options | Allow for free, or block and disappear from answers | Allow with ads, with fees, both, or not at all |

## Revenue by Session Type

| Session | What the user sees | What the source receives |
|---|---|---|
| **Free plan** | The answer, links to sources and a capped, labelled ad block from the sources used | A share of the ad revenue, weighted by its share of the answer |
| **Paid plan (ad-free)** | The answer and links to sources, no ads | A small fee per retrieval, or per display of cached content |
| **API and agents** (an AI acting for a user or a business) | Whatever the operator builds | The same per-use fee, charged to the operator |
| **Publisher with a paywall or no ads** | The answer within the publisher's licence terms | The per-use fee it set; it may require a link to subscribe |
| **Source not registered** | The AI may still cite it under general law | Nothing from the programme; no trust label |

**Cached content.** A provider may store content to answer faster. The meter then runs per display or click, not per crawl, so caching does not reduce what the source earns.

**Reasonable ads.** A cap on ads per answer and per session keeps free plans usable. Ads never appear inside the answer text, and sensitive topics such as health, politics and content for minors carry no ads.

## Trusted Sources Without Censorship

A list of "approved" sources run by a government would threaten press freedom. This proposal uses certification instead:

- **Independent and multi-stakeholder.** Certification is done by independent bodies under a public standard, with journalists, publishers, academics and civil society at the table, not by the state.
- **Process, not opinions.** It checks how content is produced (editorial independence, corrections policy, ownership and funding transparency, clear separation of news and advertising), not what it says.
- **Labels and tiers, not bans.** Certified sources carry a label and take part in the revenue programme. Others can still be read and cited.
- **Published criteria and appeal.** The criteria and the list of certified sources are public, and refusals can be appealed.
- **Regular review.** Certification is renewed periodically and can be withdrawn after repeated, uncorrected falsehoods.

## Similar Practices Elsewhere

Parts of this model already exist. As far as this proposal's author knows, none combines them all: source-specific ads in free sessions, per-use fees in paid sessions, per-display payment for cached content, one open standard with neutral clearing, and independent trust labels.

| Practice | What it does | Closest part of this proposal |
|---|---|---|
| **Perplexity Publishers' Program (2024)** | Shares ad revenue with publishers cited in an answer next to a sponsored follow-up question; launch partners included Time, Der Spiegel and Fortune. ([TechCrunch][pplx-24]) | Ad revenue shared with the cited source |
| **Perplexity Comet Plus (2025)** | A $5 monthly subscription; 80% of its revenue goes to publishers from a starting pool of $42.5 million, split by traffic, citations and agent use. ([Axios][pplx-25], [Subscription Insider][pplx-25b]) | Paid sessions paying sources by use |
| **ProRata / Gist (2025)** | An answer engine that shares 50% of its ad revenue with publishers, split by each source's contribution to the answer; agreement with almost all Danish publishers. ([A Media Operator][pr-dk], [Press Gazette][pr-pg]) | Revenue split by share of the answer |
| **Cloudflare Pay Per Crawl (2025)** | Lets sites charge AI crawlers a price per request, or block them; AI crawlers are blocked by default on new sites. ([The Register][cf]) | Per-retrieval fees set by the source |
| **RSL, Really Simple Licensing (2025)** | An open standard for machine-readable licensing terms, including pay-per-crawl and pay-per-inference, with a collective rights organization modelled on music royalties. ([RSL][rsl], [Search Engine Land][rsl-sel]) | Machine-readable source terms and collective clearing |
| **Microsoft Publisher Content Marketplace (2025)** | A planned marketplace paying publishers by how often and how usefully their content is used in AI answers, starting with Copilot. ([Axios][ms]) | Use-based payment from an AI provider |
| **Ads in ChatGPT (2026)** | OpenAI tests ads in its free and Go plans in clearly labelled slots separate from answers, says ads will not influence answers, and keeps paid plans ad-free. ([BTW Media][oai]) | Labelled ads separate from the answer; ad-free paid plans |
| **EU press publishers' right (2019)** | Gives press publishers rights over the online use of their publications by information-society service providers (Directive (EU) 2019/790, Art. 15). ([EUR-Lex][eu]) | Legal basis for paying sources |
| **Australia, News Media Bargaining Code (2021)** | Led to almost AU$250 million a year in deals; after Meta did not renew, a "news bargaining incentive" charge with an offset for deals was announced in December 2024. ([Australian Treasury][au]) | Pressure on platforms to pay for news |
| **Canada, Online News Act (2023)** | Google paid C$100 million to a journalism collective for an exemption; Meta blocked news instead. ([CBC][ca]) | Collective payment and its limits: blocking is the risk |
| **Journalism Trust Initiative (2019)** | A certification standard by Reporters Without Borders, published as CEN Workshop Agreement CWA 17493, that checks how journalism is produced, not what it says. ([JTI][jti]) | Independent trust labels |

**Why it matters now.** In a Pew Research Center study of US Google searches, users clicked a traditional result 8% of the time when an AI summary appeared, against 15% without one, and clicked a link inside the summary only 1% of the time. ([VideoWeek][pew])

**What this proposal adds:** the source's own ads travel with its content; paid sessions pay per use instead of from a discretionary pool; cached content is paid per display; one open standard and neutral clearing replace private deals; independent trust labels improve answer quality; and strict separation of ads and answers.

[pplx-24]: https://techcrunch.com/2024/07/30/perplexitys-plan-to-share-ad-revenue-with-outlets-cited-by-its-ai-chatbot
[pplx-25]: https://www.axios.com/2025/08/26/perplexity-comet-plus-subscription
[pplx-25b]: https://www.subscriptioninsider.com/article-type/news/perplexity-unveils-42-5m-revenue-sharing-model-as-comet-browser-takes-aim-at-google
[pr-dk]: https://www.amediaoperator.com/news/prorata-denmark-licensing/
[pr-pg]: https://pressgazette.co.uk/publishers/digital-journalism/prorata-publishers-ai-start-up-news-widget-answers/
[cf]: https://www.theregister.com/2025/07/01/cloudflare_creates_ai_crawler_toll/
[rsl]: https://rslstandard.org/
[rsl-sel]: https://searchengineland.com/really-simple-licensing-461834
[ms]: https://axios.com/2025/09/23/microsoft-ai-marketplace-publishers
[oai]: https://btw.media/en/openai-begins-testing-ads-in-chatgpt-free-and-go-tiers
[eu]: https://eur-lex.europa.eu/eli/dir/2019/790/oj
[au]: https://ministers.treasury.gov.au/ministers/stephen-jones-2022/media-releases/albanese-government-establish-news-bargaining-incentive
[ca]: https://www.cbc.ca/1.7422690
[jti]: https://journalismtrustinitiative.org/about
[pew]: https://videoweek.com/2025/07/23/pew-research-demonstrates-sharp-impact-of-ai-overviews-on-publisher-search-traffic/

## Implementation & Phasing

1. **Open standard:** publishers, AI providers, advertisers and press-freedom groups agree on the machine-readable terms, the counting rules and the separation of ads and answers.
2. **Pilot with news:** start with news and current affairs, where the loss of clicks is largest, in one or two countries, with a few AI providers and a broad mix of large and small publishers.
3. **Paid-session fees:** add per-use fees for paid plans, API and agent use once counting has been audited in the pilot.
4. **Trust labels:** connect independent certification so that certified sources carry a label in answers.
5. **More content types:** extend to research, reference, educational and specialist publishers.
6. **Policy support:** where needed, align the model with press publishers' rights and bargaining laws so that taking part counts towards a provider's obligations.

An initial design, to be adapted: a real implementation should adapt the rates, caps, attribution rules and governance to its market and legal system.

## Stakeholders & Benefits

- **Journalists, researchers and publishers:** income from AI use of their work, including from paid plans and cached content.
- **Small and local publishers:** the same terms as large ones, without having to negotiate individual deals.
- **AI providers:** legal certainty, access to quality content instead of blocks, and a clear way to meet press publishers' rights.
- **Advertisers:** contextual placements next to the content they chose to support, with their brand-safety rules.
- **Users:** better-sourced answers, visible trust labels, links to the original, and a choice between a free plan with capped ads and an ad-free paid plan.
- **Society:** a sustainable funding model for journalism and research, and less room for misinformation in AI answers.

## Risks & Mitigations

| Risk | Mitigation |
|---|---|
| Ads influence what the answer says | Ads outside the answer text, neutrality written into the standard, independent audit of source selection |
| Too many ads ruin the free plan | Caps per answer and per session; no ads next to sensitive topics |
| Users profiled from their conversations | Contextual ads only; no personal data in reports; compliance with data protection law |
| Fake publishers, fake retrievals or click fraud | Verified registration, independent audit, anomaly checks and claw-back of fraudulent payouts |
| Attribution is disputed | Published attribution rules, audit, and an appeal process |
| Trust labels used for censorship or favour incumbents | Independent, multi-stakeholder certification of process, not content; public criteria; appeals; no ban for uncertified sources |
| AI providers refuse to join | Policy support: participation counts towards press publishers' rights and bargaining obligations |
| Publishers block AI anyway | Flexible terms (ads, fees, both), so allowing AI pays more than blocking |
| Fees make small AI providers uncompetitive | Low per-use rates, volume tiers and free use for research and non-commercial projects |
| Payments go only to large publishers | Low entry threshold and payment by use, not by size |

**Keywords:** AI search, generative AI, publisher revenue, journalism funding, advertising, contextual advertising, revenue sharing, pay per use, pay per crawl, AI attribution, content licensing, press publishers' right, trusted sources, media literacy, misinformation, news sustainability

## Origin

Originally proposed by Merih İlgör (October 2026); published here as an open project idea. Change history: [CHANGELOG.md](CHANGELOG.md).

## License

This work is licensed under CC BY-NC-SA 4.0 for non-commercial use; see [LICENSE](LICENSE). Commercial use requires a revenue-share agreement; see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md). Modified versions may not be sold or transferred to third parties as new ideas, and patent-like rights are reserved; see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md#derivatives-improvements-and-idea-protection).
