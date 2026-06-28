# PLAN — a11y-alttext-commons

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

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
| **Mean accuracy-rubric score** (per the 4-dimension rubric below) | n/a | ≥ 3.5 / 4 mean; no description merged below 3/4 on any dimension |
| **Reviewer-corrected rate** (descriptions edited before merge) | n/a | ≤ 25%, trending down |
| **Inter-reviewer agreement** (on a sampled double-reviewed subset) | n/a | ≥ 0.7 (e.g. Cohen's κ or % exact-match on the rubric); recalibrate the rubric if below |
| **Error-taxonomy mix** (share of corrections by failure class) | n/a | tracked; *hallucinated detail* and *wrong value* trending toward 0 |
| **Share of described images** in a chosen target collection | measured at M0 | +X percentage points (set once baseline known) |
| **Confirmed-accurate sensitive-image descriptions** (people/maps/medical) | 0 | 100% expert/specialist-reviewed before merge |
| **License-compliance violations** (non-open media touched) | 0 | **0** (hard gate; any violation is a stop-the-line incident) |
| Beneficiary validation (screen-reader user confirms a described batch is usable) | none | ≥ 1 qualitative review per target collection |

**Accuracy rubric (replaces a binary "accuracy review passed").** Every description is scored
1–4 on four independent dimensions, not a single pass/fail flag:

1. **Factual fidelity** — every stated detail is actually present in the image (no inference).
2. **Completeness** — the content the image carries for the lesson is conveyed (no missing content).
3. **Objectivity** — observational, neutral language; no speculation about identity/intent.
4. **Functional adequacy** — works as a text alternative for a screen-reader user at the right length.

**Error taxonomy.** When a reviewer corrects or rejects a description, the failure is tagged with
one or more classes so we can see *how* the model fails and tune the style guide: **hallucinated
detail** (asserts something not in the image), **wrong value** (misreads a number/label/data point),
**missing content** (omits lesson-bearing content), **non-objective** (speculation / loaded
language). The taxonomy mix is reported alongside the reviewer-corrected rate.

**Counting "share of described images" (reproducible).** The metric is `described ÷ in-scope` for a
named collection snapshot at a recorded date. The **denominator** is the count of in-scope
(openly-licensed, non-decorative) images in that snapshot. **"Described"** is detected
programmatically per host: for Commons, a non-empty structured caption / `P2096`-style field in the
file's target language; for a textbook repo, a non-empty, non-placeholder `alt`/long-description
field on the figure. Baseline and every delta are computed against the same snapshot definition so
they are comparable over time and across reviewers.

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
2. **Pre-flight flagging & dedup** — automated checks classify the image: decorative vs
   informative; complexity (simple photo vs chart/map/diagram → needs long description);
   sensitivity (people / identifiable individuals / medical / potentially traumatic) → routes to
   specialist review. **Dedup is two-layer:** (a) exact match on the host **source identifier**
   (`sourceCollection` + `imageId`), and (b) near-duplicate detection via a **perceptual hash**
   (e.g. pHash) compared against already-described records, so re-uploads/crops/resizes are caught.
   A flagged near-duplicate routes to human confirmation rather than auto-skip.
3. **Claim & lock** — before drafting, the session **claims** the image by writing a lock keyed on
   `sourceCollection` + `imageId` (and its perceptual-hash bucket) into the shared `records/` index.
   A claim has a TTL and a session/owner stamp; an unexpired claim makes the image invisible to other
   sessions' batches. This prevents two parallel sessions from double-describing or double-PRing the
   same image. Claims are released on completion, rejection, or TTL expiry.
4. **Draft** — agent produces alt-text per the style guide, plus a long description if complex.
   Output is a structured record, never free-floating text.
5. **Human accuracy review** — a reviewer scores the description against the 4-dimension accuracy
   rubric (see Success metrics) and tags any failure with the error taxonomy; it must be correct,
   objective, complete, and functional. Charts/maps/data figures and all sensitive images require
   the appropriate reviewer.
   - **Data-figure sub-procedure (charts/maps/data figures).** A reviewer may not verify specific
     numbers from the rendered image alone. The describer must either (a) attach the figure's
     **source data, caption, or surrounding text** so stated values are checkable against a source,
     or (b) fall back to **describing the structure, not specific values** ("a bar chart comparing
     four regions, highest at left, declining to the right"), explicitly avoiding any number that
     cannot be sourced. Unsourced specific values are a **wrong value** error and are not merged.
6. **Upstream contribution (additive only)** — an adapter formats the accepted description for the
   host and submits it (Commons structured caption/description edit; or a PR to the textbook repo).
   The adapter **must not overwrite an existing non-empty description**: if the host already has one,
   the item is skipped (or routed to human review as a proposed improvement, never a silent
   replacement), keeping every contribution additive. Adapters detect edit conflicts / concurrent
   changes and, on conflict, re-fetch and re-evaluate rather than clobber. Attribution and provenance
   travel with the contribution.
7. **Track to live** — record merge status; a deed is "done" only when merged and visible upstream.

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
perceptualHash     pHash for near-duplicate detection across sessions
claim              { owner, sessionId, claimedAt, ttl } lock to prevent double-work (nullable)
license            SPDX-style id, e.g. "CC-BY-4.0" | "CC0-1.0" | "PD"
attribution        required attribution string (author/creator + source)
imageClass         decorative | informative-simple | informative-complex
sensitivity        none | people | identifiable-person | medical | sensitive-historical
existingDescription whether host already has a non-empty description (skip/route, never overwrite)
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
2. **Accuracy review** by a human — mandatory for every description, scored on the 4-dimension
   accuracy rubric (factual fidelity, completeness, objectivity, functional adequacy) with any
   failure tagged by the error taxonomy; nothing merges below 3/4 on any dimension. Specialist for
   medium/high.
3. **Style-guide conformance** (length, objectivity, decorative-vs-informative correct).
4. **Sensitivity clearance (testable gate).** This gate is not a function of model confidence:
   **every image flagged as containing people or as medical gets human eyes regardless of how
   confident — or unconfident — the flag is** (a low-confidence "maybe a person" still routes to
   review; the flag failing open never lets a people/medical image through unseen). The reviewer
   clearing a sensitive image must meet a stated **credential bar**: sensitivity/people imagery →
   reviewer with documented sensitivity-review training; medical/diagnostic → credentialed expert
   (high-risk path) or the item is declined. Credentials are recorded in `provenance`.

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

**Sequencing — securing a channel is a hard gate.** The channel-securing research task is a
**blocking prerequisite**: no describe or PR task starts until ≥ 1 upstream channel is confirmed.
This avoids producing descriptions with nowhere to ship. **Kill/pivot criteria:** if, after the
time-boxed channel-securing effort, *no* channel from the candidate list will accept human-reviewed,
AI-drafted descriptions, the project **pivots** (try the next candidate channel / collection per the
decision tree below) or **stops** (does not proceed to describe/PR work) — it does not generate a
backlog of undeliverable descriptions.

**Candidate upstream channels (priority order, with known acceptance posture).** The metric is
deliberately **decoupled from any single host** — "described images merged upstream" counts across
whichever channels are live:
1. **Open-textbook repos** (OpenStax-style and similar) — PR-based, clearest acceptance path where
   the repo invites contributions; acceptance posture: *generally welcoming to additive a11y PRs,
   per-repo confirmation still required.*
2. **Wikimedia Commons structured-data captions** (`P2096`-style / structured captions) — large
   volume, but acceptance posture: *AI-drafted edits at volume are policy-sensitive; needs talk-page
   pre-engagement and bot/edit-rate agreement before scaling.*
3. **GLAM open-access collections / IIIF partners** — high-value, but acceptance posture: *requires a
   named institutional partner and a delivery mechanism; TO BE SECURED.*

**Decision tree (which channel first):** confirmed PR-accepting textbook repo? → start there.
Else, a Commons community willing to accept the volume after pre-engagement? → start there. Else, a
named GLAM partner with a delivery path? → start there. Else → hit kill/pivot criteria above.

**M0 image-selection criteria (the batch of 10 must exercise every path, not 10 easy photos).** The
hand-curated cold-start batch is deliberately spread across image classes so the pipeline is proven
on hard cases too: at least one **decorative** image (alt = ""), several **simple informative**
photos, at least two **complex** images requiring a long description (including at least one
chart/map/data figure exercising the data-figure sub-procedure), and at least one **sensitive**
image (people/identifiable/medical) exercising the sensitivity gate and specialist review. A batch
of 10 trivially-easy photos is explicitly disallowed — it would not prove the pipeline.

Exit criteria:
- Alt-text style guide v1 published.
- ≥ 1 upstream channel **confirmed** (a repo that will take our PRs, or a validated Commons
  workflow) — closes the partner gap for at least one collection; **gates** all describe/PR work.
- License-verification + provenance record format finalised and applied to a sample.
- Cold-start batch selected per the image-selection criteria above (covers decorative / simple /
  complex / sensitive paths).
- **≥ 10 descriptions merged upstream and live**, each with recorded license + attribution.
- Baseline "share of described images" measured for one chosen target collection per the counting
  method in Success metrics (named snapshot + denominator + "described" detection).

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
  subject to community norms, bot policy, and rate limits. **Pre-engagement is a required exit
  artifact** of the channel-securing research task, not an afterthought: a documented **talk-page (or
  equivalent) statement of intent**, an explicit **bot/edit approval** or confirmation that
  human-reviewed manual edits at the proposed cadence are welcome, and an **agreed volume ramp**
  (starting small, scaling only with community assent). Describe/PR work does not begin until this
  artifact exists for the chosen channel.
- **Open-textbook repositories** (OpenStax-style and similar) — must accept external PRs;
  pre-engagement artifact for a repo is a confirmation (issue/maintainer reply/contributing policy)
  that additive a11y PRs at the proposed cadence are welcome.
- **GLAM open-access collections / IIIF endpoints** — source images and license metadata.
- **License metadata sources** — SPDX identifiers; CC license deeds; host-provided license fields.
- **Elyos platform pieces** — `packages/cli` (task workspace + PR prep, donated lane), the Task
  schema (`packages/schema`), and `adapters/` for all host-specific code. The CLI never runs an
  agent headless and never authenticates a coding agent.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Inaccurate description (esp. charts/maps/data) merged upstream | Medium | High | Mandatory human accuracy review scored on the 4-dimension rubric + error taxonomy; specialist reviewer for data figures; complexity flagging routes hard cases; **data-figure sub-procedure** (require source data/caption, else describe structure not specific values) | Maintainer / Reviewers |
| Non-open / unclear-license image ingested or described | Low | High | Hard pre-ingest license gate; record license+attribution; "unknown = excluded"; stop-the-line on any violation | Steward |
| Sensitive imagery (people, medical, traumatic) mishandled | Medium | High | Sensitivity flagging; specialist gate; no identity/private-attribute inference; high-risk escalation path | Reviewers / Expert |
| No upstream partner/channel secured → nothing ships | Medium | High | M0 exit criterion is a confirmed channel; `verifiedNeed=false` until secured; do not scale before delivery proven | Maintainer / Steward |
| Host community rejects bulk/automated contributions | Medium | Medium | Honour bot/automation policy; concrete pre-engagement exit artifact (talk-page intent, bot/edit approval, agreed volume ramp) before any volume; start small; human-reviewed, not spam; metric decoupled from a single host so one rejection is not fatal (decision tree across candidate channels) | Steward |
| Privacy harm from describing identifiable individuals | Low | High | Exclude unclear-consent images even if file is openly licensed; neutral observational language only | Reviewers |
| Reviewer capacity becomes the bottleneck | High | Medium | Reviewer rotation; batch sizing; flagging to triage easy vs hard; only scale fan-out to review capacity | Maintainer |
| Duplicate / redundant work across sessions | Medium | Low | Two-layer dedup (source-id exact match + perceptual-hash near-duplicate); TTL'd per-image claim/locking so parallel sessions can't double-describe/double-PR; milestone-scoped batches | Maintainer |
| Model hallucinates detail not present in image | Medium | High | Style guide forbids inference; "describe what is visible"; reviewer verifies against image and tags *hallucinated detail* in the error taxonomy | Reviewers |
| Contribution overwrites or conflicts with an existing description | Low | Medium | Adapters are additive-only: never overwrite a non-empty host description (skip or route as proposed improvement); detect edit conflicts and re-fetch rather than clobber | Steward / Maintainer |

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
- **Additive contributions only.** Contributions add a missing description; they never overwrite an
  existing one. If a host later edits or reverts a contributed description, that is the community's
  prerogative — the provenance log records the original contribution and its final disposition, and
  conflicts are resolved by re-fetch-and-re-evaluate, never by clobbering host changes.
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
- What exact **credential bar** clears each gate — what documented training qualifies a
  sensitivity/people reviewer, and what credential qualifies a medical/diagnostic expert? (The gate
  is already specified as testable: every people/medical image gets human eyes regardless of flag
  confidence; this question is about who is qualified to be those eyes.)
- What **inter-reviewer agreement** threshold and sampling rate keep the accuracy rubric calibrated,
  and how often is the rubric / error taxonomy revised from the observed failure mix?
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
