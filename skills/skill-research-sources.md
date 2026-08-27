---
name: skill-research-sources
description: Query recipes and source-quality heuristics for global internet research — Reddit, X/Twitter, GitHub, authority blogs, HN. English-first; language and region are query parameters, not defaults
when_to_use: Detected when researching an idea, validating a market, comparing tools, or looking for prior art on the internet
version: 1.2
---

# Research Sources Skill

Query recipes + quality scoring for internet research. Load before any WebSearch sweep.

## Core Principle

**Signal > volume.** 5 sources with real evidence beat 30 SEO listicles. Every claim needs a URL. No URL = not a finding.

---

## 0. Language & region — decide before searching

**Default: English, global.** English has the largest corpus, the strongest practitioner writing, and the deepest archives on almost every technical and business topic. Search English first, always.

Language and region are **parameters of the question**, not properties of the researcher. Do not localize because of who is asking. Localize only when the *subject* is bound to a place or a language.

### When to add a non-English pass

Add it only if one of these is true:

- The **subject itself is local** — a company, product, person, or community that operates in one country or language (a Spanish coaching startup, a German fintech, a Japanese OSS project)
- The **market is the question** — pricing, regulation, consumer behaviour, or competition in a specific country
- **Regulation or law** is involved — GDPR/AEPD, LFPDPPP, LGPD, PIPL etc. have authoritative sources only in their own language
- English search **already returned nothing** and the topic plausibly lives elsewhere
- The user **explicitly scoped it** to a region or language

If none apply, stay in English. A local-language pass on a global topic just returns worse translations of what English already covered.

### How to run a localized pass (any language)

Do not translate the English query word-for-word. Practitioners in each language use their own vocabulary.

```
<topic in target language> site:reddit.com
<topic in target language> foro OR forum OR fórum
site:<country TLD> <topic>          # .es .mx .de .jp .br .fr
<brand or person name> <native-language term for review/opinion>
```

Region-bound sources to reach for when the pass is justified:
- **Reddit country subs** — find them with `reddit subreddit <country>` rather than assuming the name
- **Local forums** — usually outrank Reddit outside the US; search `<topic> foro/forum/フォーラム/форум`
- **Local aggregators** — HN equivalents exist per market; find them, do not assume
- **Local-language YouTube and podcasts** — often the only place a niche local product is discussed

### Reporting rule

State which languages you searched, in the `## Gaps` section. "Searched EN + ES; no DE/FR pass" is a real limitation the reader needs.

---

## 1. Reddit — real user pain, unfiltered

Reddit is where people complain honestly. Best source for *problem validation* and *"does anyone actually want this"*.

### Query recipes

```
site:reddit.com <topic>
site:reddit.com <topic> "anyone else"
site:reddit.com <topic> "I built" OR "I made"
site:reddit.com <topic> "how do you" OR "what do you use"
site:reddit.com/r/<subreddit> <topic>
```

### High-signal subreddits by domain

| Domain | Subreddits |
|---|---|
| SaaS / indie | r/SaaS, r/indiehackers, r/EntrepreneurRideAlong, r/microsaas |
| Dev / stack | r/webdev, r/programming, r/ExperiencedDevs, r/laravel, r/reactjs, r/django |
| AI | r/LocalLLaMA, r/MachineLearning, r/ClaudeAI, r/OpenAI, r/singularity |
| Marketing / growth | r/marketing, r/SEO, r/bigseo, r/PPC, r/juststart |
| Automation / content | r/automation, r/NewTubers, r/socialmedia |

Starting points, not an allowlist. For any domain not listed, find the subreddit first:
```
reddit best subreddit for <domain>
site:reddit.com/r/ <domain>
```
Then search inside it. A niche subreddit with 20k members beats r/all every time.

### Reading Reddit results

- Sort by **comments**, not upvotes — argument threads hold the nuance
- A thread with 200 comments and 12 upvotes = controversial = valuable
- Ignore posts <6 months old with 0 comments (no validation)
- Quote the actual comment, not the post title

### Fetch full threads

Append `.json` to any Reddit URL for structured content:
`https://reddit.com/r/SaaS/comments/abc123.json`

---

## 2. X / Twitter — what is happening *right now*

Best for trends, launches, and builder-in-public signal. Weak for depth.

### Query recipes

```
site:x.com <topic>
site:twitter.com <topic>
<topic> "twitter thread" <year>
```

### Advanced search operators (via x.com/search)

```
<topic> min_faves:100                    # only posts with traction
<topic> min_retweets:50
from:<handle> <topic>
<topic> filter:links -filter:replies
<topic> since:2026-01-01 until:2026-08-20
```

### Reading X results

- `min_faves:100` is the noise filter. Without it, X search is garbage
- Threads (🧵) > single posts
- Check the author's bio: builder/practitioner > "AI growth guru"
- Screenshot-of-revenue posts: treat as unverified marketing

**Caveat**: X blocks unauthenticated fetching. Rely on WebSearch snippets + Nitter-style mirrors if the fetch fails. Do not burn 5 attempts on a blocked fetch — note it and move on.

---

## 3. GitHub — prior art and "is this already solved"

### Query recipes

```
site:github.com <topic>
github.com/topics/<topic-slug>
<topic> github awesome list
```

### GitHub search URLs (use WebFetch)

```
https://github.com/search?q=<topic>+stars:>500&type=repositories&s=stars
https://github.com/search?q=<topic>+stars:>100+pushed:>2025-06-01&type=repositories
https://github.com/search?q=<topic>&type=issues&s=comments
```

### Star thresholds — what they actually mean

| Stars | Read as |
|---|---|
| >10k | Category standard. Must know about it |
| 1k–10k | Serious project, real users |
| 200–1k | Niche but validated |
| 50–200 | Someone's weekend project. Read the code, don't depend on it |
| <50 | Ignore unless it's the *only* prior art |

### Health checks before recommending a repo

Report these as **values, never as adjectives**. "active" and "maintained" are not findings — a date is.

| Check | Report as | Never write |
|---|---|---|
| Last commit | actual date (`2026-07-14`) | "active", "recent", "maintained" |
| Open vs closed issues | `412 open / 19 closed` | "lots of issues" |
| Contributors | integer (`1`, `47`) | "small team" |
| License | exact SPDX (`AGPL-3.0`) | "open source" |
| Stars | integer + date checked | "popular" |

Rules of thumb once you have the numbers:
- Last commit >12 months = dead. Say it explicitly
- 400 open / 20 closed = abandoned regardless of stars
- 1 contributor = bus factor 1. Flag it before anyone depends on it
- MIT/Apache fine; AGPL or no license = flag it, it changes what you can build
- Read the **README + open issues**, not just the star count. Issues are where the real limitations are documented

If a value cannot be retrieved, write `unknown` — do not substitute an adjective.

---

## 4. Authority blogs — depth and mental models

### Tiers

**Tier 1 — practitioners with skin in the game**
- Indie/SaaS: Indie Hackers, Stripe blog, Paddle, ChartMogul, Lenny's Newsletter
- Eng: Stripe Engineering, Cloudflare, Netflix Tech, Figma, Vercel, Fly.io, Antirez
- AI: Anthropic Engineering, OpenAI Research, Simon Willison, Eugene Yan, Lilian Weng
- Growth/SEO: Ahrefs, Backlinko, Detailed.com, Growth.Design
- Systems thinking: Julia Evans, Dan Luu, Martin Fowler, Hillel Wayne

**Tier 2 — official docs and changelogs.** Always beat blogs for API/version facts.

**Tier 3 — HN discussion**
```
site:news.ycombinator.com <topic>
https://hn.algolia.com/api/v1/search?query=<topic>&tags=story&numericFilters=points>100
```
HN comments often correct the linked article. Read them.

### Blacklist — do not cite

- Medium posts with no author credentials
- "Top 10 X tools in 2026" affiliate listicles
- SEO content farms (anything that answers the query in the H1 and then pads 2000 words)
- LinkedIn thought-leadership posts with no data
- AI-generated blog spam (tell: perfect structure, zero specifics, no numbers)

---

## 5. Quality scoring — apply to every source

Score each finding before including it:

| Signal | Weight |
|---|---|
| First-hand experience ("I ran this for 18 months") | +3 |
| Concrete numbers (revenue, latency, conversion, N) | +3 |
| Named author with verifiable track record | +2 |
| Recent (<12 months) on a fast-moving topic | +2 |
| Reproducible: code, dataset, or method shown | +2 |
| Anonymous but detailed (good Reddit comment) | +1 |
| Contradicts the consensus *with evidence* | +2 |
| No numbers, no author, no date | −3 |
| Selling something in the same post | −2 |
| Pure speculation / "I think X will" | −2 |

**Include if score ≥ 3.** Below that, drop it or mark it explicitly as weak.

---

## 6. Search sequence (do it in this order)

0. **Set scope** — English-global by default (§0). Add a language pass only if the subject demands it
1. **Broad WebSearch in English** — get the vocabulary. What do people actually call this thing?
2. **Reddit** — is it a real pain? Whose?
3. **GitHub** — is it already built? How well?
4. **Authority blogs / HN** — what is the depth take? What is the failure mode?
5. **X** — what is the current temperature? Any recent launch?
6. **WebFetch the top 3–5** — snippets lie. Read the actual page before quoting it
7. **Localized pass** — only if §0 says it is warranted, using native vocabulary

Refine vocabulary after step 1 — the first query is almost always wrong.

---

## 7. Contradiction rule

If sources disagree, **do not average them**. Report the disagreement:

> Reddit r/SaaS says X doesn't convert (3 threads, 2025–2026). Stripe's blog claims 12% lift. Difference: Stripe measured on enterprise checkout, Reddit users are B2C sub-$50 ARPU. Both may be right for their segment.

Named disagreement is more useful than false consensus.

---

## 8. Anti-patterns

- ❌ Citing a source you only read the snippet of
- ❌ "Studies show" without the study
- ❌ Presenting one Reddit comment as market validation
- ❌ Star count as a proxy for quality without checking last commit
- ❌ Writing "active" / "well-maintained" instead of the actual commit date
- ❌ Stopping at 3 results because they agreed
- ❌ Burning 10 fetch attempts on a paywalled/blocked page
- ❌ Filling gaps with training data and not flagging it as such
- ❌ Defaulting to the reader's language instead of the subject's — English first unless §0 says otherwise
- ❌ Word-for-word translating an English query into another language instead of using native vocabulary
