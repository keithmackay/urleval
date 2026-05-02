# Domain Name Effectiveness: Research Summary and Scoring Framework

**Purpose:** This document synthesizes academic and practitioner research on domain name quality into a concrete, evidence-based scoring rubric. It serves as the foundation for the `urleval` skill's 8-dimension evaluation framework.

**Last Updated:** 2026-05-02

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Weights Table](#weights-table)
3. [Overall Score Formula](#overall-score-formula)
4. [Dimension 1: Memorability](#dimension-1-memorability-weight-020)
5. [Dimension 2: Spelling Reliability](#dimension-2-spelling-reliability-weight-015)
6. [Dimension 3: Pronunciation Clarity](#dimension-3-pronunciation-clarity-weight-010)
7. [Dimension 4: Associations](#dimension-4-associations-weight-015)
8. [Dimension 5: Cleverness / Brand Fit](#dimension-5-cleverness--brand-fit-weight-010)
9. [Dimension 6: Relevance to Purpose](#dimension-6-relevance-to-purpose-weight-015)
10. [Dimension 7: Competitor Overlap](#dimension-7-competitor-overlap-weight-010)
11. [Dimension 8: TLD Appropriateness](#dimension-8-tld-appropriateness-weight-005)
12. [Research Caveats](#research-caveats)
13. [Sources](#sources)

---

## Executive Summary

Domain names function simultaneously as technical identifiers, brand assets, and cognitive anchors. A large body of research from cognitive psychology, consumer behavior, trademark law, and digital marketing demonstrates that specific structural and semantic properties of a domain name measurably affect user recall, trust, conversion rates, and competitive risk. The 8 dimensions below capture the properties most consistently supported by empirical evidence.

Key findings at a glance:

- **Length is the single most studied variable.** The top 100 websites average 6.2 characters; research from Wharton suggests traffic drops ~2% per character beyond 7. The sweet spot is 6–14 characters.
- **Spelling errors are a quantified business risk.** ~14.5% of direct traffic involves typed URLs, of which ~10% contain a typo. Companies lose an estimated $327 million/year to typosquatting of their misspelled variants.
- **Pronunciation predicts word-of-mouth.** Processing fluency research shows hard-to-pronounce names reduce brand preference, recall, and sharing behavior.
- **.com commands a 70%+ trust premium.** An ICANN/Nielsen study across 5,452 consumers in 24 countries found .com and .net rated "trustworthy" at 91%; new gTLDs trail significantly.
- **Competitor overlap creates measurable confusion.** Consumers confuse imitator brands with originals up to 25% of the time; trademark research identifies domain similarity as a primary confusion driver.

---

## Weights Table

| Dimension                  | Weight |
|---------------------------|--------|
| Memorability              | 0.20   |
| Spelling Reliability      | 0.15   |
| Associations              | 0.15   |
| Relevance to Purpose      | 0.15   |
| Pronunciation Clarity     | 0.10   |
| Cleverness / Brand Fit    | 0.10   |
| Competitor Overlap        | 0.10   |
| TLD Appropriateness       | 0.05   |
| **Total**                 | **1.00** |

---

## Overall Score Formula

```
Overall Score = Σ(dimension_score × weight) × 20
```

This scales the weighted average (which ranges from 1.0 to 5.0) to a 20–100 range, making scores intuitive (similar to a school grade, percentile, or quality rating out of 100).

**Example:**
- Memorability: 4 × 0.20 = 0.80
- Spelling Reliability: 5 × 0.15 = 0.75
- Associations: 3 × 0.15 = 0.45
- Relevance to Purpose: 4 × 0.15 = 0.60
- Pronunciation Clarity: 5 × 0.10 = 0.50
- Cleverness / Brand Fit: 3 × 0.10 = 0.30
- Competitor Overlap: 4 × 0.10 = 0.40
- TLD Appropriateness: 5 × 0.05 = 0.25
- **Weighted sum = 4.05**
- **Overall score = 4.05 × 20 = 81 / 100**

---

## Dimension 1: Memorability (Weight: 0.20)

### Research Basis

Memorability is the highest-weighted dimension because it underlies virtually all downstream outcomes: direct navigation, word-of-mouth, brand recall, and conversion. It is also the most studied domain name property.

**Cognitive Load Theory** (Sweller, 1988; Miller, 1956) establishes that human working memory can hold approximately 7±2 pieces of information simultaneously. Domain names that exceed this processing capacity create cognitive strain. Research from Wharton School suggests a ~2% traffic reduction per character beyond the seventh (cited in multiple domain industry analyses referencing Wharton studies on name length and recall).

**Length findings:**
- Top 100 websites average 6.2 characters (DomainDetails KB, 2025)
- Average registered .com is 13.5 characters — well above optimal
- Under 15 characters is a widely cited maximum threshold
- 6–14 characters is the empirically supported "sweet spot"

**Syllable and word count:**
Research on business name effectiveness (Frozen Lemons, citing cognitive science literature) finds that companies with names of 1–2 syllables significantly outperform those with 3+ syllables on funding success, stock performance, and brand recall. Nielsen research on brand recall found most consumers retain only 3–5 brand names per product category.

**Structural mnemonics:**
Alliteration, rhyme, and repetition all measurably increase recall (American Marketing Association analysis). Invented portmanteau words ("Spotify," "Pinterest") can be highly memorable if they are phonetically smooth and visually distinct.

**Hyphens and numbers** interrupt visual processing and double the cognitive effort required to relay the address verbally. They are strongly associated with lower memorability.

### Scoring Rubric: Memorability (1–5)

| Score | Criteria |
|-------|----------|
| **5** | 6–10 characters; 1–2 syllables; single real word or clean portmanteau; no hyphens/numbers; rhymes, alliterates, or has clear phonetic hook; instantly sticky (e.g., `grab.com`, `stripe.com`) |
| **4** | 10–14 characters OR 3 syllables; minimal complexity; easy to reconstruct from memory; no hyphens/numbers; distinctive pattern (e.g., `shopify.com`, `dropbox.com`) |
| **3** | 14–17 characters OR compound of two common words; slightly harder to reconstruct but still sensible; no hyphens; numbers or minor complexity acceptable (e.g., `basecamp.com`) |
| **2** | 17–22 characters OR includes hyphens OR multiple words concatenated without clear boundaries; easy to confuse with similar domains; requires effort to recall correctly |
| **1** | >22 characters, multiple hyphens, or numbers interspersed; string of initials or random syllables; extreme cognitive load; virtually impossible to recall from memory alone |

---

## Dimension 2: Spelling Reliability (Weight: 0.15)

### Research Basis

Spelling reliability is distinct from memorability: a name can be memorable but hard to spell correctly. This distinction matters practically because mistyped domains route users to errors or competitors.

**Traffic loss data:**
- ~14.5% of web traffic comes from direct URL typing
- ~10% of those attempts contain a typo (Bill Hartzer / domain industry analysis)
- Companies behind the 250 most-visited U.S. websites lose an estimated $327 million/year to typosquatted domains (FairWinds Partners research)
- 938,000+ misspelled domain variations are registered against the top 3,264 websites globally

**Trust impact:**
Consumer research shows 56% of users say a typo in a domain makes them question business credibility; 55% lose trust when a domain does not match the company name (NameSilo / Typo-Proof Domains, 2025). For high-stakes industries (finance, healthcare, legal), this effect is amplified.

**Typosquatting and homophone risk:**
Palo Alto Networks Unit 42 research on cybersquatting identified four primary attack vectors: character substitution, misspelling, homophone replacement, and TLD variation. Homophones have become a growing risk as voice search expands (Amazon Alexa, Google Assistant). If a domain sounds identical to another word or brand, users navigating by voice will often land on the wrong site.

**Spelling red flags:**
- Intentional misspellings (e.g., "fiverr," "tumblr") — memorable but spell-unreliable
- Homophones (e.g., `there` vs `their`)
- Double letters that are non-standard
- Silent letters (e.g., `knight`)
- Uncommon letter clusters

### Scoring Rubric: Spelling Reliability (1–5)

| Score | Criteria |
|-------|----------|
| **5** | Every letter is phonetically expected; no ambiguous sequences; first-time listeners would spell it correctly >95% of the time; common word or clean invented word with no traps |
| **4** | One minor spelling ambiguity (e.g., one double letter, one common homophone risk); overall reliable; only 1 likely variant that could be confused |
| **3** | 2–3 potential spelling variants; includes non-standard sequences OR intentional creative misspelling; users familiar with the brand spell it correctly but new users may err |
| **2** | Multiple plausible misspellings; homophones of common words; silent letters; letter-number substitutions; brand depends on prior exposure to be spelled correctly |
| **1** | Regularly misspelled even by existing users; contains very unusual letter combinations, multiple homophones, or looks like a typo itself; significant typosquatting surface area |

---

## Dimension 3: Pronunciation Clarity (Weight: 0.10)

### Research Basis

Pronunciation clarity is closely related to spelling reliability but focuses on the inverse problem: can someone who reads the domain say it aloud correctly? This matters for verbal brand communication, podcasts, radio ads, word-of-mouth referrals, and voice search.

**Processing fluency:**
Research on brand name fluency (DelVecchio et al., 2024, *Psychology & Marketing*; ResearchGate studies on brand name recall) demonstrates that harder-to-pronounce names are less preferred, less recalled, and less likely to be shared. This effect is attributed to "processing fluency" — the brain assigns a negative valence to cognitive friction.

**Word-of-mouth impact:**
Spellbrand's brand naming guide and NameSilo's science-behind-brandable-domains article both cite studies showing that if customers cannot confidently pronounce a name, they avoid recommending it verbally — a direct suppression of organic word-of-mouth growth, which is a primary customer acquisition channel for many businesses.

**Voice search:**
As voice search share grows (approaching 50% of searches on mobile by some estimates), domains that cannot be unambiguously spoken to a voice assistant are functionally inaccessible via that channel.

**Pronunciation red flags:**
- Multiple vowel clusters with ambiguous pronunciation (e.g., "aeiou")
- Consonant clusters (e.g., "str," "spl") that are accent-sensitive
- Foreign-language words with non-English pronunciation rules
- Abbreviations that could be read as letters or as words

### Scoring Rubric: Pronunciation Clarity (1–5)

| Score | Criteria |
|-------|----------|
| **5** | Only one possible pronunciation; any English speaker reads it correctly on first encounter; works perfectly in voice search and verbal communication |
| **4** | One minor pronunciation variant possible but dominant reading is obvious; non-native speakers might hesitate but correct pronunciation is strongly implied |
| **3** | 2 plausible pronunciations; foreign or technical word; context usually resolves ambiguity; native speakers agree but some ESL confusion expected |
| **2** | Regularly mispronounced; multiple equally plausible readings; relies on brand familiarity to be said correctly; ineffective in radio/podcast contexts |
| **1** | No clear pronunciation; acronym with unknown expansion; string of consonants; foreign-language origin with radically different English pronunciation; functionally unusable in voice contexts |

---

## Dimension 4: Associations (Weight: 0.15)

### Research Basis

Associations measure what the domain name *evokes* — positive, negative, neutral, or simply absent. This is the semantic dimension of brand naming.

**Neuropsychological evidence:**
Research published in *PMC* (NIH) and the Springer Nature Research Communities shows that brand names trigger more emotional response than generic nouns. The ventromedial prefrontal cortex (emotional decision-making) and dopamine reward pathways activate during brand evaluation. Names that connect to positive semantic networks thus activate reward circuitry.

**Association set size:**
The Journal of Consumer Research (Keller & Aaker era research, archived at JSTOR) established that brand names with larger, richer association sets are more memorable and more likely to be recalled in purchase contexts. "Apple" has a vast, mostly positive association set (fruit, freshness, simplicity, innovation) that exceeds technically accurate names.

**Negative associations:**
Consumer behavior research (Contentstack, Journals of Sage Publications) consistently shows that associations with pain, danger, failure, or taboo topics suppress purchase intent even when the product itself is excellent. Some associations are culturally specific — a name benign in English may carry negative connotations in other languages (classic example: Chevy Nova in Spanish-speaking markets).

**Phonetic symbolism:**
Research published in the *Journal of Consumer Research* and summarized in brand linguistics studies (NEIU Honors Project) demonstrates that specific phonemes carry semantic weight. Hard consonants (K, T, P) suggest strength/speed; soft consonants and vowel-rich names suggest softness/approachability. This is relevant to domain naming because the phoneme profile of a domain affects perceived brand personality.

### Scoring Rubric: Associations (1–5)

| Score | Criteria |
|-------|----------|
| **5** | Strong, broad, positive association network; evokes desirable qualities (trust, speed, creativity, care, etc.) directly relevant to the brand's purpose; no negative connotations in primary markets |
| **4** | Mostly positive associations; one or two neutral associations; no notable negatives; association network is smaller but coherent |
| **3** | Neutral associations (e.g., invented word with no prior meaning); OR mixed associations where positives outweigh negatives but negatives exist; adequate for branding with marketing investment |
| **2** | Weak or irrelevant positive associations; one notable negative association in primary market; OR a name so abstract it evokes nothing (pure noise) |
| **1** | Actively negative associations; connotations of failure, danger, illegality, or embarrassment; OR carries strong negative meaning in an important target language/culture |

---

## Dimension 5: Cleverness / Brand Fit (Weight: 0.10)

### Research Basis

Cleverness refers to the degree to which the name exhibits creative intelligence — wordplay, metaphor, pun, portmanteau, or conceptual resonance — that rewards the audience for understanding it. Brand fit refers to whether that cleverness aligns with the brand's personality and industry.

**Wordplay effectiveness:**
ResearchGate studies on advertising puns ("Why Do Advertisers Use Puns? A Linguistic Perspective"; "Puns, Relevance and Appreciation in Advertisements") demonstrate that well-executed wordplay increases ad recall and positive attitude toward the brand. Marketsmiths and Mailbrother research cite online experiments where homophonic puns increased purchase intent through affiliative humor mechanisms.

**Dual-process engagement:**
Wordplay engages both the left brain (language processing) and right brain (humor/pattern recognition), creating "double engagement" that makes names stickier in memory. This is supported by cognitive neuroscience literature on humor processing.

**Cleverness vs. obscurity:**
A critical caveat from both academic and practitioner literature: cleverness must be accessible to the target audience. A clever reference only 5% of users understand provides no benefit; a pun that requires domain expertise alienates general audiences. The best clever names work on two levels simultaneously — surface level (anyone understands it) and depth level (insiders appreciate it more fully).

**Industry fit:**
Research on brand personality (Jennifer Edson Escalas and James Bettman, consumer-brand relationship literature) shows that names congruent with the brand's domain (e.g., playful names for children's products, authoritative names for financial services) create stronger brand-consumer identity alignment and purchase intent.

**Misfire risk:**
Medium and NameStormer analyses on pun-based business names caution that forced wordplay, obscure references, or humor mismatched to industry context can be perceived as unprofessional or off-brand, actively harming rather than helping brand perception.

### Scoring Rubric: Cleverness / Brand Fit (1–5)

| Score | Criteria |
|-------|----------|
| **5** | The name exhibits genuine wit — a wordplay, metaphor, or double meaning that most of the target audience will notice and appreciate; cleverness directly reinforces brand purpose; no risk of misfire |
| **4** | Clever concept or appealing linguistic texture; may not be immediately obvious to all users but rewards attention; well-suited to brand personality |
| **3** | Neutral — no particular cleverness or brand fit signal, but also no mismatch; competent, generic, or descriptive name; works fine without creative distinction |
| **2** | Forced wordplay that feels strained OR a tone mismatch (e.g., irreverent pun for a financial institution); clever to a narrow audience but confusing to the majority |
| **1** | Actively off-brand: a mismatch between name personality and brand purpose that creates cognitive dissonance; humor that is offensive, alienating, or inappropriate to the industry |

---

## Dimension 6: Relevance to Purpose (Weight: 0.15)

### Research Basis

Relevance to purpose asks: does the domain name signal — even faintly — what the site or company is about? This affects both user expectation-setting and search engine understanding.

**User expectation alignment:**
Research from GoDaddy, Yoast, and Shopify's SEO resources consistently notes that when domain names align with product or service categories, click-through rates from search results and direct navigation are higher. Users who see a domain in a SERP and can infer what the site does are more likely to click.

**SEO considerations:**
Google has confirmed (via official statements and SEO community research from Ahrefs analyzing 1 billion+ pages) that exact-match domain names do not provide direct ranking boosts. However, domain relevance has indirect effects: it influences click-through rate (a potential ranking signal) and ensures the domain attracts relevant anchor text in backlinks. The FINE agency's brand naming/SEO article notes that brand entity recognition by Google (where Google understands what a brand does) is increasingly important in semantic search.

**Brand vs. keyword domains:**
Industry consensus (Yoast, Shopify, SiteGuru, LinkedIn analysis) has shifted strongly toward brand-first domains (google.com, stripe.com, lego.com) over keyword-stuffed domains (bestcheapcarsinsurance.com). However, *moderate* relevance — where the name suggests a category without being keyword-awkward — remains a positive signal.

**Discoverability:**
For new brands without marketing budget, a relevant domain can serve as a thin SEO asset and a clarity signal in word-of-mouth contexts. "What's the website?" "It's healthcare.ai" conveys more than "It's xb72.io."

### Scoring Rubric: Relevance to Purpose (1–5)

| Score | Criteria |
|-------|----------|
| **5** | Domain name clearly signals the product/service category AND brand identity simultaneously; unambiguous even to a cold audience; strong click-through appeal (e.g., `stripe.com` for payments, `zoom.us` for video) |
| **4** | Domain name strongly suggests the industry or use case; moderate keyword relevance without being generic; category is inferable by most people |
| **3** | Domain has some relevance cues OR is a strong brand name with no direct category signal but is associated with the category through consistent marketing; acceptable |
| **2** | Name is largely abstract or unrelated to purpose; relevance relies heavily on tagline/context; difficult to understand purpose from domain alone |
| **1** | Actively misleading — domain implies a different product/service than what is offered; could damage trust when reality diverges from expectation |

---

## Dimension 7: Competitor Overlap (Weight: 0.10)

### Research Basis

Competitor overlap measures the degree to which the domain name could be confused with an existing brand, or could be mistaken for a competitor. This has both business risk and user experience dimensions.

**Consumer confusion research:**
Howard, Kerin, and Gengler (2000), "The Effects of Brand Name Similarity on Brand Source Confusion," published in *Journal of Public Policy & Marketing*, is the seminal academic study. It demonstrates that even mild phonetic or orthographic similarity between brand names generates measurable consumer confusion about product origin. DN.org's analysis of domain similarity research confirms this extends to domain names specifically.

**25% confusion rate:**
Multiple studies referenced in trademark litigation contexts find that consumers exposed to imitator brands (including domain imitators) confuse product origins 20–25% of the time when visual/phonetic similarity is high.

**Legal risk:**
The USPTO and courts apply the DuPont factors for trademark likelihood of confusion — including domain similarity, market overlap, and actual consumer confusion. A domain too similar to a registered trademark creates actionable infringement exposure under UDRP and the Lanham Act. WIPO UDRP arbitration is a common and relatively inexpensive mechanism for trademark holders to reclaim confusingly similar domains.

**Competitive traffic diversion:**
Even without malicious intent, a domain similar to a competitor's captures misdirected traffic and creates brand confusion in the market. This is particularly acute in crowded categories (SaaS, e-commerce, fintech) where dozens of similarly-named products compete.

**Search signal contamination:**
When Google indexes two very similarly named sites, search quality can degrade for both as the algorithm struggles to differentiate them — a phenomenon noted in SEO practitioner research.

### Scoring Rubric: Competitor Overlap (1–5)

| Score | Criteria |
|-------|----------|
| **5** | Domain name is entirely unique in its market; no phonetically, visually, or conceptually similar competitors exist; strong trademark distinctiveness; zero confusion risk |
| **4** | Minor surface-level similarity to at most one small competitor; clearly differentiated in context; low confusion risk; no trademark conflict likely |
| **3** | Shares generic words with multiple competitors but compound is unique; OR operates in a different market segment from similarly-named entities; moderate distinctiveness |
| **2** | Notably similar to a known competitor or existing brand; plausible consumer confusion; potential trademark complications; traffic interception risk is real |
| **1** | Near-identical to a well-known brand or trademarked domain; clear infringement risk; would likely generate UDRP complaints or Lanham Act litigation; user confusion nearly certain |

---

## Dimension 8: TLD Appropriateness (Weight: 0.05)

### Research Basis

The Top-Level Domain (TLD) is the extension after the final dot (.com, .org, .io, .ai, etc.). It receives the lowest weight because it is largely independent of the name itself and can be changed — but it is not irrelevant.

**ICANN/Nielsen global consumer study:**
An ICANN-commissioned study (Nielsen, 2016) surveyed 5,452 consumers across 24 countries. Key findings:
- .com was recognized by 95% of respondents
- .net was recognized by 88%
- .org was recognized by 83%
- Legacy TLDs (.com, .net, .org) were rated "trustworthy" by 91% of respondents
- New gTLDs (e.g., .shop, .club, .xyz) showed significantly lower trust ratings

**New TLD trust gap:**
Builder Society forum analysis of a large-scale domain trust study found new TLDs are approximately 70% less trusted than .com by general consumers. This gap narrows significantly among technical/startup audiences.

**Audience-specific TLDs:**
- `.io` has established credibility within developer/startup communities despite being a country code TLD (British Indian Ocean Territory)
- `.ai` has gained rapid acceptance in the AI/ML sector
- `.org` is appropriate for non-profits and open-source projects
- `.edu` and `.gov` are restricted and carry institutional authority
- `.co` is accepted as an abbreviation for "company" in startup contexts
- `.app`, `.dev`, `.health`, `.finance` are emerging as category-specific signals

**Email deliverability:**
NameSilo and domain industry research note that emails from non-.com domains land in spam filters more frequently, particularly with new gTLDs. This is an indirect business cost of non-.com TLD selection.

**Carleton University survey data:**
77.6% of 25–34-year-olds trust .com and country-code equivalents more than other TLDs. Trust differential widens with older demographics.

### Scoring Rubric: TLD Appropriateness (1–5)

| Score | Criteria |
|-------|----------|
| **5** | .com for a commercial/consumer product; .org for a non-profit; .edu for education; .gov for government; country-code TLD for a country-specific service; TLD is the canonical choice for the use case |
| **4** | Well-established alternative with strong sector acceptance (.io for dev tools, .ai for AI products, .co for startups); audience will recognize and trust; small trust gap vs. .com |
| **3** | Recognized TLD used in a slightly off-label context (.net for a non-network product, .org for a commercial entity); some trust erosion possible; acceptable with strong brand |
| **2** | New gTLD with limited consumer recognition (.xyz, .club, .online) OR a country-code TLD used for a global brand (e.g., .ly, .gg) with meaningful confusion risk |
| **1** | Obscure or negatively connoted TLD; TLD associated with spam domains; mismatch between TLD and entity type that actively confuses or concerns users; .tk, .ml, .cf (known spam-heavy extensions) |

---

## Research Caveats

### 1. Context Dependency
No single dimension is universally decisive. A domain that scores 2 on Associations might be perfectly appropriate for a B2B enterprise product where the name matters less than the sales relationship. Weights should be treated as defaults, not absolutes.

### 2. Industry Variation
TLD norms vary significantly by sector. `.io` is fine for a developer tool, questionable for a healthcare provider. Evaluators should apply judgment about industry context when scoring TLD Appropriateness and Associations.

### 3. Temporal Shifts
Consumer familiarity with new TLDs is growing. The 2016 ICANN/Nielsen study may understate current acceptance of extensions like `.ai` or `.app`. Trust gaps are real but narrowing in tech-savvy demographics.

### 4. Brand Building Effects
A poor domain name is often recoverable with sustained marketing investment. Google began as BackRub; Amazon was nearly Cadabra. Long-term brand building can overcome initial domain disadvantages. Scores reflect first-impression effectiveness, not absolute destiny.

### 5. Non-English Markets
Association scoring is particularly culture- and language-specific. A name with excellent English associations may carry negative connotations in Mandarin, Arabic, or Spanish. International brands should audit associations across primary market languages separately.

### 6. Practitioner vs. Academic Sources
Some findings (e.g., the Wharton 2% traffic drop per character) are widely cited in practitioner literature but cannot always be traced to a single peer-reviewed study. Where original academic citations were available they are listed below; practitioner sources are used where academic literature is sparse or behind paywalls.

### 7. Correlation vs. Causation
Many studies observe that successful companies have short, memorable domain names — but this is partly selection bias (successful companies have resources to acquire premium short domains). Causal direction should be interpreted cautiously.

---

## Sources

### Academic / Peer-Reviewed
- Howard, D.J., Kerin, R.A., & Gengler, C. (2000). "The Effects of Brand Name Similarity on Brand Source Confusion: Implications for Trademark Infringement." *Journal of Public Policy & Marketing.* [JSTOR](https://www.jstor.org/stable/30000631) | [ResearchGate](https://www.researchgate.net/publication/247837556)
- Keller, K.L. & Aaker, D.A. era research on brand association set size. *Journal of Consumer Research*. [Oxford Academic](https://academic.oup.com/jcr/article-pdf/16/2/197/5096444/16-2-197.pdf)
- DelVecchio, D. et al. (2024). "From easy to known: How fluent brand processing fosters self-brand connection." *Psychology & Marketing.* [Wiley](https://onlinelibrary.wiley.com/doi/full/10.1002/mar.21951)
- Kohli, C. et al. "Recall and Recognition of Brand Names: A Comparison of Word and Nonword Name Types." [Academia.edu](https://www.academia.edu/52750419)
- "Brand name recall: A study of the effects of word types, processing, and involvement levels." [ResearchGate](https://www.researchgate.net/publication/271928851)
- "Meaning or Sound? The Effects of Brand Name Fluency on Brand Recall and Willingness to Buy." [ResearchGate](https://www.researchgate.net/publication/280180469)
- Brand Linguistics / Sound Symbolism study. [NEIU Honors Project](https://neiudc.neiu.edu/cgi/viewcontent.cgi?article=1000&context=uhp-projects)
- "What's in and what's out in branding? A novel articulation effect for brand names." [PMC/NIH](https://pmc.ncbi.nlm.nih.gov/articles/PMC4429570/)
- Shahid Nawaz et al. (2020). "Role of Brand Love and Consumers' Demographics in Building Consumer-Brand Relationship." [SAGE Journals](https://journals.sagepub.com/doi/10.1177/2158244020983005)
- Pourazad et al. (2019). "Brand Attribute Associations, Emotional Consumer-Brand Relationship and Evaluation of Brand Extensions." [SAGE Journals](https://journals.sagepub.com/doi/10.1016/j.ausmj.2019.07.004)
- "Why Do Advertisers Use Puns? A Linguistic Perspective." [ResearchGate](https://www.researchgate.net/publication/250168757)
- "Puns, Relevance and Appreciation in Advertisements." [ResearchGate](https://www.researchgate.net/publication/245032493)
- Lim, D. "Trademark Confusion Revealed: An Empirical Analysis." *American University Law Review.* [PDF](https://aulawreview.org/wp-content/uploads/2022/05/Lim.to_.Printer.pdf)

### Industry / Practitioner Research
- ICANN / Nielsen (2016). "ICANN-Commissioned Study Finds Increased Awareness and Trust in Domain Name System." Survey of 5,452 consumers, 24 countries. [ICANN](https://www.icann.org/en/announcements/details/icann-commissioned-study-finds-increased-awareness-and-trust-in-domain-name-system-23-6-2016-en)
- GrowthBadger. "Domain Extensions: .com vs .org, .net, .io & 4 Other TLDs (Study)." [GrowthBadger](https://growthbadger.com/top-level-domains/)
- FairWinds Partners. "Are Typosquatters Hijacking Your Brand?" (citing $327M annual loss figure). [FairWinds](https://fairwindspartners.com/are-typosiders-hijacking-your-brand/)
- Palo Alto Networks Unit 42. "Cybersquatting: Attackers Mimicking Domains of Major Brands." [Unit 42](https://unit42.paloaltonetworks.com/cybersquatting/)
- Ahrefs. Analysis of 1 billion+ web pages on domain length vs. SEO ranking correlation. (Cited in multiple SEO publications including SiteGuru and Vodien.)
- DomainDetails KB. "Domain Name Length: How Short Should Your Domain Be? (2025)." [DomainDetails](https://domaindetails.com/kb/getting-started/domain-name-length-guide)
- NameSilo. "The Science Behind Domain Name Memorability." [NameSilo](https://www.namesilo.com/blog/en/domain-names/brandable-domains-name-memorability)
- NameSilo. "Typo-Proof Domains: How Structure Builds Trust in 2025." [NameSilo](https://www.namesilo.com/blog/en/domain-names/domain-structure-trust-typo-resistance-2025)
- NameSilo. "The Science Behind Brandable Domains & Their Impact on Conversion Rates." [NameSilo](https://www.namesilo.com/blog/en/domain-names/the-science-behind-brandable-domains--their-impact-on-conversion-rates)
- Dynadot. "Website Domain Name Psychology: The Science of Memorable Domain Names." [Dynadot](https://www.dynadot.com/blog/website-domain-name-psychology)
- Spaceship Blog. "The Psychology of Memorable & Valuable Domain Names." [Spaceship](https://www.spaceship.com/blog/psychology-domain-name/)
- Frozen Lemons. "How Long Should Your Business Name Be? What Research Reveals About Optimal Length." [Frozen Lemons](https://www.frozenlemons.com/blog/how-long-should-business-name-be)
- Spellbrand. "Ultimate Guide to Brand Naming: Create Memorable Names That Drive Recognition." [Spellbrand](https://spellbrand.com/blog/brand-naming-guide-memorable-names)
- InterNetX Snapshot. "Psychology of Domains: How TLDs Influence Brand Perception." [InterNetX](https://snapshot.internetx.com/en/psychology-of-domains-how-tlds-influence-brand-perception/)
- DN.org. "Domain Name Similarity and Its Influence on Brand Confusion." [DN.org](https://dn.org/domain-name-similarity-and-its-influence-on-brand-confusion/)
- DN.org. "The Influence of Misspellings on Domain Name Effectiveness and Brand Perception." [DN.org](https://dn.org/the-influence-of-misspellings-on-domain-name-effectiveness-and-brand-perception/)
- Yoast. "Domain Names and Their Impact on SEO." [Yoast](https://yoast.com/domain-names-seo/)
- FINE Agency. "Brand Naming & SEO — Why Is Brand Entity So Important?" [FINE](https://www.wearefine.com/news/brand-naming-seo-why-brand-entity-matters/)
- Springer Nature Research Communities. "The Psychology of Names: Can Brands Appeal to Our Subconscious Senses?" [Springer Nature](https://communities.springernature.com/posts/the-psychology-of-names-can-brands-appeal-to-our-subconscious-senses)
- Marketsmiths. "Worth The Pun-ishment? The Role of Wordplay in Marketing Copy." [Marketsmiths](https://www.marketsmiths.com/2020/worth-the-pun-ishment-the-role-of-wordplay-in-marketing-copy/)
- USPTO. "Likelihood of Confusion." [USPTO](https://www.uspto.gov/trademarks/search/likelihood-confusion)
- Corsearch. "Seven Factors for Identifying Trademark Likelihood of Confusion." [Corsearch](https://corsearch.com/content-library/blog/seven-factors-for-identifying-trademark-likelihood-of-confusion/)
