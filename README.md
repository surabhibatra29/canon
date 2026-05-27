# Canon — Personal PM Curriculum Tracker

> *Design your curriculum with AI. Execute it with intention. Walk out with proof.*

A self-contained curriculum tracker I built during my PM career break — to actually execute the 9-module AI-era PM curriculum I designed with Claude, not just collect it in a Notion doc.

**[→ Live demo](https://surabhibatra29.github.io/canon/curriculum-tracker.html)**

---

## What it does

- Tracks progress across 9 PM modules: readings, videos, assignments, reflections
- Rich-text assignment workspace with collapsible sections and source citations
- PDF attachment support (admin resources + personal uploads)
- Module 10: a private build log for drafting LinkedIn posts as I go
- Cloud sync via Supabase — works across desktop and mobile browser
- Full dark mode, keyboard navigation (j/k), admin mode for curriculum editing

## Why I built this

The structured PM curriculum I designed covered strategic thinking, design thinking, AI product strategy, pricing, metrics, and more. But I kept losing progress context across Notion, Google Docs, and browser tabs.

I wanted one workspace that matched how I actually study — with notes, sources, and assignments in one place, not scattered across tools. So I built it.

## What it demonstrates

- **Product thinking**: scoped an MVP that solved my actual problem, made deliberate tradeoffs (single file, no framework, offline-first)
- **AI-assisted development**: used Claude as a technical collaborator — I owned the product decisions, architecture tradeoffs, and UX direction
- **Local-first architecture**: localStorage for instant writes, Supabase for cross-device sync and file storage
- **Iterative delivery**: built and shipped incrementally, adding features as real usage revealed gaps

## Tech

Single HTML file — no build step, no dependencies to install, runs directly in a browser.

| Layer | Choice |
|-------|--------|
| Frontend | Vanilla JS, CSS custom properties |
| Auth + DB | Supabase (Postgres + RLS) |
| File storage | Supabase Storage |
| PDF rendering | PDF.js (lazy-loaded from CDN) |
| Font | Inter via Google Fonts |
| Hosting | GitHub Pages |

## Curriculum modules

1. Strategic Thinking & Systems Thinking
2. Design Thinking & Human-Centred Research
3. AI Fundamentals — Rigorous, Non-Technical
4. AI Tools — Hands On
5. Pricing & Monetization Strategy
6. AI Product Strategy — The Layer Above the Tools
7. Execution & Stakeholder Management
8. Metrics, Evaluation & North Star Thinking
9. Pulling It Together — Artifact & Portfolio
10. Build Log & Publishing *(private scratchpad)*

---

*Built by [Surabhi Batra](https://www.linkedin.com/in/surabhibatra/) during a structured PM career break, Singapore 2025.*
