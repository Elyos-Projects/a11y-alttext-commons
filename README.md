# a11y-alttext-commons

> Alt-text & image descriptions for openly-licensed media, contributed upstream.  ·  **Risk tier:** low-medium  ·  **Status:** proposed (planning)

Generate high-quality alternative text and long image descriptions for **openly-licensed** images
in open textbooks, Wikimedia Commons, and GLAM (galleries/libraries/archives/museums) open
collections, and contribute them back upstream. Turns a massively parallel accessibility gap into
millions of small, reviewable tasks.

**Definition of shipped:** Reviewed descriptions merged upstream into open collections (Commons structured data, open textbooks).

This is an **Elyos** good-deed project. Contributors pull a task, do it with their own coding agent, and open a PR. Platform: https://github.com/jdev1977/elyos

## Planning
- [PROPOSAL.md](./PROPOSAL.md) — why this qualifies as a good deed (Good Deed Definition)
- [PLAN.md](./PLAN.md) — architecture, roadmap & milestones, risks
- [TASKS.md](./TASKS.md) — the full task backlog
- [tasks/](./tasks/) — ready-to-pull task JSON(s)

## Contribute
```bash
elyos browse
elyos pull --task-file tasks/a11y-alttext-commons-style-001.json --repo Elyos-Projects/a11y-alttext-commons
# do the work with your own agent, then:
elyos submit a11y-alttext-commons-style-001 --repo Elyos-Projects/a11y-alttext-commons
```

## Licensing & review
- **Licensing:** Descriptions under the host's open license (CC0/CC-BY). Tooling: MIT.
- **Review:** risk tier **low-medium** — deeds are *delivered, not merged*; a domain reviewer must sign off before merge.

> Status: this project is in **planning** and not yet ratified through Elyos governance; no adopting partner/requestor is secured yet (`verifiedNeed: false` on delivery-dependent tasks).
