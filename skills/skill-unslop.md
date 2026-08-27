---
name: unslop
description: Strip AI tells from prose that gets persisted or published. Applies to README, docs, ADRs, PR bodies, commit bodies, LinkedIn drafts, oele.dev Ship Log entries, landing copy, client proposals, and emails. Does NOT apply to terminal chat replies or agent reports, which caveman owns.
when_to_use: Triggered when writing, drafting, editing, or reviewing text that will end up in a file, a git object, or a channel outside the terminal. Also run as a final pass after copywriting generates marketing copy.
---

# Unslop

Edit prose to remove AI patterns and restore human voice.

## Boundary with caveman

These two never run on the same text.

| Surface | Owner |
|---|---|
| Terminal chat reply | caveman |
| Subagent report back to main thread | caveman |
| File content (README, docs, ADR, code comments) | unslop |
| Commit body, PR description | unslop |
| LinkedIn post, Ship Log entry, landing copy, email | unslop |

Caveman already declares the handoff: "Code/commits/PRs: write normal." Normal means unslop.

Never apply "adding soul" to a chat reply. Fragments and dropped articles are correct there and wrong in a README.

Code blocks, error strings, CLI output, and config are never edited by either skill.

## Process

1. Scan for the patterns below.
2. Rewrite. Preserve meaning, match intended tone.
3. Add soul (next section).
4. Self-audit: "What makes this obviously AI generated?" Fix remaining tells.
5. Last check: does the author's own judgment appear anywhere? If the piece could have been written by someone with no context on the project, it fails no matter how clean the prose is.

## The rule behind the rules

Polish without perspective is worse than no polish. Anyone can generate competent prose in nine seconds, so competent prose has no value on its own. What carries value is the read on the problem, the tradeoff you'd make, the thing you noticed that nobody else did. Edit toward that. Never edit it out.

If the draft has no point of view to protect, stop editing and say so. The problem is the thinking, not the wording.

## Adding soul

Removing patterns is half the job. Sterile, voiceless writing is just as obvious.

- **Have opinions.** React to facts instead of neutrally listing pros and cons.
- **Vary rhythm.** Short sentences. Then longer ones that take their time. Mix it up.
- **Acknowledge complexity.** "Impressive but also kind of unsettling" beats "impressive."
- **Use "I" when it fits.** First person is not unprofessional.
- **Let some mess in.** Perfect structure looks machine-made.
- **Be specific.** Not "this is concerning" but "there's something unsettling about agents churning away at 3am."

## Patterns to detect and fix

### Content

1. **Puffery.** "pivotal moment", "testament to", "evolving landscape", "setting the stage for", "indelible mark", "deeply rooted". Cut it, state what happened.
2. **Name-dropping.** Listing tools or outlets without context. Pick one, say what it did.
3. **Superficial -ing phrases.** "highlighting...", "ensuring...", "reflecting...", "showcasing...", "fostering...". Delete or expand with a real fact.
4. **Promotional language.** "vibrant", "breathtaking", "groundbreaking", "renowned", "stunning", "must-have". Use neutral description.
5. **Vague attributions.** "Experts believe", "Industry reports suggest", "Some argue". Name the source or delete.
6. **Formulaic challenges.** "Despite challenges... continues to thrive." Replace with specific facts.

### Shipping and pitch tells

Applies to Ship Log entries, LinkedIn posts, landing copy, and client proposals.

6a. **Transformation verbs.** "revolutionize your workflow", "transform your operational efficiency", "a transformative opportunity". Say what the thing does. "Cuts deploy from 12 minutes to 40 seconds" beats any of them.

6b. **Capability adjectives.** cutting-edge, best-in-class, world-class, robust, comprehensive, sophisticated, seamless, seamlessly, future-proof, scalable (when unmeasured), state-of-the-art, blazing fast. Delete the word and check whether the sentence still says anything. Usually it does, and it reads better.

6c. **Empowerment language.** "empowering teams to", "enabling users to", "unlock actionable insights", "drive measurable outcomes". Name the person and the task. "The client pulls the report themselves instead of emailing me."

6d. **Unsourced numbers.** "40 to 60% reduction in manual effort", "3x faster", "95%+ accuracy". Either show where the number came from or cut it. On a landing page an invented range is a claim you have to defend. In a proposal it becomes a commitment.

6e. **Boilerplate openers.** "In today's fast-moving AI landscape, teams must harness the power of automation." Any sentence that could open a post about any project in any industry. Open with something true about this one instead.

6f. **Filler section headers.** "Why work with me", "Conclusion", an "Overview" that summarizes nothing. Keep the section only if it holds something the reader cannot get elsewhere in the document.

### Language

7. **AI vocabulary.** Additionally, crucial, delve, enduring, enhance, fostering, garner, interplay, intricate, landscape (abstract), pivotal, showcase, tapestry (abstract), testament, underscore, vibrant. Replace with plain words.
8. **Fancy ways to say "is".** "serves as", "stands as", "boasts", "features". Just say "is" or "has".
9. **"Not just X, but Y."** State the point directly.
10. **Rule of three.** Forcing ideas into groups of three. Use the natural number.
11. **Synonym cycling.** Picking a new word for the same thing every paragraph. Pick one, repeat it.
12. **False ranges.** "from X to Y" where X and Y are not on a meaningful scale. List the items directly.

### Style

13. **Em dash overuse.** Avoid em dashes entirely in published prose. Use periods or commas only. No parentheses as a substitute, no en dashes, no hyphen-as-dash. Reaching for parentheses instead trades one tell for another. If a thought needs separation, end the sentence.
14. **Colon overuse.** Colons are fine before a list or example. Not as mid-sentence connectors. Rewrite so the point stands without comparison framing.
15. **Boldface overuse.** Do not bold every proper noun or acronym.
16. **Inline-header lists.** The tell is a bold label and colon that restates the line: "**Performance:** Performance improved...". Convert to prose. A bold lead-in ending in a period, naming the item, followed by genuinely new detail ("**Schema in TypeScript.** Tables live in one file.") is fine, not a tell.
17. **Title case headings.** Use sentence case.
18. **Decorative emojis.** Remove from docs, proposals, and landing copy. In a LinkedIn post or a Slack message a few are fine and stripping them all reads stiff. Test: is the emoji doing a joke, or filling a slot?
19. **Curly quotes.** Replace with straight quotes.

### Communication artifacts

20. **Chatbot phrases.** "I hope this helps!", "Let me know if...", "Of course!", "Certainly!", "Found the smoking gun!" Remove.
21. **Cutoff disclaimers.** "While specific details are limited..." Find the source or remove.
22. **Sycophantic tone.** "Great question! You're absolutely right!" Respond directly.

### Filler

23. **Filler phrases.** "In order to" becomes "To". "Due to the fact that" becomes "Because". "It is important to note that" gets deleted.
24. **Excessive hedging.** "could potentially possibly be argued that it might" becomes "may".
25. **Generic conclusions.** "The future looks bright." State a specific plan or fact.

### Jargon

26. **Abstract metaphor nouns.** Substrate, wedge, vector, locus, vantage, nexus, primitive (as noun), harness (as metaphor), surface (as in "API surface"), bedrock, scaffolding (as metaphor), modality, paradigm, gold-plating, ratchet (as metaphor), evacuate (for moving code), endgame, north star, flywheel. These read as technical but have a plainer concrete word. "Substrate" becomes "base". "Wedge in" becomes "add". "Vector" becomes "way". "Gold-plating" becomes "more than the job needs". "Evacuate" becomes "move out". "Endgame" becomes "the last phase". Pick the concrete word.

### Plain speech

27. **Say what it does, not how it feels.** "the database stays close at hand", "SQL you can read", "types that follow your schema" name a feeling. The fix names the mechanism or a number: "`.toSQL()` returns the exact string sent to the database", "a column rename fails the build". Ask what the sentence tells the reader to do or know, then write that. If you cannot restate it as a concrete instruction, fact, or number, cut it. One more check: if the sentence could appear unchanged in another project's docs, it says nothing about this one. Cut it.
28. **Shorten or split dense sentences.** If the reader has to backtrack to parse a sentence, break it in two or drop clauses. One idea per sentence.
29. **Active voice.** Catch "is/are/was/were + past participle" and name the actor. "Queries are validated" becomes "the compiler validates queries". Passive is fine only when the actor is unknown or genuinely does not matter.
30. **Cut adverbs, or use a stronger verb.** "runs quickly" becomes "is fast" or the number. "significantly improves" becomes the measured delta. An adverb propping up a weak verb means the verb is wrong.
31. **Prefer the plain word.** "utilize" becomes "use", "leverage" becomes "use", "facilitate" becomes "help", "numerous" becomes "many", "in the event that" becomes "if".

## Spanish

Patterns 1 to 31 are English. When the draft is in Spanish (LinkedIn posts, client email, landing copy in ES) apply these equivalents.

- **Vocabulario IA**: además, asimismo, crucial, fundamental, clave (as filler adjective), profundizar, potenciar, impulsar, fomentar, panorama, ecosistema (abstract), abordar, en aras de, cabe destacar, es importante señalar.
- **Aperturas de relleno**: "En un mundo cada vez más digital", "Hoy en día, las empresas deben", "En el vertiginoso mundo de la IA". Same rule as 6e. Open with something true about this one thing.
- **Adjetivos de capacidad**: robusto, integral, innovador, disruptivo, de vanguardia, escalable (unmeasured), a medida (when it means nothing).
- **"No solo X, sino Y"** and **"no se trata solo de X"**: state the point directly. Same as pattern 9.
- **Cierres genéricos**: "En definitiva", "En resumen", "El futuro es prometedor", "¿Y tú, qué opinas?" as a reflex CTA. Cut or replace with a specific ask.
- **Verbos hinchados**: "realizar" becomes "hacer", "efectuar" becomes "hacer", "posibilitar" becomes "dejar" or "permitir", "utilizar" becomes "usar".
- **Gerundio en cadena**: "permitiendo, garantizando, asegurando" at the end of a sentence. Same tell as pattern 3. Delete or turn into a fact.
- Voseo and contractions are fine in ES drafts if that is the author's voice. Formal is not the same as inflated.

## Register

Client-facing and public writing runs warm and plain. Contractions are fine. Some contexts expect formality and that is fair to give, but formal is not inflated. A formal sentence still says something.

Internal writing runs terse. Tickets, standup notes, and ADRs get the shortest form that survives being read by someone with no context in six months.

Neither register gets puffery.

## Related skills

- `copywriting` generates marketing copy. Unslop is the pass that runs after it, not a replacement.
- `caveman` owns terminal replies and agent reports. See the boundary table above.
- When delegating prose work to a subagent, inject this skill in the prompt. Agents do not auto-load skills.

## What not to do

Do not lengthen. If the edited version is longer than the draft, you did the wrong job. The one exception is a draft that hedges around a point that needs stating outright.

Do not neutralize. The author is allowed to say a vendor is a bad fit, an estimate is optimistic, or a client's data is a mess. Softening those into balanced phrasing removes the only part worth reading.

Do not invent detail. If a sentence needs a number or a source to work and neither is available, flag it for the author. Never fill the gap.
