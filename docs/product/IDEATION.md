# Canon — Ideation Log

**What this is:** A running log of product ideas, feature concepts, and open questions. Newest at the top. Not everything here will get built — some are hunches, some are half-baked, some will be wrong.

**Why it's here:** Ideas have a way of disappearing. This is where they land so they can be revisited, combined, or killed with evidence.

---

## How to read this

Each entry has a date, a one-line idea, and enough context to remember why it seemed worth noting. Status tags:
- `[Exploring]` — thinking about it, not committed
- `[Specced]` — written up in PRD, not built
- `[Built]` — shipped
- `[Parked]` — not now, maybe later
- `[Killed]` — tried it or thought it through, not doing it

---

## Log

### 2026-06-16

**Reading groups / submodules** `[Specced]`
As modules grow in content, a flat list of 10+ readings gets hard to navigate. A lightweight `group` field on readings clusters them under section headers (e.g. "How to Think", "Self-Knowledge") without adding a new entity. Spec in PRD §14.

**What's New modal** `[Exploring]`
Show users what changed in the product — written from a user perspective, not a git log. Appears in the header with a badge when there's something new. Non-logged-in users see it too. Should only include shipped features, with a "Coming up" section for WIP.

**Retrieval practice quiz mode** `[Exploring]`
The Evaluate tab's Ask Yourself questions are a partial implementation of retrieval practice. A fuller version would surface questions from any completed module, not just the current one — a lightweight quiz loop, not a full SRS system.

**Spaced practice cue** `[Exploring]`
No spacing algorithm in Canon currently. A lightweight version: a "revisit" nudge that surfaces one prior module per week for a quick re-read or re-attempt of the Ask Yourself questions. Simpler than Anki, consistent with the pace-note philosophy.

---
