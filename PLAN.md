# PLAN — a11y-alttext-commons

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## Executive summary

Millions of openly-licensed images in open textbooks, Wikimedia Commons, and GLAM
(galleries, libraries, archives, museums) open collections ship with no alternative text or
long description. For blind and low-vision learners using screen readers, an undescribed image
is a hole in the page — and in open education, that hole sits exactly where a diagram, map, or
chart carries the lesson.

`a11y-alttext-commons` turns this accessibility gap into a massively parallel stream of small,
reviewable good deeds: **one image = one task**. An AI session drafts WCAG-conformant alt-text
(short, functional) and, where the image is complex, a long description; a human reviewer checks
accuracy; the result is contributed **upstream** under the host source's open license (Commons
structured-data captions/`P2096`-style fields, open-textbook repository PRs). Success is not
"descriptions generated" — it is **described images merged into the upstream source and visible
to real screen-reader users**.

The work is **low–medium risk**. The two real hazards are (1) description *accuracy* — a wrong
alt-text is worse than none, especially for charts, maps, and data figures — and (2) *sensitive
imagery* (people, medical, historical/traumatic, identifiable individuals). Both are handled by
a mandatory human accuracy-review gate and a sensitivity-flagging pass, not by trusting the model.
The hard guardrail is **licensing**: only CC-licensed or public-domain sources are ever ingested
or described, and license + attribution + provenance are recorded per image.

This document is honest about what is not yet in place: **no upstream partner org is secured**,
and the Wikimedia / open-textbook contribution pathways must be validated against current
community norms and bot policies before scaling. M0 is a thin, manual, end-to-end slice to prove
the pipeline and earn the right to scale.

## Problem & beneficiaries

**Who is helped (primary beneficiaries):**

- **Blind and low-vision learners** who rely on screen readers (JAWS, NVDA, VoiceOver, TalkBack)
  and Braille displays. Undescribed images in open courseware mean missed content and unequal
  access to free education.
- **Educators and accessibility teams** who adopt open textbooks and are legally and ethically
  obligated to provide accessible materials but lack capacity to describe thousands of figures.

**Secondary beneficiaries:**

- The **open-knowledge commons** itself — Wikimedia Commons, OpenStax / open-textbook projects,
  and GLAM open collections — whose reusable assets become more usable for everyone (better
  search indexing, captions, downstream reuse).

**The verified need.** The underlying need is well documented and not in dispute: WCAG 2.x
Success Criterion 1.1.1 (Non-text Content) requires text alternatives; surveys of open
educational resources and of Commons repeatedly find that a large majority of images lack
meaningful alt-text or structured captions. The proposal marks verified need = yes on this basis.

**The partner gap (honest).** A *documented gap* is not the same as a *secured delivery channel*.
No specific partner organisation (a textbook publisher, a GLAM institution, or a WMF-affiliated
group) has yet agreed to receive, review, and merge our contributions, and no batch of target
images has been formally prioritised with a beneficiary. **Partner org: TO BE SECURED.** Until at
least one upstream channel is confirmed (a repo that will accept our PRs, or a Commons workflow
that community norms welcome), task-level `verifiedNeed` is set conservatively to `false` for
delivery-dependent tasks. M0 includes securing this channel as an exit criterion.

## Goals and non-goals

**Goals**

- Produce accurate, WCAG-conformant alt-text and long descriptions for openly-licensed images.
- Contribute every accepted description **upstream** under the source's open license, with
  attribution and provenance recorded.
- Make the work safely parallelisable: a clear style guide, a self-contained per-image task unit,
  and a human accuracy-review gate that scales.
- Measurably increase the share of *described* images in concrete target collections.

**Non-goals**

- **Not** describing, ingesting, scraping, or even caching non-open / unclear-license media.
- **Not** building a hosted SaaS, public image-upload service, or a general "describe any image" API.
- **Not** OCR/transcription of text-in-images as a primary deliverable (may be a byproduct only).
- **Not** editorial or content moderation of the host collections; we add descriptions, we do not
  curate, reclassify, or remove images.
- **Not** medical/legal/diagnostic interpretation of imagery (e.g. "this X-ray shows…"). Where an
  image's *correct* description requires credentialed expertise, the task escalates to high risk
  and out of the default flow.
- **Not** generating descriptions for images of identifiable private individuals where privacy or
  consent is unclear.

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Vanity counts (e.g. "images processed") are explicitly *not*
the headline metric; **merged-and-live** is.

| Metric | Baseline | Target (first 2 quarters post-M0) |
|---|---|---|
| Described images **merged upstream and live** | 0 | ≥ 500 across ≥ 2 collections |
| **Acceptance rate** of submitted descriptions (merged ÷ submitted) | n/a | ≥ 80% (signals quality, not spam) |
| **Reviewer-corrected rate** (descriptions edited before merge) | n/a | ≤ 25%, trending down |
| **Share of described images** in a chosen target collection | measured at M0 | +X percentage points (set once baseline known) |
| **Confirmed-accurate sensitive-image descriptions** (people/maps/medical) | 0 | 100% expert/specialist-reviewed before merge |
| **License-compliance violations** (non-open media touched) | 0 | **0** (hard gate; any violation is a stop-the-line incident) |
| Beneficiary validation (screen-reader user confirms a described batch is usable) | none | ≥ 1 qualitative review per target collection |

Note: percentage-point targets are deferred until M0 measures a real baseline in a chosen
collection — inventing one now would be dishonest.

## Scope

**In scope**

- Alt-text (short, functional) and long descriptions for **openly-licensed** still images.
- Per-image license + attribution + provenance capture.
- A duplicate / low-confidence / sensitivity flagging pass routing items to human review.
- Upstream contribution adapters: Wikimedia Commons structured-data captions/descriptions, and
  open-textbook repository pull requests.
- A published, versioned alt-text style guide.

**Out of scope**

- Non-open or unknown-license images (hard exclusion).
- Video/audio description, image generation, or image editing.
- Hosting, redistributing, or republishing the source images themselves.
- Describing images that require credentialed expertise to describe *correctly* (medical imaging,
  legal/forensic) — these are out of the default flow and gated as high risk.
- Bulk automated submission that violates a host's bot/automation policy.
- Translation of descriptions into other languages (valuable, but a future project/extension).

## Solution approach & architecture

This is a **content/data pipeline** project with supporting **adapter code**, run in the **donated
lane**: a human runs their own agent interactively to draft descriptions; Elyos prepares the
per-image task workspace and opens the upstream PR. The CLI never runs an agent headless.

**Pipeline (per image)**

1. **Select & verify source** — pull an image record from an approved, openly-licensed collection.
   Confirm license is CC-* or public domain; capture canonical URL, license, author/attribution,
   and source identifier. *If license is not clearly open → drop, do not ingest.*
2. **Pre-flight flagging** — automated checks classify the image: decorative vs informative;
   complexity (simple photo vs chart/map/diagram → needs long description); sensitivity
   (people / identifiable individuals / medical / potentially traumatic) → routes to specialist
   review; duplicate/near-duplicate detection to avoid redundant work.
3. **Draft** — agent produces alt-text per the style guide, plus a long description if complex.
   Output is a structured record, never free-floating text.
4. **Human accuracy review** — a reviewer verifies the description is correct, objective, and
   functional. Charts/maps/data figures and all sensitive images require the appropriate reviewer.
5. **Upstream contribution** — an adapter formats the accepted description for the host and submits
   it (Commons structured caption/description edit; or a PR to the textbook repo). Attribution and
   provenance travel with it.
6. **Track to live** — record merge status; a deed is "done" only when merged and visible upstream.

**Components**

- `style-guide/` — the published alt-text style guide (length limits, objectivity rules,
  decorative-vs-informative, charts/maps/data-figure conventions, sensitive-imagery handling).
- `pipeline/` — selection, license verification, flagging (complexity/sensitivity/duplicate),
  and the per-image task-record schema.
- `adapters/` (Elyos-conformant: all source/host-specific logic lives here) —
  - `adapters/commons/` — Wikimedia Commons structured-data caption/description submission via the
    MediaWiki/Wikibase API, honouring community + bot policy.
  - `adapters/textbook-pr/` — open-textbook repo PR generation (locate the figure's source,
    insert/patch the alt/description field, open a DCO-signed PR).
- `records/` — per-image description records (the project's data artifact / dataset deliverable).

**Tech stack & conventions.** TypeScript, ESM, pnpm workspaces. Adapter code MIT-licensed.
Generated descriptions licensed to match the host (CC-BY / CC-BY-SA / CC0 / PD as the source
requires — see Data, licensing & compliance). DCO sign-off (`git commit -s`) on all code and PRs.

**Per-image task record (data model, draft)**

```
imageId            stable id within source collection
sourceUrl          canonical URL of the image
sourceCollection   e.g. "wikimedia-commons" | "openstax-bio-2e" | "<glam>"
license            SPDX-style id, e.g. "CC-BY-4.0" | "CC0-1.0" | "PD"
attribution        required attribution string (author/creator + source)
imageClass         decorative | informative-simple | informative-complex
sensitivity        none | people | identifiable-person | medical | sensitive-historical
altText            short functional alt-text (or "" if decorative)
longDescription    long description for complex images (nullable)
confidence         model self-reported / heuristic confidence
reviewStatus       drafted | needs-specialist | reviewed | rejected
upstreamRef        PR URL or Commons edit/revision id once submitted
mergeStatus        submitted | merged | declined
provenance         tool, model, date, reviewer
```

**Key decisions**

- *Upstream-first, no parallel store.* We contribute into the host; we do not stand up a competing
  description database. The `records/` artifact is an audit/provenance log, not a re-publication.
- *License match, not license override.* The description inherits the host's licensing expectation
  so it can be legally merged there; we do not impose an Elyos license on upstream content.
- *Human-in-the-loop is non-optional.* No description merges without human accuracy review; this is
  the difference between "helpful" and "confidently wrong."

## Data, licensing & compliance

**This is the critical, conservative section.**

**Allowed sources (only these classes).**

- Wikimedia Commons files under CC-BY, CC-BY-SA, CC0, or Public Domain.
- Open textbooks (e.g. OpenStax-style and similar) under CC-BY / CC-BY-SA, where the repo accepts
  contributions.
- GLAM open-access collections explicitly released under CC0 / Public Domain / CC-BY(-SA).

**Hard license gate.** Before *any* ingestion or description:

1. The image's license must be machine-or-human verified as CC-* or PD. **Unknown, "all rights
   reserved," "non-commercial-only where it conflicts with the host," or unclear → excluded.** No
   ingestion, no caching, no description.
2. The license id and the required **attribution** string are recorded with the description.
3. Provenance is recorded: source URL, collection, retrieval date, tool/model used, reviewer.

**Output licensing.** Generated descriptions are contributed under a license **compatible with the
host** so they can be merged: typically CC-BY-4.0 or CC0-1.0 for Commons captions, and the
textbook's existing CC license for textbook PRs. Adapter/pipeline **code** is MIT.

**Privacy / PII stance.** Images may depict people. The pipeline:

- Flags images containing people, and especially *identifiable individuals*, to a specialist gate.
- Does **not** attempt to **identify, name, or infer** private attributes (identity, health,
  religion, sexuality, etc.) of individuals — descriptions stay observational and neutral
  ("a person wearing a lab coat," not speculation about who they are).
- Excludes images where privacy/consent is unclear, even if the *file* license is open (an open
  license on a photo does not grant a right to make claims about the depicted person).

**Sensitive imagery.** Medical, traumatic/historical, violent, or otherwise sensitive images route
to a specialist reviewer; medical/diagnostic interpretation is out of scope (high-risk).

**Robots / automation policy.** Selection and contribution honour each host's API terms, rate
limits, and bot/automation policies. We do not scrape around access controls.

## Quality, review & risk gates

**Risk tier: low–medium** (per proposal and the good-deed definition).

- **low** — simple, non-sensitive informative or decorative images; standard reviewer.
- **medium** — charts, maps, diagrams, data figures (accuracy-critical) and any image containing
  people → reviewer with the relevant skill / sensitivity training.
- **high (escalation, out of default flow)** — images whose *correct* description needs
  credentialed expertise (medical imaging, legal/forensic). These require credentialed expert
  sign-off before merge, or are declined.

**Required review before a deed is "done":**

1. **License check passed** (recorded license + attribution + provenance).
2. **Accuracy review** by a human — mandatory for every description; specialist for medium/high.
3. **Style-guide conformance** (length, objectivity, decorative-vs-informative correct).
4. **Sensitivity clearance** for flagged images.

**Definition of Shipped (project-level).** A reviewed description is **merged into the upstream
source** (Commons structured data, or the open-textbook repo) and is **live** for screen-reader
users, with license/attribution/provenance recorded. Generated-but-not-merged ≠ shipped.

## Roadmap & milestones

Phased; each milestone has measurable exit criteria. M0 is a deliberately thin, manual,
end-to-end slice — it must prove a description can travel from source to *merged upstream* before
anything scales.

**M0 — Foundation & cold-start (prove one end-to-end deed).**
Goal: publish the style guide, secure at least one upstream channel, and merge a tiny hand-curated
batch end-to-end.
Exit criteria:
- Alt-text style guide v1 published.
- ≥ 1 upstream channel **confirmed** (a repo that will take our PRs, or a validated Commons
  workflow) — closes the partner gap for at least one collection.
- License-verification + provenance record format finalised and applied to a sample.
- **≥ 10 descriptions merged upstream and live**, each with recorded license + attribution.
- Baseline "share of described images" measured for one chosen target collection.

**M1 — Adapters & flagging (make it repeatable).**
Goal: turn the manual slice into a repeatable pipeline.
Exit criteria:
- Commons adapter and/or textbook-PR adapter working and policy-compliant.
- Pre-flight flagging (complexity, sensitivity, duplicate) operational and routing to review.
- Reviewer workflow + checklist documented; ≥ 2 reviewers onboarded.
- ≥ 100 descriptions merged upstream; acceptance rate measured.

**M2 — Controlled scale (one collection, deep).**
Goal: meaningfully move the described-share metric in a single collection.
Exit criteria:
- ≥ 500 descriptions merged upstream across ≥ 2 collections.
- Acceptance rate ≥ 80%; reviewer-corrected rate trending ≤ 25%.
- ≥ 1 beneficiary (screen-reader user) validation of a described batch.
- Zero license-compliance violations.

**M3 — Broaden & sustain (more collections, durable ops).**
Goal: add collections and a maintenance model.
Exit criteria:
- ≥ 2 additional collections onboarded with confirmed channels.
- Outcome-tracking dashboard (merged/live counts per collection) maintained.
- Documented maintainer + reviewer rotation; sustainability plan in effect.

## Work breakdown

The itemised, schema-mapped backlog lives in **`TASKS.md`** (same directory). It is organised by the
milestones above, with one task per work unit, stable IDs (`a11y-alttext-commons-<area>-NNN`), a
size/risk/reviewer table per milestone, acceptance criteria for the key tasks, and a complete
example Task JSON for the first M0 task. The defining characteristic of the production backlog is
fan-out: at scale, **one image = one task**, drawn from a milestone-scoped batch.

## Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — owns the style guide, pipeline, adapters, and overall quality bar.
- **Reviewers / rotation:** a pool of accuracy reviewers; at least one with data-figure (chart/map)
  skill and one trained for sensitive/people imagery. Rotation documented in M1.
- **Steward (last-mile owner):** owns the relationship with each upstream channel and shepherds
  contributions to *merged/live* — the person accountable for "delivered, not merged."
- **Partner / requestor:** TO BE SECURED — the upstream collection(s) and any beneficiary
  organisation that prioritises target batches.
- **Expert reviewers (risk tiers):** specialist reviewer for medium (data figures, people);
  credentialed expert required for any high-risk escalation (medical/legal) before merge.
- **Beneficiary validators:** screen-reader users who confirm described batches are actually usable.

## Dependencies & integrations

- **Wikimedia Commons / MediaWiki + Wikibase API** — structured-data captions/descriptions;
  subject to community norms, bot policy, and rate limits.
- **Open-textbook repositories** (OpenStax-style and similar) — must accept external PRs.
- **GLAM open-access collections / IIIF endpoints** — source images and license metadata.
- **License metadata sources** — SPDX identifiers; CC license deeds; host-provided license fields.
- **Elyos platform pieces** — `packages/cli` (task workspace + PR prep, donated lane), the Task
  schema (`packages/schema`), and `adapters/` for all host-specific code. The CLI never runs an
  agent headless and never authenticates a coding agent.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Inaccurate description (esp. charts/maps/data) merged upstream | Medium | High | Mandatory human accuracy review; specialist reviewer for data figures; complexity flagging routes hard cases | Maintainer / Reviewers |
| Non-open / unclear-license image ingested or described | Low | High | Hard pre-ingest license gate; record license+attribution; "unknown = excluded"; stop-the-line on any violation | Steward |
| Sensitive imagery (people, medical, traumatic) mishandled | Medium | High | Sensitivity flagging; specialist gate; no identity/private-attribute inference; high-risk escalation path | Reviewers / Expert |
| No upstream partner/channel secured → nothing ships | Medium | High | M0 exit criterion is a confirmed channel; `verifiedNeed=false` until secured; do not scale before delivery proven | Maintainer / Steward |
| Host community rejects bulk/automated contributions | Medium | Medium | Honour bot/automation policy; start small; engage community early; human-reviewed, not spam | Steward |
| Privacy harm from describing identifiable individuals | Low | High | Exclude unclear-consent images even if file is openly licensed; neutral observational language only | Reviewers |
| Reviewer capacity becomes the bottleneck | High | Medium | Reviewer rotation; batch sizing; flagging to triage easy vs hard; only scale fan-out to review capacity | Maintainer |
| Duplicate / redundant work across sessions | Medium | Low | Duplicate/near-duplicate detection; batch claim/locking; milestone-scoped batches | Maintainer |
| Model hallucinates detail not present in image | Medium | High | Style guide forbids inference; "describe what is visible"; reviewer verifies against image | Reviewers |

## Security & privacy

- **Threat surface.** Mostly content-integrity and privacy rather than classic security: wrong or
  fabricated descriptions, license violations, and privacy harms to depicted people. Adapters also
  touch external write APIs (Commons edits, repo PRs).
- **Secrets handling.** Any host API tokens (MediaWiki, GitHub) are supplied via the human's own
  environment, never written into logs, receipts, records, or committed files (per CLAUDE.md). The
  donated lane means the human authenticates with their own account; Elyos does not store agent
  credentials.
- **PII / privacy.** No identification or private-attribute inference about individuals; exclude
  unclear-consent imagery; neutral observational descriptions only.
- **Abuse / misuse prevention.** No mass unreviewed automated edits; honour rate limits and bot
  policy; every merge is human-reviewed. Refuse and flag (per Elyos guardrails) any request to
  describe non-open media, to target/deceive, or to make high-stakes claims without expert review.

## Sustainability & maintenance

- **Post-delivery ownership.** The descriptions live upstream in the host collections — they are
  maintained by those communities once merged, which is the durable, non-Elyos-dependent outcome.
- **Project maintenance.** The maintainer keeps the style guide and adapters current with host API
  and policy changes; reviewer rotation keeps review capacity alive.
- **Outcome tracking.** A lightweight dashboard tracks merged/live counts and described-share per
  collection over time, so impact is visible after the active push ends.
- **Wind-down.** If a channel closes, in-flight items are either completed or cleanly withdrawn; the
  provenance log records final disposition.

## Open questions

- Which upstream channel is secured first — a specific open textbook repo, Commons, or a GLAM
  partner? (Blocks M0 exit.)
- What output license does each chosen Commons workflow actually require for structured captions
  (CC0 vs CC-BY) — confirm before submitting at any volume.
- What is the current Wikimedia community/bot policy stance on AI-drafted, human-reviewed captions
  at modest volume? Engage before scaling.
- How is reviewer credentialing handled for medium-risk data figures and for sensitive imagery?
- What is the right batch size to keep reviewer capacity from becoming the bottleneck?
- Should descriptions be translated (multilingual a11y) — and if so, as this project or a sibling?

## References

- `C:\code\elyos\CLAUDE.md` — Elyos work rules, lanes, quality bar, refusal guardrails.
- `C:\code\elyos\docs\good-deed-definition.md` — good-deed criteria and risk tiers.
- `C:\code\elyos\packages\schema\src\schemas.ts` — Task JSON schema.
- `C:\code\elyos\governance\proposals\a11y-alttext-commons.md` — originating proposal.
- WCAG 2.x Success Criterion 1.1.1 (Non-text Content) — text-alternative requirement.
- Creative Commons license suite (CC-BY, CC-BY-SA, CC0) and Public Domain.
- Wikimedia Commons structured data (captions / depicts) and MediaWiki/Wikibase API + bot policy.
