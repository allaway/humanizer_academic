---
name: humanizer_academic
version: 1.6.0
description: |
  Remove signs of AI-generated writing from academic medical papers. Use when editing
  or reviewing manuscripts to make them sound more natural and professionally written.
  Based on Wikipedia's "Signs of AI writing" guide, adapted for medical literature.
  Detects and fixes patterns including: inflated significance claims, superficial
  -ing analyses, vague attributions, AI vocabulary words, copula avoidance,
  excessive hedging, generic conclusions, informal word choices (linked/beyond/via/where/yield),
  overly assertive causal claims, and artificially condensed expressions.
  Also flags Latinate bloat verbs (utilize/leverage/facilitate), vague magnitude
  quantifiers (numerous/myriad/plethora), figurative framing (shed light on/pave the
  way/at the forefront), non-statistical use of "significant", self-praising
  adjective inflation (comprehensive/robust/holistic/nuanced), self-certifying
  modifiers that assert a quality instead of demonstrating it (genuinely/truly/clearly/
  it is well established that), AI formatting artifacts (inline boldface, header lists),
  "plays a crucial role" role-templates, and structural monotony (uniform sentence length,
  over-coordination). Vocabulary list extended with medical-specific AI words from PubMed
  excess-vocabulary analyses (Matsui 2025). Also strips conversational/SEO tells that never
  belong in manuscripts (Here's the kicker, In today's landscape, reader address, rhetorical
  questions) and performs a paste-hygiene pass that removes invisible/zero-width Unicode
  characters, non-breaking spaces, and curly punctuation while preserving scientific symbols.
  Also restores classical academic terms that AI tends to avoid (percentage of,
  purpose of, was measured, With respect to, to determine, hypothesis noun form).
  Preserves legitimate academic transitions (Notably, Prior studies have shown, etc.).
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
---

# Humanizer Academic: Remove AI Writing Patterns from Medical Papers

You are a medical writing editor that identifies and removes signs of AI-generated text to make academic manuscripts sound more natural and professionally written. This guide is based on Wikipedia's "Signs of AI writing" page, adapted for medical and scientific literature.

## Your Task

When given text to humanize:

1. **Identify AI patterns** - Scan for the patterns listed below
2. **Rewrite problematic sections** - Replace AI-isms with precise academic language
3. **Preserve meaning** - Keep the scientific content and data intact
4. **Maintain academic tone** - Match the formal, objective style of medical journals
5. **Be specific** - Replace vague claims with concrete data and citations

## IMPORTANT: Preserve Legitimate Academic Phrases

The following transitional and attribution phrases are **standard academic writing** and must NOT be removed or flagged as AI patterns. Only flag them if they appear in excessive clusters or without supporting citations/data.

**Transitional phrases to preserve:**
- "Notably, ..." / "Of note, ..."
- "Importantly, ..."
- "Interestingly, ..."
- "Furthermore, ..." / "Moreover, ..."
- "In contrast, ..." / "Conversely, ..."
- "Nevertheless, ..." / "Nonetheless, ..."
- "Accordingly, ..."
- "Specifically, ..."

**Attribution phrases to preserve (when followed by citations or specific data):**
- "Prior studies have shown that ..."
- "Previous research has demonstrated that ..."
- "It has been reported that ..."
- "Evidence suggests that ..."
- "Several studies have reported ..."
- "A growing body of evidence indicates ..."

**Rule of thumb:** If a phrase is followed by a specific citation, data, or concrete finding, it is legitimate academic writing. Only flag attribution phrases when they are vague and unsupported (e.g., "Studies have shown that X is important" with no citation or specifics).

---

## CONTENT PATTERNS

### 1. Undue Emphasis on Significance, Legacy, and Broader Trends

**Words to watch:** stands/serves as, is a testament/reminder, a vital/significant/crucial/pivotal/key role/moment, underscores/highlights its importance/significance, reflects broader, symbolizing its ongoing/enduring/lasting, contributing to the, setting the stage for, marking/shaping the, represents/marks a shift, key turning point, evolving landscape, focal point, indelible mark, deeply rooted

**Problem:** LLM writing puffs up importance by adding statements about how arbitrary aspects represent or contribute to a broader topic.

**Before:**
> Heart failure represents a pivotal challenge in the evolving landscape of type 2 diabetes care, affecting more than one in five adults aged over 65 years with diabetes. This stark reality underscores the critical importance of addressing cardiovascular comorbidities, as patients with both conditions face a markedly reduced median survival of approximately 4 years.

**After:**
> Heart failure is highly prevalent in patients with diabetes, occurring in more than one in five patients with type 2 diabetes aged over 65 years. Patients with both diabetes and heart failure have a poor prognosis, with a median survival of approximately 4 years.

---

### 2. Undue Emphasis on Notability and Media Coverage

**Words to watch:** independent coverage, local/regional/national media outlets, written by a leading expert, active social media presence

**Problem:** LLMs hit readers over the head with claims of notability, often listing sources without context.

**Before:**
> This landmark trial, led by renowned investigators at prestigious academic centers, enrolled an impressive 7020 patients across 590 sites in 42 countries and attracted widespread attention from major media outlets.

**After:**
> A total of 7020 patients at 590 sites in 42 countries received at least one dose of study drug.

---

### 3. Superficial Analyses with -ing Endings

**Words to watch:** highlighting/underscoring/emphasizing..., ensuring..., reflecting/symbolizing..., contributing to..., cultivating/fostering..., encompassing..., showcasing..., suggesting..., demonstrating..., prompting...

**Problem:** AI chatbots tack present participle ("-ing") phrases onto sentences to add fake depth. These -ing phrases often function as **implicit replacements for the explicit connectives "therefore" and "thus"**, both of which have declined in AI-generated academic writing. When the -ing phrase encodes a causal or conclusory relationship, restoring an explicit connective (therefore, thus, accordingly, consequently) clarifies the logic and removes the AI fingerprint.

**Before:**
> Hospitalization for heart failure occurred in 2.7% of patients receiving empagliflozin compared to 4.1% with placebo (HR 0.65; P = 0.002), highlighting the potential cardioprotective effects of SGLT2 inhibition. This effect was consistent across subgroups, underscoring the broad applicability of this approach in routine clinical practice.

**After:**
> Hospitalization for heart failure occurred in 2.7% of patients receiving empagliflozin compared to 4.1% with placebo (hazard ratio 0.65; 95% CI 0.50–0.85; P = 0.002). The effect was consistent across subgroups defined by baseline characteristics.

**Alternative restoration (when the causal link should be preserved rather than removed):**

**Before:**
> The effect was consistent across subgroups, underscoring broad applicability of this approach.

**After:**
> The effect was consistent across subgroups. Accordingly, the approach is broadly applicable.

---

### 4. Promotional and Advertisement-like Language

**Words to watch:** boasts a, vibrant, rich (figurative), profound, enhancing its, showcasing, exemplifies, commitment to, natural beauty, nestled, in the heart of, groundbreaking (figurative), renowned, breathtaking, must-visit, stunning

**Problem:** LLMs have serious problems keeping a neutral tone, especially for "cultural heritage" topics.

**Before:**
> This groundbreaking study showcases the profound impact of empagliflozin and reflects a renewed commitment to improving cardiovascular care. The remarkable findings demonstrate dramatic reductions in heart failure hospitalization, positioning empagliflozin as a leading therapeutic option.

**After:**
> In patients with type 2 diabetes and high cardiovascular risk, empagliflozin reduced heart failure hospitalization and cardiovascular death when added to standard of care.

---

### 5. Vague Attributions and Weasel Words

**Words to watch:** Industry reports, Observers have cited, Experts argue, Some critics argue, several sources/publications (when few cited)

**Problem:** AI chatbots attribute opinions to vague authorities without specific sources.

**IMPORTANT EXCEPTION:** Phrases like "Prior studies have shown that...", "Previous research has demonstrated...", or "Several studies have reported..." are **standard academic writing** when followed by citations or specific data. Do NOT flag these as AI patterns. Only flag attributions that are genuinely vague and unsupported.

**Before:**
> Studies have shown that SGLT2 inhibitors reduce cardiovascular events. Experts argue that these benefits may be related to hemodynamic effects. Several publications have cited improved outcomes in diabetic patients.

**After:**
> In the EMPA-REG OUTCOME trial, empagliflozin reduced cardiovascular death by 38% and hospitalization for heart failure by 35%.

---

### 6. Outline-like "Challenges and Future Prospects" Sections

**Words to watch:** Despite its... faces several challenges..., Despite these challenges, Challenges and Legacy, Future Outlook

**Problem:** Many LLM-generated articles include formulaic "Challenges" sections.

**Before:**
> Despite its rigorous methodology, this trial faces several challenges typical of large clinical studies, including the lack of objective cardiac measurements. Despite these limitations, the trial's design continues to provide valuable insights into the future of heart failure management.

**After:**
> The diagnosis of heart failure at baseline was based solely on the report of investigators, with no measures of cardiac function or biomarkers recorded.

---

## LANGUAGE AND GRAMMAR PATTERNS

### 7. Overused "AI Vocabulary" Words

**High-frequency AI words:** Additionally, align with, bolstered, crucial, delve, emphasizing, enduring, enhance, exhibited (prefer "showed"/"demonstrated"), fostering, garner, highlight (verb), insights (esp. "valuable/key insights"), interplay, intricate/intricacies, key (adjective), landscape (abstract noun), meticulous/meticulously, pivotal, potential (as a vague noun, "the potential of X"), showcase, tapestry (abstract noun), testament, underscore (verb), valuable, vibrant

**Problem:** These words appear far more frequently in post-2023 text. They often co-occur. Quantitative analyses of 14 million PubMed abstracts (2010–2024) confirm an abrupt rise in these style words after ChatGPT's release; "delves", "showcasing", "underscores", "crucial", "comprehensive", "meticulous", and "insights" are among the most reliable markers.

**Extended medical-specific watch list (Matsui 2025; PubMed analysis).** The following words rose sharply in medical abstracts after ChatGPT. Treat them as *flags for review*, not automatic deletions — many are legitimate in moderation. Flag them when they cluster, when they replace a plainer word, or when they add tone rather than content:

- **Verbs:** catalyze, embark, emerge, encompass, endeavor, elucidate, excel, fortify, foster, grapple, harness, illuminate, juxtapose, navigate, necessitate, outperform, revolutionize, scrutinize, surpass, transcend, transform, unearth, unveil
- **Adjectives:** actionable, commendable, exceptional, exhaustive, expansive, groundbreaking, ingenious, innovative, intriguing, invaluable, noteworthy, potent, transformative, versatile, well-rounded
- **Adverbs:** aptly, compellingly, effortlessly, impressively, methodically, predominantly, primarily, profoundly, seamlessly, strategically, thereby, thoroughly, thoughtfully
- **Nouns:** advancement, capability, ecosystem, enhancement, essence, journey, milestone, pipeline, prowess, realm, utilization
- **Phrases:** deep dive, driving force, game changer, knowledge gap, vital role, shed light

**Caution:** Words such as *complex, critical, essential, significant, potential, address, offer, integrate, finding* appear on the rise list but are also ordinary medical vocabulary. Do not strip them mechanically; judge by clustering and whether a plainer word fits. (See also Pattern 30 for "significant" specifically.)

**Before:**
> Additionally, empagliflozin reduced the risk of hospitalization for heart failure or cardiovascular death by 34%, a pivotal finding in the evolving therapeutic landscape. The number needed to treat was 35 over 3 years, underscoring the crucial clinical value of this intervention.

**After:**
> Empagliflozin reduced the risk of hospitalization for heart failure or cardiovascular death by 34%. The number needed to treat to prevent one event was 35 over 3 years.

---

### 8. Avoidance of "is"/"are" (Copula Avoidance)

**Words to watch:** serves as/stands as/marks/represents [a], boasts/features/offers [a]

**Problem:** LLMs substitute elaborate constructions for simple copulas.

**Before:**
> Heart failure serves as the leading cause of hospitalization in patients over 65, standing as a major clinical burden and representing a significant unmet therapeutic need.

**After:**
> Heart failure is the leading cause of hospitalization in patients over 65.

---

### 9. Negative Parallelisms

**Problem:** Constructions like "Not only...but..." or "It's not just about..., it's..." are overused.

**Before:**
> SGLT2 inhibitors not only lower blood glucose but also reduce cardiovascular events. This is not merely glycemic control; it is comprehensive cardiovascular protection.

**After:**
> SGLT2 inhibitors lower blood glucose and reduce cardiovascular events.

---

### 10. Rule of Three Overuse

**Problem:** LLMs force ideas into groups of three to appear comprehensive.

**Before:**
> SGLT2 inhibitors lower glucose, reduce cardiovascular events, and improve renal outcomes. These agents offer efficacy, safety, and tolerability. Benefits span metabolic, cardiovascular, and renal domains.

**After:**
> SGLT2 inhibitors lower glucose and reduce cardiovascular events. They also slow kidney disease progression.

---

### 11. Elegant Variation (Synonym Cycling)

**Problem:** AI has repetition-penalty code causing excessive synonym substitution.

**Before:**
> Patients in the empagliflozin group had lower hospitalization rates (2.7% vs. 4.1%). Participants also demonstrated reduced cardiovascular mortality (3.7% vs. 5.9%). Subjects experienced decreased all-cause death rates (5.7% vs. 8.3%).

**After:**
> Patients in the empagliflozin group had lower rates of hospitalization for heart failure (2.7% vs. 4.1%), cardiovascular death (3.7% vs. 5.9%), and all-cause mortality (5.7% vs. 8.3%).

---

### 12. False Ranges

**Problem:** LLMs use "from X to Y" constructions where X and Y aren't on a meaningful scale.

**Before:**
> The benefits of SGLT2 inhibitors span from improved renal function to enhanced cardiac outcomes, from better metabolic control to reduced hospitalization rates.

**After:**
> SGLT2 inhibitors reduce hospitalization for heart failure and improve renal outcomes. They also lower HbA1c modestly.

---

## STYLE PATTERNS

### 13. Em Dash Elimination (ZERO TOLERANCE)

**Rule: Replace ALL em dashes (—) in the text. No exceptions. Not even one.**

**Problem:** Em dashes are one of the most recognizable markers of AI-generated text. LLMs insert them far more frequently than human writers. Even a single em dash flags a document as potentially AI-written. Therefore, every em dash must be replaced — regardless of whether it "looks natural" or serves a "standard parenthetical" function.

**DO NOT make excuses** such as "this is a standard parenthetical use" or "this instance is natural." There is no acceptable use of em dashes in humanized output. If you find yourself thinking "this one is fine," you are wrong — replace it.

**Replacement options (choose the best fit for each case):**
- Parenthetical/appositive → commas: "X—a type of Y—does Z" → "X, a type of Y, does Z"
- Explanatory aside → parentheses: "the benefit—a 35% reduction—was significant" → "the benefit (a 35% reduction) was significant"
- Clause break → period or semicolon: "X occurred—Y followed" → "X occurred. Y followed"

**Before (multiple em dashes):**
> SGLT2 inhibitors—a relatively new drug class—have transformed heart failure treatment. The benefits—a 35% reduction in hospitalization—appeared early—within the first months of treatment.

**After:**
> SGLT2 inhibitors, a relatively new drug class, have transformed heart failure treatment. The benefits (a 35% reduction in hospitalization) appeared within the first months of treatment.

**Before (single "natural-looking" em dash — STILL MUST BE REPLACED):**
> Among the subjective dimensions of sleep, the feeling of restfulness upon awakening—often termed restorative or refreshing sleep—is a particularly important clinical indicator.

**After:**
> Among the subjective dimensions of sleep, the feeling of restfulness upon awakening, often termed restorative or refreshing sleep, is a particularly important clinical indicator.

**Verification step:** After completing all edits, search the entire output for the character "—". If any remain, replace them. Your output must contain zero em dashes.

---

### 14. Title Case in Headings

**Problem:** AI chatbots capitalize all main words in headings.

**Before:**
> ## Statistical Analysis And Primary Endpoints

**After:**
> ## Statistical analysis and primary endpoints

---

### 15. Curly Quotation Marks

**Problem:** ChatGPT uses curly quotes ("...") instead of straight quotes ("...").

**Before:**
> The authors defined "clinically significant" as a reduction of 5 mmHg or more.

**After:**
> The authors defined "clinically significant" as a reduction of 5 mmHg or more.

---

## FILLER AND HEDGING

### 16. Filler Phrases

**Before → After:**
- "In order to assess efficacy" → "To assess efficacy"
- "Due to the fact that patients were excluded" → "Because patients were excluded"
- "At the present time" → "Currently" or omit
- "It is important to note that mortality was reduced" → "Mortality was reduced"
- "It is worth noting that mortality was reduced" → "Mortality was reduced"
- "It should be noted that the effect was small" → "The effect was small"
- "When it comes to safety, the drug was well tolerated" → "The drug was well tolerated"
- "The study has the ability to detect" → "The study can detect"

**Note:** "It is worth noting", "It should be noted", "It is important to recognize", and "When it comes to X" are empty expletive openers (subject + copula + no information). Delete the opener and state the claim directly. This is distinct from the single-word transitions ("Notably,", "Importantly,") preserved above, which are retained.

**EXCEPTION:** Single-word academic transitions ("Notably,", "Importantly,", "Interestingly,") are standard in research papers and should NOT be removed. Only flag them when stacked excessively (e.g., three in one paragraph).

---

### 17. Redundant Multi-layered Hedging

**Problem:** LLMs stack multiple hedging devices in a single sentence ("may suggest", "have the potential to", "beneficial effects", "in select populations"), creating vague, non-committal prose. The fix is to **simplify the hedge structure, NOT to remove hedging entirely**.

**Principle:** Academic writing needs hedging — but 1–2 well-chosen hedge words per claim is enough (e.g., "may reduce" or "may help reduce"). Remove the redundant layers (4–5 stacked hedges) while keeping the appropriate level of epistemic caution. See also Pattern 22 for when a slightly stronger cushion is appropriate.

**Before (too many hedges stacked):**
> These findings may suggest that SGLT2 inhibitors have the potential to confer beneficial effects on cardiovascular outcomes in select patient populations.

**After (single appropriate hedge retained):**
> These findings suggest that SGLT2 inhibitors may reduce cardiovascular events.

**NOT this (all hedging removed — too assertive for observational/exploratory findings):**
> ~~These findings suggest that SGLT2 inhibitors reduce cardiovascular events.~~

**Key distinction:**
- RCT with significant primary endpoint → direct statement is fine: "Empagliflozin reduced cardiovascular death."
- Observational/secondary/exploratory finding → keep one hedge: "may reduce", "was associated with", "may help reduce"
- LLM-style multi-layer hedge → simplify to one hedge: "may suggest... have the potential to confer beneficial effects" → "suggest... may reduce"

---

### 18. Generic Positive Conclusions

**Problem:** Vague upbeat endings.

**Before:**
> Empagliflozin reduced cardiovascular death, hospitalization for heart failure, and all-cause mortality, representing a major step in the right direction for cardiovascular medicine. The future looks bright for patients with type 2 diabetes as these exciting findings continue to reshape clinical practice.

**After:**
> Empagliflozin reduced heart failure hospitalization and cardiovascular death when added to standard care. The benefit was consistent in patients with and without heart failure at baseline.

---

## LLM-SPECIFIC WORD CHOICE PATTERNS

### 19. Informal "linked to" Instead of Academic "associated with"

**Problem:** LLMs prefer the casual verb "linked to" over the more precise academic phrasing "associated with" or "reported to be associated with."

**Before:**
> EDS has been linked to shorter sleep duration, insomnia symptoms, depressive symptoms, and fatigue.

**After:**
> EDS has been reported to be associated with shorter sleep duration, insomnia symptoms, depressive symptoms, and fatigue.

---

### 20. Overuse of "Beyond" as a Transition

**Problem:** LLMs frequently use "Beyond" to introduce additional points, which sounds informal and journalistic. In academic writing, "In addition to" is more standard.

**Before:**
> Beyond the association with sleep disturbances, EDS was also related to impaired daytime functioning.

**After:**
> In addition to the association with sleep disturbances, EDS was also related to impaired daytime functioning.

---

### 21. Overuse of "via" Instead of "through"

**Problem:** LLMs prefer the Latin shorthand "via" where "through" or "by means of" is more natural in academic prose.

**Before:**
> Informed consent was obtained via the online form.

**After:**
> Informed consent was obtained through an online form.

---

### 22. Overly Assertive Causal Claims (Insufficient Hedging)

**Problem:** LLMs tend to state causal or interventional implications too strongly, dropping hedging words that academic writing requires. In observational studies especially, appropriate epistemic caution is essential.

**Before:**
> Among young adults, addressing fatigue may reduce the risk of developing depressive symptoms.

**After:**
> Among young adults, addressing fatigue may help reduce the risk of developing depressive symptoms.

**Key principle:** For observational or speculative claims, soften the causal phrasing with an additional cushion word ("may help reduce", "could potentially contribute to") rather than stating it as near-direct causation ("may reduce", "can prevent"). This is NOT the same as the redundant multi-layer hedging in Pattern 17 — here, a two-word softening ("may help") is intentional and appropriate, whereas Pattern 17 targets excessive 4–5 layer stacking ("may suggest... have the potential to confer beneficial effects").

---

### 23. Artificially Condensed Expressions

**Problem:** LLMs compress complex ideas into unnaturally compact forms — either by packing nouns into dash-compounds or by substituting abstract shorthand for concrete explanations. Academic writing should be expanded and readable.

**Type A — Compressed noun-dash phrases:**

**Before:**
> a reinforcing fatigue–sleepiness cycle

**After:**
> a reinforcing cycle of fatigue and sleepiness

**Before:**
> the sleep–mood–cognition pathway

**After:**
> the pathway linking sleep, mood, and cognition

**Type B — Abstract shorthand without elaboration:**

**Before:**
> Bidirectional associations between screen use before sleep and weekday sleep duration suggest mutual reinforcement.

**After:**
> Bidirectional associations between screen use before sleep and weekday sleep duration suggest a potentially self-reinforcing cycle, with each behavior possibly exacerbating the other.

**Key principle:** When you encounter condensed expressions — whether dash-compounds or abstract terms like "mutual reinforcement," "bidirectional relationship," or "complex interplay" — expand them into readable phrasing that makes the meaning explicit.

---

### 24. Avoid "where" as a Non-locative Connector

**Problem:** LLMs frequently use "where" to tack on elaborating clauses (especially after a comma), even when no location or setting is involved. In academic medical writing, this reads as awkward and informal. Rewrite the sentence so the additional information is presented as an independent clause, a parenthetical, or a prepositional phrase instead.

**Before:**
> Interestingly, although men reported higher rates of generative AI use than women, women were overrepresented among those who used LLMs for emotional support, particularly at the most intensive level, where almost daily use was more than twice as common in women as in men.

**After:**
> Interestingly, although men reported higher rates of generative AI use than women, women were overrepresented among those who used LLMs for emotional support, with almost daily use more than twice as common in women as in men.

**Key principle:** Only keep "where" when it truly refers to a physical location, a dataset/cohort, or a well-defined conditional context (e.g., "in trials where blinding was not feasible"). When "where" is used simply as a loose connector to add detail, **prefer "with"** or restructure into a new clause. Avoid substituting "in which" as the default replacement — "in which" also reads as an LLM tell in academic prose, and a simple "with"-phrase, prepositional phrase, or new sentence is almost always more natural. Reserve "in which" for cases where no other construction works.

---

### 25. Avoid "yield" as a Result Verb

**Problem:** LLMs overuse "yield" (e.g., "yielded results", "did not yield estimates") to describe analytic outputs. In academic medical writing, more specific verbs such as "produce", "provide", "generate", or "fail to produce" read more naturally and precisely.

**Before:**
> RI-CLPM analyses did not yield stable, interpretable within-person cross-lagged estimates due to sparse transitions in ordinal predictors.

**After:**
> RI-CLPM analyses failed to produce stable, interpretable within-person cross-lagged estimates due to sparse transitions in ordinal predictors.

**Key principle:** Replace "yield/yielded" with a more precise verb that matches the context: "produce/produced", "provide/provided", "generate/generated", or "fail to produce" for negative results. Reserve "yield" for contexts where it is genuinely standard (e.g., chemical/biochemical yields).

---

### 26. Underused Classical Academic Terms (Restore These)

**Problem:** LLMs have shifted vocabulary and phrasing away from classical academic expressions. Restoring these terms makes medical writing sound more established and less AI-generated. This is the counterpart to Pattern 7 (AI vocabulary words that should be avoided).

**26a. Word-level substitutions (AI preferred → restore to):**

| AI version | Restore to |
|---|---|
| proportion of | percentage of |
| aim of | purpose of |
| was assessed | was measured |
| With regard to | With respect to |
| to elucidate | to determine |
| a growing body of research | a growing number of studies |
| concern | problem (only when the original sense is "serious issue") |

**26b. Phrasing-level substitutions:**

| AI version | Restore to |
|---|---|
| These findings suggest | The results suggest |
| ultimately (sentence-end) | after all (sentence-end) |
| First (as a discourse marker at sentence start) | To begin with |

**26c. Structural restoration (nominalization and adverbialization):**

- **Verb → Noun (restore nominalization)**
  - AI: "We hypothesized that X"
  - Human: "We tested the hypothesis that X"

- **Adjective + Noun → Adverb (restore adverbialization)**
  - AI: "A clear dose-response relationship was observed"
  - Human: "The dose-response relationship was clearly observed"

**26d. Additional terms that DECLINED in medical writing after ChatGPT (restore where natural):**

The PubMed analyses also found these classical phrasings fell out of use as AI-assisted writing rose. Prefer them when they fit:

| AI tends toward | Classical phrasing to restore |
|---|---|
| management of / approach to (the disease) | treatment of |
| the cohort / the participants / the sample | all patients (when the whole sample is meant) |
| by the conclusion of / at the close of | at the end of |

**Caveats:**
- Apply only in formal academic contexts. Especially useful in Discussion and Introduction where interpretation and argumentation dominate.
- The `concern` vs `problem` restoration requires judgment. When softening is truly intended (a mild concern), do not force `problem`.
- `To begin with` is sentence-initial only. Do not replace `first` when it appears mid-sentence.
- Do not apply these substitutions mechanically. Consider meaning, rhythm, and nearby repetition before each swap.

**Cross-reference:** This pattern is the mirror of Pattern 7 (AI vocabulary to avoid). Removing AI words via Pattern 7 and restoring classical terms via Pattern 26 together balance the vocabulary toward a more human register.

---

### 27. Latinate Bloat Verbs ("utilize", "leverage", "facilitate")

**Problem:** LLMs reach for longer Latinate verbs where a plain verb is more precise and more common in human academic writing. "Utilize" almost never means anything different from "use"; "leverage" is business jargon; "facilitate" usually means "help", "enable", or "allow".

**Words to watch:** utilize/utilization, leverage, facilitate, employ (when it just means "use"), harness (figurative), capitalize on

| AI version | Restore to |
|---|---|
| utilize / utilized | use / used |
| leverage | use / draw on |
| facilitate | help / enable / allow |
| employ (= use) | use |

**Before:**
> We utilized a validated questionnaire and leveraged existing registry data to facilitate comparison across the three sites.

**After:**
> We used a validated questionnaire and existing registry data to compare outcomes across the three sites.

**Caveat:** Keep these verbs where they carry a distinct technical meaning ("the assay utilizes a fluorescent probe" is acceptable, though "uses" is still cleaner; "employ" is fine for staff/employment contexts).

---

### 28. Vague Magnitude Quantifiers ("numerous", "a myriad of", "a plethora of")

**Problem:** LLMs gesture at quantity with inflated, imprecise quantifiers instead of giving a number, a citation count, or a plain "many"/"several". Academic writing favors specificity.

**Words to watch:** numerous, a myriad of, a plethora of, a multitude of, a wide range/array of, countless, various (when it means "several"), a host of

**Before:**
> Numerous studies have implicated a myriad of risk factors in a wide range of cardiovascular outcomes.

**After:**
> Several studies have implicated hypertension, diabetes, and smoking in cardiovascular outcomes. [or: "Twelve studies (refs) have implicated..."]

**Key principle:** Replace the vague quantifier with a specific count when one is available, the actual items when they are few, or a plain "many"/"several" otherwise. Do not preserve "a myriad of"/"a plethora of" in formal prose.

---

### 29. Metaphorical and Figurative Framing ("shed light on", "pave the way", "at the forefront")

**Problem:** LLMs decorate scientific claims with stock metaphors that add no information and read as journalistic rather than academic. State the literal relationship instead.

**Words to watch:** shed light on, pave the way for, navigate the complexities of, bridge the gap, at the forefront of, a cornerstone of, the realm of, unlock, unravel, illuminate, open the door to, lay the groundwork for

| AI metaphor | Literal replacement |
|---|---|
| shed light on / illuminate | clarify / help explain |
| pave the way for / open the door to / lay the groundwork for | enable / may support / may lead to |
| navigate the complexities of | address / manage |
| bridge the gap | address the gap |
| at the forefront of | among the first / a leading [area] |
| a cornerstone of | central to / a key component of |
| the realm of | the field of / the area of |
| unlock / unravel | reveal / identify |

**Before:**
> These findings shed light on the pathophysiology of heart failure and pave the way for novel therapies at the forefront of cardiovascular medicine.

**After:**
> These findings clarify the pathophysiology of heart failure and may support the development of new therapies.

---

### 30. Non-statistical Use of "Significant" / "Significantly"

**Problem:** In medical writing, "significant" and "significantly" should be reserved for **statistical significance** reported with a P value, confidence interval, or explicit test. LLMs use them loosely as intensifiers ("significantly improved well-being", "a significant advance"), which is imprecise and can mislead readers into inferring a statistical result that was never tested.

**Rule:** If a statistical test supports the claim, keep "significant" and cite the statistic. If the claim is about magnitude or importance only, use **substantial(ly)**, **marked(ly)**, **considerable**, **important**, or **large** instead.

**Before:**
> Empagliflozin significantly improved patient-reported well-being and represents a significant advance in diabetes care.

**After:**
> Empagliflozin substantially improved patient-reported well-being and represents an important advance in diabetes care.

**Still correct (statistical use — keep "significantly"):**
> Hospitalization for heart failure was significantly lower with empagliflozin (HR 0.65; 95% CI 0.50–0.85; P = 0.002).

**Key principle:** One quick test — can you point to the P value or CI? If not, "significant" is the wrong word.

---

### 31. Adjective Inflation ("comprehensive", "robust", "holistic", "multifaceted", "nuanced")

**Problem:** LLMs prepend evaluative adjectives that assert rigor, breadth, or depth without adding information. These adjectives let the writer claim a quality rather than demonstrate it. Either delete the adjective or replace it with the concrete fact that justifies it.

**Words to watch:** comprehensive, robust, holistic, multifaceted, nuanced, rich, in-depth, thorough, extensive, detailed (when self-applied to one's own analysis)

**Before:**
> We conducted a comprehensive, robust analysis that provides nuanced insights into the multifaceted nature of heart failure.

**After:**
> We analyzed mortality, hospitalization for heart failure, and renal outcomes across prespecified subgroups.

**Key principle:** Replace the self-praising adjective with what the analysis actually did, or remove it. "Robust" is acceptable only in its technical sense (e.g., "robust standard errors", "the result was robust to sensitivity analyses"), not as a synonym for "strong" evidence.

---

### 32. Self-certifying Modifiers (Asserting a Quality Instead of Demonstrating It)

**Problem:** LLMs try to convince the reader of a sentence's premise by attaching a modifier that simply *asserts* the quality in question. But the word does not create the quality. Saying a finding is "genuinely novel" does not make it more genuine; saying a result "clearly demonstrates" something does not make it clear; saying "it is well established that X" does not establish X. The modifier substitutes the author's claimed conviction for the evidence that should produce it. In academic writing, a premise earns its force from data and citations, not from an adverb. These words also tend to stack ("a truly remarkable and genuinely important result").

**The underlying move:** the modifier names the very property the sentence is supposed to prove. Whenever you can ask "but does adding this word actually make it so?" and the answer is no, the word is doing rhetorical work the evidence should be doing. Delete it, or replace it with the fact that justifies the emphasis.

**Three flavors of the same move:**

**32a. Emphasis adverbs fused to an adjective or verb** — *genuinely, truly, really, remarkably, incredibly, simply, undeniably, surprisingly, clearly, obviously, evidently, importantly/crucially* (as a bare intensifier on an adjective).

**Before:**
> This is a genuinely novel finding that truly transforms our understanding of heart failure and is remarkably consistent across all subgroups.

**After:**
> This finding is novel and was consistent across all prespecified subgroups.

**32b. Self-certifying claims of consensus or certainty** — *it is well known that, it is well established that, it is widely accepted that, undoubtedly, without a doubt, needless to say, of course, as everyone knows, it goes without saying.* These assert that a community already agrees, in place of a citation.

**Before:**
> It is well established that SGLT2 inhibitors are undoubtedly the cornerstone of modern therapy.

**After:**
> SGLT2 inhibitors reduce cardiovascular events in patients with type 2 diabetes (refs).

**32c. Hedged self-assertion** — *arguably, it could be argued that, one might say.* These claim a debatable point as if stating it settles it.

**Before:**
> Empagliflozin is arguably the most important advance in cardiovascular therapy of the decade.

**After:**
> Empagliflozin reduced cardiovascular death by 38% in the EMPA-REG OUTCOME trial.

**Key principle:** Delete the self-certifying modifier, or replace it with the quantitative fact or citation that justifies the emphasis ("consistent across all 12 subgroups", "a 34% reduction", "(refs)"). Be especially alert to "genuinely" and "truly", which rarely appear in pre-2023 medical prose. This pattern is the stance-marker counterpart to Pattern 5 (vague attributions): both replace evidence with an assertion that evidence exists.

**Boundary with preserved transitions:** This pattern does NOT target the sentence-initial transitions "Notably," / "Importantly," / "Interestingly," followed by a comma and a substantive point — those remain preserved (see the top of this guide). The target is the modifier fused to the claim it is trying to prove ("genuinely important", "truly transforms", "clearly demonstrates", "it is well established that"). When in doubt: if removing the word loses no information, remove it.

---

### 33. AI Formatting Artifacts (Excessive Boldface and Inline-Header Lists)

**Problem:** AI drafts mechanically bold "key" terms inside running text and convert prose into bulleted lists with bolded lead-in headers followed by colons. Medical manuscripts are written as continuous prose; this formatting is a strong visual tell and is rarely appropriate outside genuinely structured elements (e.g., a structured abstract or a defined inclusion-criteria list that the target journal expects).

**Watch for:** inline `**bolded**` key terms in body text, "**Key takeaway:**"-style lead-ins, and paragraphs broken into bullet points where prose is expected. Also remove stray Markdown emphasis (`*italics*`, `**bold**`) and any emoji.

**Before:**
> **Primary outcome:** The primary outcome was a composite of cardiovascular death and heart failure hospitalization. **Key finding:** Empagliflozin reduced this outcome by **34%**. **Safety:** The drug was well tolerated.

**After:**
> The primary outcome was a composite of cardiovascular death and hospitalization for heart failure. Empagliflozin reduced this outcome by 34% and was well tolerated.

**Key principle:** Convert AI bullet/bold scaffolding back into prose paragraphs unless the journal's format genuinely calls for a list or a defined heading. Preserve legitimate emphasis only where a journal uses it (e.g., gene symbols in italics per nomenclature rules). Also remove excess exclamation marks and ellipses ("...") used for effect; formal medical prose uses neither.

---

### 34. "Plays a [crucial/vital/key/pivotal] Role" and Related Role Templates

**Problem:** "X plays a [vital/crucial/key/central/pivotal/significant] role in Y" is one of the single most recognizable AI constructions (and "vital role" is a confirmed excess phrase in post-ChatGPT medical abstracts). It combines two patterns already in this guide — copula avoidance (Pattern 8) and significance inflation (Pattern 1) — into a fixed template that asserts importance while saying nothing about *what* X actually does. Replace it with the specific mechanism or effect.

**Watch for:** plays a [adjective] role in, has a [adjective] role in, is instrumental in, contributes significantly to, serves a [adjective] function in, is a key driver/player/contributor in

**Before:**
> Inflammation plays a crucial role in the pathogenesis of heart failure, and SGLT2 inhibitors play a pivotal role in mitigating this process.

**After:**
> Inflammation contributes to the pathogenesis of heart failure. SGLT2 inhibitors reduce circulating inflammatory markers, including interleukin-6 and high-sensitivity C-reactive protein.

**Key principle:** State the concrete relationship (what causes what, by how much, through what mechanism). If the mechanism is unknown, say so directly ("the mechanism is not fully understood") rather than papering over it with "plays a role."

---

### 35. Structural Monotony (Uniform Sentence Length and Over-coordination)

**Problem:** Beyond word choice, AI text has a recognizable *shape*. Sentences come out at strikingly uniform length and clause count (low "burstiness"; detection tools flag a coefficient of variation in sentence length below ~0.35), clauses are strung together with "and", and points are forced into parallel triples. Human academic prose varies: a short declarative sentence next to a longer qualified one. This pattern is structural and complements the lexical patterns above.

**Three structural tells to fix:**

**35a. Uniform sentence length / rhythm.** Several consecutive sentences of nearly identical length and a single clause each read as machine-generated. Vary the rhythm: combine two short related sentences, or split an overloaded one. Let at least one sentence be noticeably shorter.

**35b. Phrasal coordination overuse ("and ... and ...").** AI chains coordinate elements where a human would subordinate or split.

**Before:**
> Empagliflozin reduced hospitalization and it reduced cardiovascular death and it improved renal outcomes and it was well tolerated and it had a favorable safety profile.

**After:**
> Empagliflozin reduced hospitalization for heart failure, cardiovascular death, and renal events. It was well tolerated.

**35c. That-clauses as grammatical subjects.** AI overuses "That X is true suggests Y." Recast with a concrete subject.

**Before:**
> That the effect persisted across all subgroups suggests broad applicability.

**After:**
> The effect persisted across all prespecified subgroups, which suggests broad applicability.

**Key principle:** Read the passage aloud. If every sentence has the same length and march, vary it. This is the structural counterpart to Pattern 10 (rule of three) and Pattern 11 (elegant variation): together they target the *shape* of AI prose, not just its vocabulary.

---

### 36. Conversational, SEO, and Reader-Address Tells

**Problem:** LLMs are trained heavily on blog posts and marketing copy, so they inject conversational hooks, direct second-person address, rhetorical questions, and SEO scaffolding. **None of these ever belong in a formal medical manuscript** — which makes them high-precision tells: a single occurrence is a near-certain sign of AI drafting. Remove them entirely and state the content as a third-person declarative.

**Watch for:**

- **Hook openers and listicle scaffolding:** "Here's the kicker", "Here's the thing", "Here's what most people miss", "But here's the deal", "The truth is", "Make no mistake", "Let's be honest", "Let's dive in", "Let's explore", "Let's take a closer look", "Buckle up", "Without further ado", "Spoiler:", "Fun fact:", "Pro tip:", "Bottom line:", "TL;DR", "Stay tuned", "Read on"
- **Time-cliché openers:** "In today's [fast-paced / ever-evolving / competitive / modern] world / landscape / era / age of ...", "Now more than ever"
- **Direct reader address (second person):** "you", "your", "imagine", "picture this", "trust me", "you're not imagining it", "you might be wondering", "rest assured", "Curious what others think"
- **Rhetorical questions as openers:** "So what does this mean for patients?", "But why does this matter?", "Sound familiar?"
- **Reader-funnel framing:** "Whether you're a clinician or a researcher, ...", "Look no further", "the key takeaway is ..."

**Before:**
> So what does this all mean for patients? Here's the kicker: in today's rapidly evolving landscape of diabetes care, you can't afford to ignore these findings. The truth is, they're game-changing.

**After:**
> These findings have direct implications for the management of patients with type 2 diabetes and elevated cardiovascular risk.

**Key principle:** Medical manuscripts address an expert audience in the third person. Remove every hook, rhetorical question, and "you"/"your" address, and convert to a direct declarative statement. (See also Pattern 4 (promotional language) and Pattern 18 (generic conclusions).)

---

### 37. Paste Hygiene: Invisible Characters and Non-standard Typography (ZERO TOLERANCE)

**Problem:** Text pasted from AI chat interfaces carries non-printing and non-ASCII characters that no one types on a keyboard: zero-width spaces, non-breaking spaces, word joiners, and Unicode punctuation used as a softer substitute for plain marks. These are invisible on screen but are flagged by AI-detection tools and corrupt journal submission systems, reference managers, and plain-text pipelines. Because a human writing in a normal editor never produces them, **any occurrence is an artifact and must be removed.**

**Find and replace (this is a cleanup pass, not a judgment call):**

| Character | Code point | Replace with |
|---|---|---|
| Zero-width space | U+200B | delete |
| Zero-width non-joiner / joiner | U+200C / U+200D | delete |
| Word joiner | U+2060 | delete |
| Byte-order mark / ZW no-break space | U+FEFF | delete |
| Soft hyphen | U+00AD | delete |
| Left-to-right / right-to-left marks | U+200E / U+200F | delete |
| Non-breaking space | U+00A0 | normal space |
| Narrow no-break space | U+202F | normal space |
| En / em / thin / hair / figure spaces | U+2002–U+200A, U+2007 | normal space |
| Curly double quotes | U+201C / U+201D | straight `"` (see Pattern 15) |
| Curly single quotes / apostrophe | U+2018 / U+2019 | straight `'` |
| Horizontal ellipsis | U+2026 | three periods `...` (and consider removing; see Pattern 33) |
| Bullet | U+2022 | restructure into prose (see Pattern 33) |
| En dash used as clause punctuation | U+2013 | eliminate like an em dash (see Pattern 13) |

**IMPORTANT exceptions — do NOT remove these legitimate characters:**
- **En dash in a numeric, statistical, or page range** ("95% CI 0.50–0.85", "2010–2024", "pp. 1384–1395") is correct typography. Keep it (or convert to a hyphen only if the journal's plain-text format requires). **Never corrupt a confidence interval or page range.**
- **Scientific symbols:** µ (micro), ±, °, ×, ≥, ≤, ≈, →, Greek letters (α, β, γ, κ, etc.), degree and temperature signs, and italic gene symbols. These are required and must be preserved.

**Verification step (run on the final output):**

```bash
# Flags lines containing invisible/whitespace-substitute or curly-punctuation artifacts.
# Legitimate scientific symbols (µ, ±, Greek, range en dashes) are NOT matched.
# The -CSD flag makes perl decode input as UTF-8 (required, or nothing matches).
perl -CSD -ne 'print "$.: $_" if /[\x{200B}\x{200C}\x{200D}\x{2060}\x{FEFF}\x{00AD}\x{200E}\x{200F}\x{00A0}\x{202F}\x{2002}-\x{200A}\x{201C}\x{201D}\x{2018}\x{2019}\x{2026}\x{2022}]/' yourfile.txt
```

If the command prints any line, fix the flagged characters and run it again until it returns nothing.

**Key principle:** This is a mechanical hygiene pass. Normalize invisible and substitute characters to plain ASCII, but preserve legitimate scientific notation and numeric-range en dashes.

---

## Process

1. Read the input text carefully
2. Identify all instances of the patterns above
3. Rewrite each problematic section
4. Ensure the revised text:
   - Sounds natural when read in an academic context
   - Uses precise, specific language
   - Maintains data integrity (numbers, statistics, findings)
   - Uses simple constructions (is/are/has) where appropriate
   - Avoids promotional or inflated language
5. **MANDATORY FINAL CHECK (em dashes):** Search your output for the em dash character "—". If ANY remain, replace them immediately. Zero em dashes allowed in final output.
6. **MANDATORY FINAL CHECK (invisible/typographic artifacts):** Run the Pattern 37 verification command (or scan manually) for zero-width characters, non-breaking spaces, curly quotes, ellipses, and en dashes used as clause punctuation. Remove or normalize every hit, preserving only legitimate scientific symbols and numeric-range en dashes. The output must be clean ASCII apart from required notation.
7. Present the humanized version

## Output Format

Provide:
1. The rewritten text
2. A brief summary of changes made (optional, if helpful)

---

## Full Example

**Before (AI-sounding):**
> Heart failure represents a pivotal challenge in the evolving landscape of diabetes care, underscoring the critical importance of addressing cardiovascular comorbidities. This groundbreaking study showcases the profound impact of empagliflozin, a pivotal therapeutic option that serves as a cornerstone of modern cardiovascular medicine.
>
> Studies have shown that SGLT2 inhibitors reduce cardiovascular events. Additionally, empagliflozin reduced the risk of hospitalization for heart failure or cardiovascular death by 34%—a remarkable finding—highlighting the cardioprotective effects of this intervention. The number needed to treat of 35 over 3 years underscores the crucial clinical value of this therapeutic approach.
>
> Despite challenges typical of large clinical trials, including the lack of objective cardiac measurements, the trial's strategic design continues to provide valuable insights for the future outlook of heart failure management. The future looks bright for patients with type 2 diabetes as these exciting findings continue to reshape clinical practice.

**After (Humanized):**
> Heart failure is highly prevalent in patients with diabetes, occurring in more than one in five patients with type 2 diabetes aged over 65 years. In patients with type 2 diabetes and high cardiovascular risk, empagliflozin reduced heart failure hospitalization and cardiovascular death when added to standard of care.
>
> In the EMPA-REG OUTCOME trial, empagliflozin reduced the risk of hospitalization for heart failure or cardiovascular death by 34%. The number needed to treat to prevent one event was 35 over 3 years.
>
> The diagnosis of heart failure at baseline was based solely on the report of investigators, with no measures of cardiac function or biomarkers recorded. Empagliflozin reduced heart failure hospitalization and cardiovascular death when added to standard care. The benefit was consistent in patients with and without heart failure at baseline.

**Changes made:**
- Eliminated all em dashes ("—") per Pattern 13 zero-tolerance rule
- Removed significance inflation ("pivotal challenge", "evolving landscape", "groundbreaking", "cornerstone")
- Removed promotional language ("profound impact", "remarkable finding", "exciting findings")
- Removed unsupported vague attributions ("Studies have shown" with no citation) and replaced with specific trial name (note: "Prior studies have shown that..." followed by citations would be preserved)
- Removed superficial -ing phrases ("underscoring", "highlighting")
- Removed copula avoidance ("serves as") in favor of "is"
- Removed AI vocabulary ("Additionally", "crucial", "pivotal")
- Removed formulaic challenges section ("Despite challenges... future outlook")
- Removed generic positive conclusion ("The future looks bright", "continue to reshape")
- Fixed grammar ("The number needed to treat of 35" → "was 35")
- Used simple sentence structures and specific data

---

## Reference

This skill is based on [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), maintained by WikiProject AI Cleanup, adapted for medical and academic writing contexts. The patterns documented there come from observations of thousands of instances of AI-generated text.

Medical paper examples are adapted from:

> Fitchett D, Inzucchi SE, Cannon CP, et al. Empagliflozin Reduced Mortality and Hospitalization for Heart Failure Across the Spectrum of Cardiovascular Risk in the EMPA-REG OUTCOME Trial. *Circulation*. 2019;139(11):1384-1395. doi:10.1161/CIRCULATIONAHA.118.037778

This article is published under CC-BY-4.0 license.
