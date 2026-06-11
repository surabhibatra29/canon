# Canon — AI-Era PM Curriculum Execution Workspace

> *Generate your curriculum with AI. Execute it deliberately. Walk out with proof.*

A self-contained tool built to close the gap between structured learning and structured output. Co-designed with AI, built with AI, and used daily as the primary learner.

**[→ Live demo](https://surabhibatra29.github.io/canon/curriculum-tracker.html)** · **[→ Intro video (2 min)](https://www.loom.com/share/02c93c7ead8e4b37a517bffa76144359)**

---

## The design problem

Most learning tools are instruction products: they solve the content problem. Canon is an **execution product**: it solves the proof problem, the distance between reading something and producing work that demonstrates you understood it.

The curriculum inside Canon was itself designed with AI: a structured discovery process to surface gaps, define what rigorous PM learning at this level should cover, and sequence it into 14 modules with named deliverables. The tool executes that curriculum. Both layers, what to learn and how to track it, were built with AI as a collaborator.

Reflections are private proof for yourself. Assignments are portfolio-ready proof for anyone else. The artifacts of working through this curriculum aren't always tidy Google Docs. They're a live product, a structured PRD, and real decisions made under real constraints. That's the point.

---

## What it does

- Tracks progress across **14 PM modules**: readings, watching, assignments, and reflections
- Every module produces a named **artifact**, a specific deliverable rather than a completion checkbox
- **Evaluate tab**: rubric with weak / strong / ask yourself criteria per module; self-evaluation workspace separate from the reflection tab
- **Skip / Restore**: dismiss any reading or watching item with an optional reason, excluded from progress. The skip reason respects that you may already know the material. It's not just a UX shortcut, it's a design philosophy: the tool should meet you where you are
- **Per-reading notes**: WYSIWYG note modal per reading; compact note chip shows on the row once written
- **Spine + pace structure**: four load-bearing Core modules marked; dashboard pace note tells you what to prioritise if time is short
- Rich-text assignment workspace with draggable, collapsible sections; cite specific readings inside your writing
- PDF viewer that remembers your last-read page and preserves highlights across zoom levels
- **Refresh / back button navigation**: URL hash preserves your current module and tab across sessions
- Cloud sync via Supabase; admin can publish curriculum updates to all users
- Dark mode, keyboard navigation (j/k), Design mode for curriculum editing

---

## What it demonstrates

- **Product thinking with a living PRD**: every significant feature decision is documented, with problem framing, tradeoffs considered, and what was deliberately not built
- **AI-assisted development at every stage**: used AI to define the curriculum, design the architecture, review UX decisions, and build the product. The discipline: clear problem framing first, AI-accelerated execution second. Getting that order wrong always meant redoing it
- **Dogfooding as evaluation method**: I'm the primary user. Success isn't a metric dashboard. It's whether the work I'm producing is portfolio-ready and whether the reflections I'm writing are honest enough to be embarrassing if read aloud
- **Local-first architecture**: localStorage for instant writes, Supabase for cross-device sync and file storage, IndexedDB for PDF blobs
- **Iterative delivery**: built and shipped in public, features driven by real usage

---

## Why I built this

There's no tool that holds your work through a structured curriculum. Note apps capture but don't demand output. Course platforms deliver content but don't require proof. Notion does everything, which means it does nothing well. Canon does one thing: makes it harder to pretend you've finished something you haven't.

---

## What's next

- **AI recommendations** personalised to your reflection notes and skip reasons, because generic suggestions aren't personalised learning
- **Multi-curriculum support**: the execution layer works for any domain, not just PM

---

## Tech

Single HTML file, no build step, no dependencies to install, runs directly in a browser.

| Layer | Choice |
|-------|--------|
| Frontend | Vanilla JS, CSS custom properties |
| Auth + DB | Supabase (Postgres + RLS) |
| File storage | Supabase Storage |
| PDF rendering | PDF.js (lazy-loaded from CDN) |
| Font | Inter via Google Fonts |
| Hosting | GitHub Pages |

---

## Curriculum modules

| # | Module | Artifact |
|---|--------|----------|
| 0 | Mindset & Meta-cognition | Operating Manual for Yourself |
| 1 | Strategic Thinking & Systems Thinking | Strategic Position Map |
| 2 | Design Thinking & Human-Centred Research | Discovery Map |
| 3 | AI Fundamentals (Rigorous, Non-Technical) | AI Opportunity Map |
| 4 | AI Tools, Hands On | Working AI Feature + Prompt Iteration Log |
| 5 | Product Taste & Craft | Product Critique + Evaluation Framework |
| 6 | Pricing & Monetization Strategy | Commercial Model + Pricing Page Copy |
| 7 | AI Product Strategy | 12-Month AI Product Thesis |
| 8 | Prioritization & Roadmapping | Prioritised Roadmap + Decision Memo |
| 9 | Execution & Stakeholder Management | GTM Plan + Ideal Environment Spec |
| 10 | Written Communication & PM Craft | Product Brief (one-pager) |
| 11 | Metrics, Evaluation & North Star Thinking | Metrics Framework + Weekly Review Template |
| 12 | Career Thesis | Career Thesis Document |
| 13 | Build Log & Publishing *(private scratchpad)* | Two to three public-ready drafts |

---

*Built by [Surabhi Batra](https://www.linkedin.com/in/surabhibatra/) during a structured PM career break, Singapore 2026.*
