# Canon

> Execute a curriculum deliberately. Walk out with proof.

Canon is a personal learning workspace. You take a curriculum, work through it module by module, and produce a real artifact at the end of each one. I built it solo, with AI-assisted development, during a career break.

**[Live](https://surabhibatra29.github.io/canon/curriculum-tracker.html)** · **[Intro video (2 min)](https://www.loom.com/share/02c93c7ead8e4b37a517bffa76144359)** · **[PRD](PRD.md)**

![Canon dashboard, the module gallery](assets/canon-dashboard.png)

---

## Why I built it

I took a career break to upskill. I knew roughly where my gaps were, but working through them was another matter. I tried cohort courses and a big stack of self-directed reading. What I had to show for it was forty open tabs, a lot of half-read articles, and not much else.

So Canon started as a question more than a product. Could a tool actually change the way I learn, not just store what I read? I am the test subject. The curriculum, the structure, the small nudges, all of it is built to change as I move through it. When something does not fit the way I actually study, I rework it.

## How it works

A module holds readings, things to watch, an assignment with a real deliverable at the end, a space to reflect, and a rubric to grade myself against.

The behaviour I am testing is simple. A module only counts as finished once I have produced the thing it asks for. I have not hit that bar consistently, and watching where I fall short is the point. Canon is an experiment in how a learning tool could shape behaviour, not just track it. Where I cut corners tells me what to build next.

The curriculum inside it is mine. I worked through a structured discovery process with AI to find my gaps, then generated a PM curriculum sized to my goals. How many modules and how deep they go is a choice, not a fixed number.

## What it does

You start on a gallery of modules and open one. Inside, you work through a curated set of readings and videos, checking things off as you go. If you already know something, you skip it and note why, so the list reflects where you actually are.

You can take notes on anything as you read, and they stay attached to the item. Then comes the real work: each module has an assignment with a named artifact, written in a workspace where you can cite the exact readings you are drawing from. When you finish, you reflect on what landed and grade yourself against the module rubric.

Underneath, it syncs to the cloud, remembers the page you left off on in a PDF, survives a refresh or the back button, and has a dark mode.

## What building it taught me

- Clear problem framing first, AI-accelerated execution second. Every time I got that order wrong, I redid the work.
- I leaned on AI three ways: as a sparring partner for positioning, for technical options with tradeoffs before I committed, and for specced UX work rather than "make it look nice."
- I keep a living PRD: problem framing, feature decisions, and what I deliberately left out.
- Local-first architecture: localStorage for instant writes, Supabase for sync, IndexedDB for PDF blobs.

## Next up (in progress)

- **Generate a curriculum with AI, in the app.** Right now the curriculum is generated with a prompt outside Canon. The plan is to bring it inside: answer a few questions, get a structured curriculum back.
- **Multiple curricula.** The execution layer is not specific to PM. The same readings, reflections, rubrics, and tracking work for any subject.

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

## The curriculum I am running now

PM, Sharpened, a curriculum I generated for myself. Each module ends in a named artifact.

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
