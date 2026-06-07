# Humanizer Academic

A Claude Code skill that removes signs of AI-generated writing from academic medical papers, making them sound more natural and professionally written.

## Installation

### Recommended (clone directly into Claude Code skills directory)

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/matsuikentaro1/humanizer_academic.git ~/.claude/skills/humanizer_academic
```

### Manual install/update (only the skill file)

If you already have this repo cloned (or you downloaded `SKILL.md`), copy the skill file into Claude Code's skills directory:

```bash
mkdir -p ~/.claude/skills/humanizer_academic
cp SKILL.md ~/.claude/skills/humanizer_academic/
```

## Usage

In Claude Code, invoke the skill:

```
/humanizer_academic

[paste your manuscript text here]
```

Or ask Claude to humanize text directly:

```
Please humanize this academic text: [your text]
```

## Overview

Based on [Wikipedia's "Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) guide, adapted for medical and scientific literature. Examples are derived from the EMPA-REG OUTCOME trial publications and from the author's (K. Matsui) observations during academic manuscript editing.

### Key Insight from Wikipedia

> "LLMs use statistical algorithms to guess what should come next. The result tends toward the most statistically likely result that applies to the widest variety of cases."

## 37 Patterns Detected (with Before/After Examples)

### Content Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 1 | **Significance inflation** | "represents a pivotal challenge in the evolving landscape" | "is highly prevalent in patients with diabetes" |
| 2 | **Notability claims** | "landmark trial, led by renowned investigators" | "A total of 7020 patients..." |
| 3 | **Superficial -ing analyses** | "highlighting the cardioprotective effects" | Report data without interpretation |
| 4 | **Promotional language** | "groundbreaking study showcases the profound impact" | "empagliflozin reduced heart failure hospitalization" |
| 5 | **Vague attributions** | "Studies have shown... Experts argue..." | "In the EMPA-REG OUTCOME trial..." |
| 6 | **Formulaic challenges** | "Despite challenges... future outlook" | State specific limitations |

### Language Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 7 | **AI vocabulary** | "Additionally... pivotal... landscape... crucial" | Remove or replace with simple words |
| 8 | **Copula avoidance** | "serves as... standing as... representing" | "is" |
| 9 | **Negative parallelisms** | "Not only X but also Y" | "X and Y" |
| 10 | **Rule of three** | "efficacy, safety, and tolerability" | Use natural number of items |
| 11 | **Synonym cycling** | "Patients... Participants... Subjects" | "Patients" (consistent terminology) |
| 12 | **False ranges** | "from renal function to cardiac outcomes" | List benefits directly |

### Style Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 13 | **Em dash elimination (zero tolerance)** | "benefits—a 35% reduction—appeared early—" | Use commas, parentheses, or periods. ALL em dashes removed, no exceptions |
| 14 | **Title Case Headings** | "Statistical Analysis And Primary Endpoints" | "Statistical analysis and primary endpoints" |
| 15 | **Curly quotes** | \u201cclinically significant\u201d | "clinically significant" |

### Filler and Hedging

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 16 | **Filler phrases** | "In order to", "Due to the fact that" | "To", "Because" |
| 17 | **Redundant multi-layered hedging** | "may suggest... have the potential to confer" | "suggest... may reduce" (keep 1-2 hedges) |
| 18 | **Generic conclusions** | "The future looks bright" | Specific findings and implications |

### LLM-Specific Word Choice Patterns (v1.1.0)

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 19 | **"linked to" → "associated with"** | "has been linked to shorter sleep duration" | "has been reported to be associated with" |
| 20 | **"Beyond" → "In addition to"** | "Beyond the association with..." | "In addition to the association with..." |
| 21 | **"via" → "through"** | "obtained via the online form" | "obtained through an online form" |
| 22 | **Insufficient hedging** | "may reduce the risk of..." | "may help reduce the risk of..." |
| 23 | **Artificially condensed expressions** | "fatigue–sleepiness cycle", "mutual reinforcement" | "cycle of fatigue and sleepiness", "a self-reinforcing cycle, with each behavior possibly exacerbating the other" |
| 24 | **"where" as a non-locative connector** | "...at the most intensive level, where almost daily use was..." | "...at the most intensive level, with almost daily use..." |
| 25 | **"yield" as a result verb** | "did not yield stable estimates" | "failed to produce stable estimates" |
| 26 | **Underused classical terms (restore)** | "proportion of, aim of, was assessed, With regard to, to elucidate, These findings suggest" | "percentage of, purpose of, was measured, With respect to, to determine, The results suggest" |

### Additional AI Tropes (v1.3.0)

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 27 | **Latinate bloat verbs** | "utilized... leveraged... facilitate comparison" | "used... and existing data to compare" |
| 28 | **Vague magnitude quantifiers** | "numerous studies... a myriad of risk factors" | "several studies... hypertension, diabetes, and smoking" |
| 29 | **Metaphorical/figurative framing** | "shed light on... pave the way... at the forefront" | "clarify... may support the development of new therapies" |
| 30 | **Non-statistical "significant"** | "significantly improved well-being" (no test) | "substantially improved well-being" (reserve "significant" for P/CI) |
| 31 | **Adjective inflation** | "comprehensive, robust... nuanced insights into the multifaceted nature" | State what the analysis actually did |

### Self-certifying Modifiers and Formatting (v1.4.0)

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 32 | **Self-certifying modifiers** (asserting a quality instead of demonstrating it) | "a genuinely novel finding that truly transforms... it is well established that... arguably the most important" | Delete the modifier or replace with the data/citation that earns it |
| 33 | **AI formatting artifacts** | inline `**bold**` key terms, "**Key finding:**" header lists, stray Markdown/emoji | Continuous prose (unless the journal format calls for a list) |

Pattern 32 captures a single underlying move: the LLM attaches a word ("genuinely", "clearly", "it is well established that") that names the very quality the sentence is supposed to prove. The word does not create the quality — only evidence does. Pattern 7's vocabulary list was also expanded (bolstered, meticulous/meticulously, exhibited, "valuable/key insights", potential), and Pattern 16 now covers empty expletive openers ("It is worth noting", "When it comes to X").

### Role Templates and Structural Tells (v1.5.0)

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 34 | **"Plays a crucial role" templates** | "Inflammation plays a crucial role in... SGLT2 inhibitors play a pivotal role in..." | State the concrete mechanism or effect (with the specific markers/numbers) |
| 35 | **Structural monotony** (uniform sentence length, over-coordination, that-clause subjects) | "...reduced X and reduced Y and improved Z and was well tolerated and..." | Vary sentence length; subordinate or split; recast that-clause subjects |

Pattern 7's watch list was further extended with medical-specific AI vocabulary from the PubMed excess-vocabulary analyses (Matsui 2025): catalyze, harness, navigate, unveil, transformative, commendable, noteworthy, invaluable, milestone, prowess, "vital role", and others — with an explicit caution not to strip ordinary words (complex, critical, essential, potential) mechanically. Pattern 26 gained a 26d sub-table of classical phrasings that *declined* after ChatGPT ("treatment of", "all patients", "at the end of").

### Conversational Tells and Paste Hygiene (v1.6.0)

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 36 | **Conversational / SEO / reader-address tells** | "So what does this mean? Here's the kicker: in today's evolving landscape, you can't ignore these findings." | Third-person declarative: "These findings have direct implications for..." |
| 37 | **Paste hygiene** (invisible/zero-width Unicode, non-breaking spaces, curly punctuation, en-dash-as-punctuation) | zero-width spaces, ` `, curly quotes/apostrophes, `…`, bullets | Plain ASCII — with a `perl` verification command — while **preserving** scientific symbols (µ, ±, β, ≥) and numeric-range en dashes (95% CI 0.50–0.85) |

These two are high-precision because they never legitimately occur in formal manuscripts: any hit is almost certainly an AI artifact. Pattern 37 is a mechanical cleanup pass with a grep-style verification step (mirroring the Pattern 13 em-dash check) and explicit exceptions so confidence intervals, page ranges, and scientific notation are never corrupted.

### Preserved Academic Phrases (v1.1.0)

The skill now explicitly **preserves** standard academic phrases that were previously over-corrected:

- Transitional phrases: "Notably,", "Furthermore,", "In contrast,", etc.
- Attribution phrases with citations: "Prior studies have shown that...", "Evidence suggests that...", etc.

These are only flagged when used in excessive clusters or without supporting citations/data.

## Full Example

**Before (AI-sounding):**
> Heart failure represents a pivotal challenge in the evolving landscape of diabetes care, underscoring the critical importance of addressing cardiovascular comorbidities. This groundbreaking study showcases the profound impact of empagliflozin. Additionally, empagliflozin reduced the risk of hospitalization for heart failure or cardiovascular death by 34%—a remarkable finding—highlighting the cardioprotective effects of this intervention. The future looks bright for patients with type 2 diabetes.

**After (Humanized):**
> Heart failure is highly prevalent in patients with diabetes, occurring in more than one in five patients with type 2 diabetes aged over 65 years. In the EMPA-REG OUTCOME trial, empagliflozin reduced the risk of hospitalization for heart failure or cardiovascular death by 34%. The benefit was consistent in patients with and without heart failure at baseline.

## References

- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) - Primary source for AI writing patterns
- [WikiProject AI Cleanup](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup) - Maintaining organization
- Kobak D, et al. [Delving into LLM-assisted writing in biomedical publications through excess vocabulary](https://www.science.org/doi/10.1126/sciadv.adt3813). *Science Advances*. 2025. (Identifies the "excess vocabulary" verbs/adjectives that surged in PubMed abstracts after ChatGPT; basis for the extended Pattern 7 watch list and Patterns 30–35.)

### Examples Source

Medical paper examples (Patterns 1–18) are adapted from:

> Fitchett D, Inzucchi SE, Cannon CP, et al. Empagliflozin Reduced Mortality and Hospitalization for Heart Failure Across the Spectrum of Cardiovascular Risk in the EMPA-REG OUTCOME Trial. *Circulation*. 2019;139(11):1384-1395. doi:10.1161/CIRCULATIONAHA.118.037778

This article is published under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/) license.

Examples for Patterns 19–25 are based on the author's (K. Matsui) observations during academic manuscript editing in sleep medicine research.

Pattern 26 (Underused Classical Academic Terms) is adapted from the following papers that quantified words and phrases with decreased frequency in post-ChatGPT medical and scientific writing:

> Matsui K. Delving into PubMed records: some terms in medical writing have drastically changed after the arrival of ChatGPT. *medRxiv*. 2025. doi:[10.1101/2024.05.14.24307373](https://doi.org/10.1101/2024.05.14.24307373)

> Matsui K. Delving Into PubMed Records: How AI-Influenced Vocabulary has Transformed Medical Writing since ChatGPT. *Perspect Med Educ*. 2025;14(1):882-890. doi:[10.5334/pme.1929](https://doi.org/10.5334/pme.1929)

> Bao T, Zhao Y, Mao J, et al. Examining linguistic shifts in academic writing before and after the launch of ChatGPT: a study on preprint papers. *Scientometrics*. 2025;130:3597-3627. doi:[10.1007/s11192-025-05341-y](https://doi.org/10.1007/s11192-025-05341-y)

> Galpin R, Anderson B, Juzek TS. Exploring the Structure of AI-Induced Language Change in Scientific English. *Int FLAIRS Conf Proc*. 2025;38. doi:[10.32473/flairs.38.1.138958](https://doi.org/10.32473/flairs.38.1.138958)

## Related Work by the Author

> Matsui K. Delving Into PubMed Records: How AI-Influenced Vocabulary has Transformed Medical Writing since ChatGPT. *Perspect Med Educ*. 2025 Dec 2;14(1):882-890. doi:[10.5334/pme.1929](https://doi.org/10.5334/pme.1929)

This is a paper I wrote. Using PubMed records, I measured how frequently LLMs such as ChatGPT tend to overuse certain words in medical writing. Take a look if you're curious!

![Figure 1 from Matsui 2025 — AI-influenced vocabulary trends in PubMed](2025_Matsui_Delving_into_PubMed_Records_Fig1.png)

*The image above is Figure 1 from the paper cited above, reproduced under the [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/) license.*

## Version History

- **1.6.0** - Added Pattern 36 (conversational/SEO/reader-address tells: hook openers, "in today's landscape", second-person address, rhetorical questions → third-person declarative) and Pattern 37 (paste hygiene: remove invisible/zero-width Unicode characters, non-breaking and exotic spaces, curly quotes/apostrophes, ellipsis and bullet characters, and en dashes used as clause punctuation — with a perl verification command and explicit exceptions for scientific symbols and numeric-range en dashes). Added a second mandatory final-check step for invisible/typographic artifacts
- **1.5.0** - Added Pattern 34 ("plays a crucial/vital/key/pivotal role" templates → state the concrete mechanism) and Pattern 35 (structural monotony: uniform sentence length/low burstiness, phrasal over-coordination, that-clauses as grammatical subjects). Substantially extended Pattern 7's watch list with medical-specific AI vocabulary from the PubMed excess-vocabulary analyses (verbs, adjectives, adverbs, nouns, phrases), with a caution against mechanically stripping ordinary medical words. Added Pattern 26d (classical phrasings that declined after ChatGPT: "treatment of", "all patients", "at the end of"). Expanded Pattern 33 to cover excess exclamation marks and ellipses
- **1.4.0** - Added Pattern 32 (self-certifying modifiers: words that assert a quality instead of demonstrating it — genuinely/truly/clearly/remarkably, "it is well established that", arguably; the word never creates the quality, only evidence does) and Pattern 33 (AI formatting artifacts: inline boldface, "Key finding:" header lists, stray Markdown/emoji → continuous prose). Expanded Pattern 7's vocabulary list (bolstered, meticulous/meticulously, exhibited, "valuable/key insights", potential) with citation to the PubMed excess-vocabulary analyses, and expanded Pattern 16 to cover empty expletive openers ("It is worth noting", "It should be noted", "When it comes to X")
- **1.3.0** - Added five new AI-trope patterns: 27 (Latinate bloat verbs: utilize/leverage/facilitate → use/help/enable), 28 (vague magnitude quantifiers: numerous/myriad/plethora → specific counts or "many/several"), 29 (metaphorical framing: shed light on/pave the way/at the forefront → literal phrasing), 30 (non-statistical use of "significant"/"significantly" → reserve for reported statistical significance; otherwise "substantial/marked"), and 31 (self-praising adjective inflation: comprehensive/robust/holistic/multifaceted/nuanced)
- **1.2.0** - Added Pattern 26 (Underused Classical Academic Terms: restore classical expressions that AI tends to avoid, such as "percentage of", "purpose of", "was measured", "With respect to", "to determine"); enhanced Pattern 3 to recognize -ing phrases as implicit replacements for "therefore"/"thus" and added a restoration example; removed the "With respect to" line from Pattern 16 filler phrases (that substitution was the wrong direction — AI avoids "With respect to" rather than overusing it)
- **1.1.3** - Added patterns 24 ("where" as a non-locative connector) and 25 ("yield" as a result verb); added author paper reference and Fig.1 to README
- **1.1.2** - Pattern 13: Em dash rule upgraded to zero-tolerance elimination (no exceptions, mandatory final check step)
- **1.1.1** - Merged compressed noun-dash phrases and vague abstractions into single "Artificially condensed expressions" pattern (23)
- **1.1.0** - Added LLM-specific word choice patterns (19-23), preserved legitimate academic phrases, fixed hedging guidance consistency
- **1.0.0** - Initial release adapted for academic medical writing

## License

MIT

Based on [blader/humanizer](https://github.com/blader/humanizer).
