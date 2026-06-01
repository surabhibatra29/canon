# Canon — Personal PM Curriculum Tracker

> *Design your curriculum with AI. Execute it with intention. Walk out with proof.*

A self-contained curriculum tracker built during a PM career break, to actually execute a structured, self-designed AI-era PM curriculum, not just collect it in a Notion doc.

**[→ Live demo](https://surabhibatra29.github.io/canon/curriculum-tracker.html)**

---

## What it does

- Tracks progress across 11 PM modules: readings, videos, assignments, reflections
- Every module has a named **output artifact**: a specific deliverable, not just content consumed
- **Evaluate tab** — every module has a rubric with weak/strong artifact criteria and "ask yourself" prompts, plus a self-evaluation workspace
- **Skip / Restore** — dismiss any reading or watching item you don't want; it's struck out and excluded from your progress, restorable any time
- **Spine + pace structure** — core modules marked, pace note on the dashboard ("one module every 10 days")
- Rich-text assignment workspace with collapsible sections and source citations
- PDF attachment support (admin resources + personal uploads)
- Module 10: a private build log for drafting writing as you go
- Cloud sync via Supabase, works across desktop and mobile browser
- Full dark mode, keyboard navigation (j/k), Design mode for curriculum editing
- All resources curated to verified links — direct TED, MIT OCW, HBR, practitioner sites; no dead-end search terms

## The design philosophy

Most learning tools are instruction products: they tell you what to do. Canon is an **execution product**: it holds your work.

The curriculum is a template. The artifact you produce at the end of each module is the proof. Reflections are private proof for yourself. Assignments are public-ready proof for anyone else. Both count.

## Why I built this

I kept losing progress context across Notion, Google Docs, and browser tabs. I wanted one workspace that matched how I actually study, with notes, sources, assignments, and reflections in one place. So I built it.

## What it demonstrates

- **Product thinking**: scoped an MVP that solved my actual problem, made deliberate tradeoffs (single file, no framework, offline-first)
- **AI-assisted development**: used Claude as a technical collaborator. I owned the product decisions, architecture tradeoffs, and UX direction
- **Local-first architecture**: localStorage for instant writes, Supabase for cross-device sync and file storage
- **Iterative delivery**: built and shipped incrementally, adding features as real usage revealed gaps

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
