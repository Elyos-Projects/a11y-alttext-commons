# Proposal: a11y-alttext-commons

**Proposer:** jdev1977 (drafted by Claude Code)
**Date:** 2026-06-28
**Domain(s):** accessibility, education, open-culture
**Lane:** donated
**Requestor / beneficiary:** blind & low-vision learners; open-education and GLAM communities  ·  **Verified need:** yes (WCAG/alt-text gap is well documented)

## Summary
Generate high-quality alternative text and long image descriptions for **openly-licensed** images
in open textbooks, Wikimedia Commons, and GLAM (galleries/libraries/archives/museums) open
collections, and contribute them back upstream. Turns a massively parallel accessibility gap into
millions of small, reviewable tasks.

## Why it qualifies as a good deed
- **Tangible public benefit:** makes open knowledge usable by screen-reader users.
- **Freely available:** descriptions contributed upstream under the host's open license.
- **Not primarily for-profit:** benefits learners and the public commons.
- **No harm/misinformation:** descriptions are factual, neutral, and review-checked.
- **Executable by AI sessions:** one image = one task; ideal fan-out work.

## Scope / first tasks
- (small) Adopt an alt-text style guide (length, objectivity, decorative-vs-informative rules).
- (medium) Describe a batch of N images from one open textbook; record source + license per image.
- (small) Build a duplicate/low-confidence flagging pass for human review.
- (medium) Upstream-contribution adapter (Commons "caption"/structured-data, textbook PRs).

## Definition of shipped
Reviewed descriptions merged into the upstream source (Commons structured data, open-textbook
repos), measurably improving the share of described images in target collections.

## Risks / review needs
**Risk tier: low–medium.** Needs a description-accuracy review (especially for charts, maps,
people, and sensitive imagery). **License gate:** only CC / public-domain sources; record the
image's license and attribution; never describe (or ingest) non-open media.
