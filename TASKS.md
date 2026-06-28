# TASKS — a11y-alttext-commons

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

Backlog for the `a11y-alttext-commons` good-deed project. Read alongside `PLAN.md` (same directory).

## How these tasks map to Elyos

Every task here becomes a **Task JSON** validated by `packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable, e.g. `a11y-alttext-commons-style-001` (the table's ID column).
- `title` — the task title.
- `project` — `"a11y-alttext-commons"` for all tasks.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (see each task).
- `lane` — `"donated"` for all tasks (the human runs their own agent; CLI preps workspace + PR).
  No `funded` tasks in this backlog, so `fundedBudgetUsd` is not required.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["accessibility","education","open-culture"]`.
- `riskTier` — `low | medium | high` (data figures and people-imagery are `medium`; medical/legal
  escalations are `high`).
- `urgent` — boolean (default `false`; nothing here is time-critical).
- `deliverable` — `pr | dataset | document | translation`. Upstream descriptions → `pr`;
  description records → `dataset`; style guide / checklists → `document`.
- `tokenEstimate` — `small | medium | large` (the table's Size column).
- `status` — `open | in-progress | review | delivered | done` (all start `open`).
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — task body fields.
- `requestor` — proposer/maintainer; `verifiedNeed` — **`false`** for delivery-dependent tasks
  until an upstream partner/channel is secured (see PLAN "partner gap"); `true` only where the
  need is self-evident and not channel-dependent.
- `outputLicense` — MIT for code; `CC-BY-4.0` / `CC0-1.0` / host license for content & descriptions.

At production scale this project fans out: **one image = one task**, drawn from a milestone-scoped
batch (the `*-describe-*` and `*-batch-*` tasks below are the templates for that fan-out).

---

## Milestone M0 — Foundation & cold-start

Prove one description can travel from an openly-licensed source to *merged upstream and live*.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| a11y-alttext-commons-style-001 | Author alt-text & long-description style guide v1 | writing | small | low | document | — | Maintainer |
| a11y-alttext-commons-research-001 | Secure ≥1 upstream channel (Commons or textbook repo) | research | medium | low | document | — | Maintainer + Steward |
| a11y-alttext-commons-data-001 | Define per-image description record + license/provenance format | data | small | low | dataset | style-001 | Maintainer |
| a11y-alttext-commons-research-002 | Validate license-verification rules for target sources | research | small | medium | document | — | Steward |
| a11y-alttext-commons-describe-001 | Hand-describe 10 curated images (cold-start batch covering decorative/simple/complex/sensitive) | data | medium | medium | dataset | research-001 (channel gate), style-001, data-001, research-002 | Specialist reviewer |
| a11y-alttext-commons-pr-001 | Merge the 10 descriptions upstream + measure baseline share | maintenance | medium | medium | pr | research-001, describe-001 | Steward |

**Sequencing — channel-securing is a hard gate.** `research-001` blocks `describe-001` and `pr-001`:
no description is drafted or submitted until ≥1 upstream channel is confirmed. If the time-boxed
channel-securing effort confirms no channel, the project hits its **kill/pivot criteria** (try the
next candidate channel/collection per PLAN's decision tree, or stop) rather than producing
undeliverable descriptions.

**Key task acceptance criteria**

- **a11y-alttext-commons-style-001** (style guide v1)
  - [ ] Defines alt-text length/structure limits and an objectivity rule ("describe what is
        visible; never infer identity, intent, or unseen facts").
  - [ ] Gives decorative-vs-informative decision rules (when alt = "").
  - [ ] Covers complex images (charts, maps, diagrams, data figures) and when a long description
        is required.
  - [ ] Has an explicit sensitive-imagery section (people, identifiable individuals, medical,
        traumatic/historical) and routing to specialist review.
  - [ ] Published, versioned, and licensed CC-BY-4.0.

- **a11y-alttext-commons-research-001** (secure upstream channel)
  - [ ] Evaluates candidate channels in priority order (open-textbook repos → Commons structured
        captions → GLAM/IIIF partners) with each one's known acceptance posture, and applies the
        PLAN decision tree to pick the first to pursue.
  - [ ] Identifies ≥1 concrete channel (named repo that accepts PRs, or a validated Commons
        workflow) confirmed to accept human-reviewed, AI-drafted descriptions.
  - [ ] Produces a **pre-engagement exit artifact** for the chosen channel: a documented talk-page
        (or equivalent) statement of intent, a bot/edit approval or confirmation that human-reviewed
        edits at the proposed cadence are welcome, and an agreed volume ramp (start small, scale on
        assent).
  - [ ] Records that channel's required output license and attribution format.
  - [ ] Documents the host's bot/automation policy and rate limits.
  - [ ] States the time-box and the kill/pivot criteria if no channel is secured.
  - [ ] On success, flips `verifiedNeed=true` for tasks targeting that channel.

- **a11y-alttext-commons-describe-001** (cold-start batch of 10)
  - [ ] The batch of 10 is selected to exercise every path — ≥1 decorative, several simple
        informative, ≥2 complex (incl. ≥1 chart/map/data figure), ≥1 sensitive (people/medical) —
        not 10 easy photos.
  - [ ] All 10 images verified CC-* / PD with license + attribution recorded before description.
  - [ ] Each description conforms to style guide v1; complex images include a long description; any
        chart/map/data figure follows the data-figure sub-procedure (source data/caption attached,
        else describe structure not specific values).
  - [ ] Every record is scored on the 4-dimension accuracy rubric against the actual image, with any
        failure tagged by the error taxonomy; nothing below 3/4 on any dimension is kept.
  - [ ] Every people/medical image gets human eyes regardless of flag confidence and is cleared by a
        specialist meeting the credential bar.
  - [ ] No identity/private-attribute inference about any depicted person.

- **a11y-alttext-commons-pr-001** (merge + baseline)
  - [ ] ≥10 descriptions merged upstream and confirmed live for screen-reader users (additive only;
        never overwriting an existing description).
  - [ ] Each merged item carries license + attribution + provenance.
  - [ ] Baseline "share of described images" measured and recorded for one target collection using a
        reproducible method: named snapshot + date, in-scope denominator, and programmatic
        "described" detection (so later deltas are comparable).

**M0 Definition of Done:** style guide v1 published; ≥1 upstream channel confirmed; ≥10
descriptions merged upstream and live with full provenance; baseline described-share measured;
zero license violations.

---

## Milestone M1 — Adapters & flagging

Turn the manual slice into a repeatable, policy-compliant pipeline.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| a11y-alttext-commons-adapter-001 | Commons structured-data caption/description adapter | code | large | medium | pr | research-001, data-001 | Maintainer |
| a11y-alttext-commons-adapter-002 | Open-textbook PR adapter (locate figure, patch alt, open PR) | code | large | medium | pr | research-001, data-001 | Maintainer |
| a11y-alttext-commons-pipeline-001 | License-verification pre-ingest gate (hard exclude unknown) | code | medium | medium | pr | research-002, data-001 | Steward |
| a11y-alttext-commons-pipeline-002 | Flagging pass: complexity + sensitivity + duplicate detection | code | large | medium | pr | data-001 | Maintainer |
| a11y-alttext-commons-doc-001 | Reviewer workflow + 4-dimension accuracy rubric, error taxonomy & sensitivity-credential checklist | writing | small | low | document | style-001 | Maintainer |
| a11y-alttext-commons-batch-001 | Describe + merge batch of ~100 (first repeatable run) | data | large | medium | pr | adapter-001 OR adapter-002, pipeline-001, pipeline-002, doc-001 | Reviewer rotation |

**Key task acceptance criteria**

- **a11y-alttext-commons-pipeline-001** (license gate)
  - [ ] Rejects any image whose license is not verifiably CC-* or PD ("unknown = excluded").
  - [ ] Records SPDX-style license id + attribution + source URL + retrieval date per accepted image.
  - [ ] Never ingests/caches excluded media; emits an auditable decision log (no secrets in logs).

- **a11y-alttext-commons-pipeline-002** (flagging)
  - [ ] Classifies decorative vs informative and simple vs complex (routes complex → long desc).
  - [ ] Flags people / identifiable individuals / medical / traumatic → specialist gate; people and
        medical route to human review regardless of flag confidence (fail-open never ships unseen).
  - [ ] Detects duplicates/near-duplicates via two-layer dedup: source-id exact match +
        perceptual-hash (pHash) near-duplicate.
  - [ ] Implements per-image claim/locking (TTL + owner stamp keyed on source-id/hash bucket) so
        parallel sessions cannot double-describe or double-PR the same image.

- **a11y-alttext-commons-adapter-001** (Commons adapter)
  - [ ] Submits a human-reviewed caption/description via the Wikibase/MediaWiki API with correct
        license + attribution, honouring bot policy and rate limits.
  - [ ] Additive-only: never overwrites an existing non-empty description (skip or route as a proposed
        improvement); detects edit conflicts and re-fetches/re-evaluates rather than clobbering.
  - [ ] Tokens supplied by the human's environment; never logged or committed.
  - [ ] Records the resulting edit/revision id as `upstreamRef`.

- **a11y-alttext-commons-batch-001** (first ~100 run)
  - [ ] ≥100 descriptions merged upstream; acceptance rate measured.
  - [ ] All went through license gate → flagging → human review → adapter submission.

**M1 Definition of Done:** ≥1 adapter live and policy-compliant; license gate + flagging
operational; reviewer workflow documented with ≥2 reviewers onboarded; ≥100 descriptions merged
upstream with measured acceptance rate; zero license violations.

---

## Milestone M2 — Controlled scale

Meaningfully move the described-share metric in one-to-two collections, with quality held.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| a11y-alttext-commons-batch-002 | Scale to ≥500 merged across ≥2 collections | data | large | medium | pr | batch-001 | Reviewer rotation |
| a11y-alttext-commons-doc-002 | Specialist guide: data figures (charts/maps/diagrams) | writing | medium | medium | document | style-001 | Data-figure specialist |
| a11y-alttext-commons-research-003 | Beneficiary validation with screen-reader users | research | small | low | document | batch-001 | Maintainer |
| a11y-alttext-commons-data-002 | Quality metrics: acceptance, reviewer-corrected, rubric scores, inter-reviewer agreement & error-taxonomy mix | data | small | low | dataset | batch-001 | Maintainer |

**Key task acceptance criteria**

- **a11y-alttext-commons-batch-002** (scale run)
  - [ ] ≥500 descriptions merged upstream and live across ≥2 collections.
  - [ ] Acceptance rate ≥80%; reviewer-corrected rate ≤25% and trending down.
  - [ ] Zero license-compliance violations; sensitive items 100% specialist-cleared.

- **a11y-alttext-commons-research-003** (beneficiary validation)
  - [ ] ≥1 screen-reader user reviews a described batch and confirms usability per collection.
  - [ ] Findings feed back into the style guide / reviewer checklist.

**M2 Definition of Done:** ≥500 merged across ≥2 collections at ≥80% acceptance; ≥1 beneficiary
validation; quality metrics dashboarded; zero license violations.

---

## Backlog / future (sized, unscheduled)

| ID | Title | Type | Size | Risk | Deliverable | Notes |
|---|---|---|---|---|---|---|
| a11y-alttext-commons-adapter-003 | GLAM / IIIF source + contribution adapter | code | large | medium | pr | New source class; needs partner channel |
| a11y-alttext-commons-translate-001 | Multilingual description extension (sibling project?) | writing | large | medium | translation | Out of current scope; high a11y value |
| a11y-alttext-commons-doc-003 | Sensitive-imagery handling deep-dive guide | writing | medium | medium | document | Expand style-guide section |
| a11y-alttext-commons-pipeline-003 | Outcome-tracking dashboard (merged/live per collection) | code | medium | low | pr | Sustainability metric |
| a11y-alttext-commons-research-004 | High-risk escalation path (medical/legal imagery) | research | small | high | document | Defines credentialed expert sign-off flow |
| a11y-alttext-commons-maint-001 | Adapter maintenance vs host API/policy changes | maintenance | medium | low | pr | Ongoing post-delivery |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 task. `verifiedNeed` is `true` here: a published,
openly-licensed style guide is a self-evidently useful artifact and does not depend on an upstream
delivery channel being secured (unlike the `pr`/`describe` tasks, which are `false` until M0's
channel is confirmed).

```json
{
  "id": "a11y-alttext-commons-style-001",
  "title": "Author alt-text & long-description style guide v1",
  "project": "a11y-alttext-commons",
  "type": "writing",
  "lane": "donated",
  "priority": "high",
  "domain": ["accessibility", "education", "open-culture"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "small",
  "status": "open",
  "context": "Openly-licensed images in open textbooks, Wikimedia Commons, and GLAM open collections frequently lack alt-text and long descriptions, excluding blind and low-vision screen-reader users. Before any image is described at scale, the project needs a single, versioned style guide so that descriptions from many parallel AI sessions are consistent, objective, and WCAG-conformant.",
  "objective": "Write and publish version 1 of the alt-text and long-description style guide that all describe/batch tasks will follow.",
  "acceptanceCriteria": [
    "Defines alt-text length/structure limits and an objectivity rule (describe only what is visible; never infer identity, intent, or unseen facts).",
    "Gives clear decorative-vs-informative decision rules, including when alt-text should be empty.",
    "Covers complex images (charts, maps, diagrams, data figures) and when a long description is required.",
    "Includes an explicit sensitive-imagery section (people, identifiable individuals, medical, traumatic/historical) and routing to specialist review.",
    "Aligns with WCAG 2.x SC 1.1.1 and is published, versioned, and licensed CC-BY-4.0."
  ],
  "resources": [
    "C:\\code\\elyos\\planning\\projects\\a11y-alttext-commons\\PLAN.md",
    "C:\\code\\elyos\\governance\\proposals\\a11y-alttext-commons.md",
    "WCAG 2.x Success Criterion 1.1.1 (Non-text Content)"
  ],
  "output": "A versioned style-guide document (style-guide/alt-text-style-guide.md), CC-BY-4.0, covering alt-text rules, long-description rules, decorative-vs-informative, data-figure conventions, and sensitive-imagery handling.",
  "requestor": "jdev1977",
  "verifiedNeed": true,
  "outputLicense": "CC-BY-4.0"
}
```
