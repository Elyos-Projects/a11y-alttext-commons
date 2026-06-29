# Competitive & Improvement Analysis — a11y-alttext-commons

> Analyst review of `planning/projects/a11y-alttext-commons/PLAN.md` (v0.2.0, 2026-06-28) and `TASKS.md`.
> Web-researched, cited. Scope: an open commons of high-quality, human-reviewed alt text / image
> descriptions for openly-licensed images (Wikimedia Commons, OpenStax, GLAM) to serve blind/low-vision users.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong for a draft. It correctly internalizes the field's hard-won lessons: a
wrong alt-text is worse than none; merged-and-live (not "descriptions generated") is the success
metric; licensing is a hard gate; and human review is non-optional. Below are correctness findings,
ordered by importance.

**(a) The cardinal requirement — blind/low-vision-user involvement in review — is UNDER-WEIGHTED. (Most important finding.)**
The plan's review gate is a *sighted accuracy reviewer* scoring a 4-dimension rubric (factual fidelity,
completeness, objectivity, functional adequacy). That is necessary but not sufficient. The entire
accessibility field's consensus is that **"functional adequacy" cannot be judged by a sighted person**
— only a screen-reader user can tell you whether a description actually *works* as a substitute for the
image in context. The plan relegates blind-user validation to a single qualitative review *per
collection* at M2 (success-metrics table and `research-003`), i.e. after 500 images are already merged.
This inverts the priority. The American Foundation for the Blind's own framing — "Beyond Alt Text" —
is that descriptions written *for* blind people without blind people in the loop systematically miss
what blind users need (afb.org). **Recommendation:** promote blind/low-vision reviewers from
"validators at M2" to a *standing role in the M0/M1 review gate*; have at least one blind reviewer help
author the style guide (`style-001`); and make "functional adequacy" a dimension that *requires* a
screen-reader-user signal, not a sighted proxy. This is the single biggest correctness gap.

**(b) AI-describes-then-human-reviews is the right architecture, but the hallucination risk is sharper than the plan states. (Second most important finding.)**
The plan does flag hallucination (risk register: "Model hallucinates detail," mitigated by
"describe what is visible" + reviewer tagging). Good. But two structural points are missing:
(1) **The reviewer-against-the-image is itself the weak link for the exact cases that matter most** —
charts, maps, data figures, and dense scenes — because a confident, fluent, *plausible* wrong
description is the hardest thing for a reviewer to catch (it reads correctly). Be My AI / GPT-4V's
documented failure — describing a raincoat's "clouds and raindrops" as "hearts and stars" — is precisely
this class: fluent and wrong, and a blind user "would have little way of noticing unless they knew in
advance what was in an image" (bemyeyes.com; dl.acm.org/10.1145/3663548.3675631; slate.com). The plan's
data-figure sub-procedure (require source data, else describe structure not values) is excellent and
should be **generalized**: for *any* image, the reviewer should verify against ground truth (source
caption, surrounding text, depicts-data), not just "look at the image and agree." (2) The plan should
state explicitly, as a first-class principle, **"no AI-drafted description ships without human
sign-off, ever"** — it is implied but deserves to be a named invariant, because it is the project's core
differentiator versus every auto-alt-text bot.

**(c) Alt-text QUALITY standards — mostly right, with gaps.** The plan correctly distinguishes
decorative vs informative (alt=""), short functional alt vs long description, and the drill-down need
for complex images. Gaps to close in the style guide (`style-001`):
- **Function-over-description / context-dependence is named but not operationalized.** WCAG 1.1.1
  requires a text alternative that serves the *same purpose* as the image in *that context* — the same
  photo needs different alt text in a biology chapter vs a history chapter. The plan's "one image = one
  task" model and upstream-Commons target are in tension with this: a Commons caption is **context-free**
  by construction, while a textbook figure's alt text is **context-bound**. The plan should say
  explicitly that Commons structured captions are *not* a substitute for context-specific alt text, and
  that the same image may legitimately get different descriptions in different host contexts.
- **Text-in-image transcription** (Cooper Hewitt / NCAM both require verbatim transcription of text
  within an image) is only mentioned as an out-of-scope "byproduct." It should be an explicit style-guide
  rule, not deprioritized — untranscribed text in an image is a hard WCAG failure.
- **Tables/charts as structured data, not prose.** NCAM's guidance is that bar/pie charts and tables
  should be rendered as *HTML tables*, not narrative description (wgbh.org). The plan's data-figure
  sub-procedure describes structure-in-prose; it should add "prefer structured/tabular representation
  where the host supports it."

**(d) Image licensing — strong, with one real legal gap.** The hard "unknown = excluded" gate, SPDX
recording, attribution capture, and provenance log are best-in-class. **However, the
"description-is-a-derivative-work" question is left unexamined.** The plan assumes the description
"inherits the host's licensing expectation" (CC-BY/CC0). The unsettled legal reality: a textual
description of an image is most likely an *independent* copyrightable work (the describer's expression),
**not** a derivative of the image — analogous to a book review not being a derivative of the book. This
matters because (i) it means contributors may have a *choice* of license rather than an obligation to
match, and (ii) the "license match, not override" decision should be framed as a *contribution-norm/CLA*
choice (what Commons/the repo wants), not a copyright *requirement* that the description is bound to the
image's license. Conversely, a *very detailed* long description of a *copyrighted* image could raise
derivative-work questions — but the plan only ingests CC/PD images, so this is moot for the source image;
the open question is purely about the description's *own* license. **Recommendation:** add an open
question / `research-002` sub-item: "Is a description a derivative of the image, or an independent work?
Confirm we are choosing a license per host *norms*, not per copyright *compulsion*." (creativecommons.org;
copyright.gov/circs/circ14.pdf).

**(e) Upstream-acceptance risk is correctly identified but may be more severe than rated for Commons.**
The plan rates "host community rejects bulk/automated contributions" as Medium/Medium and gates volume
behind a pre-engagement artifact — correct. But recent Wikimedia governance has hardened sharply against
AI-generated content: English Wikipedia adopted speedy-deletion for suspected LLM articles (Aug 2025) and
a March 2026 policy prohibiting LLM-generated/rewritten article content (en.wikipedia.org/wiki/
Wikipedia:Artificial_intelligence; meta.wikimedia.org RfC; slate.com). While these target *article prose*,
not accessibility captions, the *cultural* climate means an "AI-drafted captions at volume" pitch will
face scrutiny even when human-reviewed. The plan's instinct to lead with **open-textbook PR channels**
(not Commons) for M0 is therefore well-judged and should be stated as a *deliberate* sequencing choice,
not just a priority list. The plan should also emphasize the human-reviewed framing relentlessly in any
talk-page engagement.

**(f) Scope & completeness — minor.** Good non-goals (no SaaS, no scraping around access controls, no
medical interpretation). Missing or thin: (i) **multilingual** is deferred (reasonable) but the plan
should note Commons captions are *inherently* multilingual and CC0 — a translation sibling has a natural
home there; (ii) **measuring beneficiary outcome** beyond "merged/live" — there's no metric for "did a
real screen-reader user reach this image and was served a useful description"; (iii) the **error
taxonomy** is excellent but lacks a "context-mismatch" failure class (a description that is factually
correct but wrong *for the context*), which is the WCAG-1.1.1 failure mode most likely to slip past a
factual-fidelity check.

---

## 2. Competitive landscape (researched, cited)

No incumbent occupies the exact niche — *an open, human-reviewed, context-rich, upstream-contributed
alt-text commons for openly-licensed images*. The space splits into four clusters:

**A. On-demand AI describers (consumer, real-time, unreviewed).**
- **Be My Eyes / Be My AI** (GPT-4V-based). Strength: instant, free, huge blind user base, genuinely
  loved (slate.com). Weakness: explicitly hallucination-prone — vendor itself warns "you may still get
  hallucinations, occasional wrong answers"; documented errors (raincoat pattern) (bemyeyes.com;
  dl.acm.org/10.1145/3663548.3675631). Ephemeral, per-user, nothing persists upstream.
- **Microsoft Seeing AI.** Strength: free, multi-channel (text, scene, product, people), 18 languages,
  iOS+Android (microsoft.com/garage; blogs.microsoft.com/accessibility). Weakness: same as above —
  real-time, device-local, no persistence, no human review, not tied to a content commons.
  *These are complementary, not competitors:* they describe *the user's* world on demand; they do not
  fix the missing alt text *in the source*, which is our deliverable.

**B. Automated alt-text bots (publish unreviewed).**
- **AltBot / @altbot (Fediverse)** and similar Mastodon/WordPress bots. Strength: open-source, zero-cost,
  local-LLM/GDPR-friendly, fills empty alt at posting time (github.com/micr0-dev/AltBot). Weakness: the
  bots *themselves* warn "results may sometimes be factually incorrect... always double-check" — i.e.
  unreviewed AI output shipped to users, the exact anti-pattern our guardrails forbid.

**C. Expert human description services (high quality, closed/paid, not a commons).**
- **Scribely.** Strength: professional, standards-compliant alt + extended descriptions, museum/gallery
  clients, photo-metadata-standards advocacy (scribely.com). Weakness: commercial, per-client, not open,
  not a reusable public dataset.
- **CIDI (Georgia Tech, ex-AMAC/CATEA).** Strength: subject-matter-expert describers, braille/etext
  pipeline, research pedigree (cidi.gatech.edu/services/etext/descriptions). Weakness: fee-for-service,
  delivered as private spreadsheets to clients, not an open commons.

**D. Standards, tooling & guidelines (the knowledge base — allies, not competitors).**
- **DIAGRAM Center / Poet (Benetech).** Open-source, crowd-sourced image-description training tool for
  DAISY/EPUB; teaches describing complex images (flow charts, Venn diagrams) per extended NCAM guidelines
  (diagramcenter.org/poet.html; poet.diagramcenter.org). The closest *philosophical* sibling — but it's a
  *training/tooling* tool over EPUB books, not an upstream-contribution pipeline for open image
  collections.
- **WGBH/GBH NCAM.** The authoritative STEM image-description guidelines: data-first, drill-down
  structure, tables-as-tables (wgbh.org/foundation/services/ncam). Our style guide should *cite and build
  on* these, not reinvent them.
- **Smithsonian / Cooper Hewitt.** Detailed museum image-description guidelines (no "image of...",
  transcribe embedded text, neutral age/gender language) (cooperhewitt.org). Directly reusable.
- **Wikimedia Commons / SDC.** The largest target *and* a partial "competitor": Structured Data captions
  (CC0, multilingual) and `depicts` (P180, 27.2M+ files as of Jan 2026), plus community efforts (Image
  Description Week/Month, the iOS app "Alt Text Experiment," 2021 Community Wishlist) — yet **<10% of
  Wikipedia images have alt text and <half are captioned** (commons.wikimedia.org/wiki/Commons:SDC;
  meta.wikimedia.org Image_Description_Week; mediawiki.org Alt_Text_Experiment). The gap is real and
  acknowledged *by Wikimedia itself*.
- **OpenStax.** Targets WCAG 2.x AA, "all images developed with detailed explanatory text," partners with
  Level Access (openstax.org/accessibility-statement). Strong on *their own* new content — but the long
  tail of community/adapted OER and older figures is exactly our opening.

---

## 3. Gaps we can fill

1. **Persistent, upstream, reusable** — bots/apps (Cluster A/B) are ephemeral or per-user; we fix the
   source so *every* downstream reader benefits once.
2. **Human-reviewed + blind-user-validated** — unlike auto-bots (B) which ship unreviewed AI, and unlike
   apps (A) which warn-and-shrug about hallucination.
3. **Open & free as a public good** — unlike Scribely/CIDI (C) which are paid and closed; we produce an
   open dataset and open contributions.
4. **Context-rich for *education*** — leaning on NCAM/DIAGRAM rigor for charts/maps/data figures, where
   the consumer apps are weakest and where OER's missing descriptions hurt most.
5. **A described-image *dataset* with provenance** — a byproduct nobody else publishes openly: image →
   reviewed description → rubric scores → error tags → license/attribution. Valuable for research and for
   training/evaluating future describer models.
6. **The "long tail" of open collections** — OpenStax covers its own new figures; we cover the adapted
   OER, GLAM, and Commons backlog that no funded team owns.

---

## 4. Differentiators to win

- **Blind/low-vision users *in the review loop*, not just as end-beneficiaries.** If we make this the
  signature, we beat both the unreviewed bots and the sighted-only expert services on the one axis that
  actually defines quality.
- **"No AI ships unreviewed" as a public, enforced invariant** — the credibility wedge for Wikimedia/OER
  community trust in an era of AI-content backlash.
- **Upstream-first, additive-only, never-overwrite** — we improve the commons in place; we don't build a
  rival silo or clobber existing human work.
- **Education-grade complex-image rigor** (NCAM/DIAGRAM data-figure procedure) — the apps' weakest spot.
- **Radical provenance** — every description carries license, attribution, model, reviewer, rubric score,
  error tags. Auditable trust.
- **Outcome accountability** — "merged and live for screen-reader users," not "descriptions generated."

---

## 5. Claude API leverage — and where Claude must NOT decide

**Where Claude (vision) adds the most leverage (always as a *draft for human review*):**
1. **First-draft alt text + long description** from the image, *conditioned on the style guide and the
   image's context* (surrounding textbook text, caption, `depicts` data). Claude's vision + long-context
   is well-suited to "draft a WCAG-1.1.1 functional alt text AND a drill-down long description for this
   chart, given this source caption."
2. **Context & ground-truth gathering** — pull the figure's surrounding text/caption/source-data so the
   *reviewer* (and the draft) can be checked against a source, directly powering the data-figure
   sub-procedure. Claude assembles the evidence packet; the human verifies.
3. **Long-description structuring** — convert a dense diagram into NCAM-style drill-down: summary →
   detail → data, and propose tabular representation for charts. Structuring is where LLMs are strong and
   the failure cost (if reviewed) is low.
4. **Pre-flight triage/flagging** — decorative-vs-informative, simple-vs-complex, and *sensitivity*
   flagging (people/medical/traumatic) to route to the right reviewer. (Flag liberally; fail-open routes
   to human, never ships unseen.)
5. **Self-critique / hallucination-surfacing** — have Claude label each asserted detail with "visible in
   image" vs "inferred," and flag low-confidence claims, giving the reviewer a verification checklist
   rather than fluent prose to rubber-stamp.
6. **Style-guide / error-taxonomy authoring assistance** and dedup heuristics support.

**Where Claude must NOT be the decider (hard human gates):**
- **Final alt text** — never merged on Claude's say-so. Human accuracy review (rubric + error tags) is
  mandatory, **and "functional adequacy" needs a blind/low-vision-user signal**, not a sighted proxy.
- **Hallucination = the core risk** — because Claude (like GPT-4V in Be My AI) can be *fluently, plausibly
  wrong* in ways a blind user cannot detect. AI never ships unreviewed; specific data values must be
  *sourced*, not model-asserted.
- **License/PD determination** — machine *assists*, human *verifies*. "Unknown = excluded" is a human
  judgment, not a model classification.
- **Sensitivity clearance** — every people/medical image gets human eyes regardless of model confidence;
  the model's flag never *clears*, only *routes*.
- **Identity / private-attribute inference** — Claude must never name individuals or infer
  health/religion/etc.; observational language only, enforced by the human reviewer.
- **Context/function fit** — whether a description serves the image's *purpose in this specific context*
  is a human (ideally blind-user) judgment WCAG 1.1.1 demands.

---

## 6. Ten concrete optimizations

1. **Put a blind/low-vision reviewer in the M0/M1 gate** (not just M2 validation). Co-author the style
   guide with at least one blind reviewer. *(Addresses finding 1a — highest priority.)*
2. **Name "no AI-drafted description ships without human sign-off" as a first-class, public invariant** in
   PLAN, README, and every upstream talk-page pitch — it's the trust wedge against the AI-content backlash.
3. **Generalize the data-figure "verify against a source" rule to ALL images.** Require the draft to carry
   a per-claim "visible vs inferred" annotation so reviewers verify rather than rubber-stamp fluent prose.
4. **Add a "context-mismatch" error class** and make the style guide state that Commons captions are
   context-free and NOT a substitute for context-bound textbook alt text (same image → different alt text
   by context). *(WCAG 1.1.1 / finding 1c.)*
5. **Lead M0 with an open-textbook PR channel, explicitly, given the 2025–26 Wikimedia AI-content
   hardening** — and keep Commons as channel #2 behind heavy human-reviewed framing + pre-engagement.
6. **Resolve the derivative-work / license question** in `research-002`: is the description an independent
   work (likely) → we choose a license per host *norm*, not copyright *compulsion*. Document the answer.
7. **Adopt, don't reinvent, NCAM + DIAGRAM/Poet + Cooper Hewitt guidelines** by reference in the style
   guide; add their rules we currently underweight: verbatim text-in-image transcription, and
   tables/charts as structured tables not prose.
8. **Publish the described-image dataset as a first-class deliverable** (image ref → reviewed description
   → rubric scores → error tags → license/provenance) under CC0/CC-BY — a unique open asset for research
   and model evaluation.
9. **Add a beneficiary-reach metric** beyond "merged/live": e.g. periodic confirmation that a
   screen-reader user reaching a described item is well served (sample-based), closing the gap between
   "merged" and "actually helps."
10. **Build an MCP/describer tool that bundles the style guide + ground-truth fetch + per-claim
    visible/inferred annotation**, so any donated-lane agent drafts in-house style consistently and hands
    the reviewer a verification checklist, not just prose. *(Also de-risks reviewer bottleneck, the plan's
    own High-likelihood risk.)*

---

## 7. Parallel & perpendicular spin-offs

- **open-pictograms** — alt text and pictogram/AAC symbol descriptions overlap; a shared "describe a
  symbol's meaning, neutrally, in context" style and reviewer pool. Pictograms ↔ alt text are siblings in
  the "make visual meaning textual" family.
- **caption-commons** — closed-caption/transcript commons for openly-licensed AV; same upstream-first,
  human-reviewed, license-gated pipeline, different media. Strong code reuse (claim/lock, provenance,
  additive-only adapters).
- **easy-read-plus** — plain-language / cognitive-accessibility versions of the *same* descriptions
  (cognitive guidance per the guardrails). A long description has an "easy-read" sibling output; one image,
  multiple accessibility renderings.
- **loc-public-domain-engine** — a high-volume *source* of PD images (Library of Congress) feeding the
  pipeline; perpendicular supply-side partner with clean licensing.
- **open-childrens-books** — illustrated OER for kids needs especially careful, age-appropriate, neutral
  descriptions; a focused vertical that exercises the sensitivity gate hard.
- **alt-text API / dataset** — the described-image dataset (opt. #8) exposed as an open API/benchmark for
  evaluating describer models — turns a byproduct into a research public good (and a way to measure
  whether AI describers are improving against *human-reviewed* ground truth).
- **MCP server** — "alt-text-commons" MCP exposing: style guide, ground-truth/context fetch, license
  verifier, dedup (pHash) lookup, and a draft-with-visible/inferred-annotations tool. Lets any agent
  participate in the donated lane with house-standard quality. (See opt. #10.)

---

## 8. Open questions

1. **Blind-reviewer capacity & compensation** — recruiting/fairly paying blind/low-vision reviewers for a
   *standing* gate role is the project's true scarce resource. Where do they come from, and is it funded?
2. **Derivative-work / license compulsion** — is a description legally independent of the image (→ we
   *choose* a license per host norm) or treated as derivative? (Finding 1d.)
3. **Wikimedia's real posture on human-reviewed AI-drafted captions at volume**, given the 2025–26
   article-content AI restrictions — does the captions/accessibility carve-out hold? (Engage before scaling.)
4. **Context vs. context-free** — can a single per-image task model honestly serve both Commons
   (context-free captions) and textbook figures (context-bound alt text)? Or do these need distinct task
   types?
5. **Credential bar** — what training qualifies a sensitivity/people reviewer; what credential qualifies a
   medical/diagnostic expert (already an open question in PLAN — keep it).
6. **How to catch fluent-but-wrong descriptions at scale** — what reviewer aids (visible/inferred
   annotation, source-data attachment, double-review sampling) actually move the *hallucinated-detail* and
   *wrong-value* error rates toward zero?
7. **Reviewer bottleneck economics** — at "one image = one task," does review capacity (esp. blind-user
   review) cap throughput far below describe capacity, and how is fan-out throttled to it?
8. **Multilingual** — Commons captions are inherently multilingual+CC0; is translation this project or the
   `caption-commons`/`easy-read-plus` sibling?

---

### Sources
- Wikimedia Commons SDC / captions / depicts: https://commons.wikimedia.org/wiki/Commons:SDC ; https://meta.wikimedia.org/wiki/Image_Description_Week/en ; https://www.mediawiki.org/wiki/Special:MyLanguage/Wikimedia_Apps/iOS_Suggested_edits_project/Alt_Text_Experiment ; https://meta.wikimedia.org/wiki/Community_Wishlist_Survey_2021/Reading/Alt-Texts_and_Image_Descriptions
- Wikimedia AI-content governance: https://commons.wikimedia.org/wiki/Commons:AI-generated_media ; https://en.wikipedia.org/wiki/Wikipedia:Artificial_intelligence ; https://meta.wikimedia.org/wiki/Requests_for_comment/Artificial_intelligence_policy ; https://slate.com/technology/2026/04/wikipedia-ai-chatbot-ban.html
- DIAGRAM Center / Poet: http://diagramcenter.org/poet.html ; https://poet.diagramcenter.org/about.html ; https://benetech.org/blog/benetech-releases-image-description-tool-to-improve-accessibility-of-graphical-content-for-students-with-print-disabilities/
- NCAM / GBH STEM image description: https://www.wgbh.org/foundation/services/ncam/tools-resources/effective-practices-for-description-of-science-content-guidelines-for-describing-stem-images ; https://www.wgbh.org/foundation/services/ncam/image-description-resources
- Cooper Hewitt / Smithsonian: https://www.cooperhewitt.org/cooper-hewitt-guidelines-for-image-description/ ; https://access.si.edu/museum-professionals
- Be My Eyes / Be My AI (hallucination): https://www.bemyeyes.com/bme-ai/ ; https://www.bemyeyes.com/news/announcing-be-my-ai-soon-available-for-hundreds-of-thousands-of-be-my-eyes-users/ ; https://dl.acm.org/doi/fullHtml/10.1145/3663548.3675631 ; https://afb.org/blog/entry/alt-text-age-ai ; https://slate.com/technology/2023/10/ai-image-tools-blind-low-vision.html
- Microsoft Seeing AI: https://www.microsoft.com/en-us/garage/wall-of-fame/seeing-ai/ ; https://blogs.microsoft.com/accessibility/seeing-ai-app-launches-on-android-including-new-and-updated-features-and-new-languages/
- AltBot (unreviewed auto alt-text): https://github.com/micr0-dev/AltBot
- Scribely: https://www.scribely.com/image-accessibility ; https://www.scribely.com/about
- CIDI (Georgia Tech): https://cidi.gatech.edu/services/etext/descriptions
- OpenStax accessibility: https://openstax.org/accessibility-statement/ ; https://openstax.org/blog/accessibility-at-openstax
- Copyright / derivative works: https://www.copyright.gov/circs/circ14.pdf ; https://wiki.creativecommons.org/wiki/Recommended_practices_for_attribution
