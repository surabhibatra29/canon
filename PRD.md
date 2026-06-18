# Canon: Product Requirements Document

**Last updated:** 2026-06-14  
**Status:** Living document. I update it after any significant feature change.  
**Owner:** Surabhi Batra  
**Live URL:** https://surabhibatra29.github.io/canon/curriculum-tracker.html  
**Codebase:** single self-contained file, `curriculum-tracker.html`

---

## TL;DR

Canon is a personal learning workspace, built by and for one person (of course open to anybody else, a PM who wants to up their reading!), with an experiment to ground it in cognitive science. It is built around three themes:

**1. Curate, with AI as copilot:**
- You choose the curriculum
- AI helps you find gold-standard practitioners and primary sources in any field
- The source discovery protocol: which voices to trust, how to go to the originator over the summary, is being developed through the process of running the curriculum itself
- This is a work in progress

**2. Execute and produce:**
- Track progress by what you consume and what you produce
- Every module ends in a named artifact
- You decide what to skip
- The execution layer is curriculum-agnostic

**3. Sharpen continuously:**
- Recommendations feed back into the curriculum based on what you reflected on, skipped, and noted
- The longer you run it, the sharper the curriculum gets

**Live:** full execution loop: modules, readings, assignments, reflection, rubric, PDF viewer, cloud sync.  
**In progress:** AI Recommendations (§12), Multiple Curricula (§13).  
**Next:** in-app AI curriculum generator.

---

## Status

Live: the execution layer runs end to end on one curriculum. Modules, readings, watching, assignments, reflections, the rubric, skip and restore, notes, the PDF viewer, cloud sync, and dark mode all work.

In progress: multiple curricula (§13) and generating a curriculum with AI in the app. Both are designed in detail below, and each section header says where it stands.

---

<details>
<summary><b>Contents</b></summary>

- [TL;DR](#tldr)
- [Status](#status)
- [1. What this is](#1-what-this-is)
  - [Why each node is designed this way](#why-each-node-is-designed-this-way)
  - [What Canon is not](#what-canon-is-not)
- [2. Who it is for](#2-who-it-is-for)
  - [How I'll know it's working](#how-ill-know-its-working)
- [3. Architecture](#3-architecture)
- [4. Curriculum Structure](#4-curriculum-structure)
- [Technical Reference](docs/technical/ARCHITECTURE.md) *(separate doc)*
- [Changelog](docs/technical/CHANGELOG.md) *(separate doc)*
- [Learning Science](docs/product/LEARNING-SCIENCE.md) *(separate doc)*
- [Competitive Research](docs/product/COMPETITIVE.md) *(separate doc)*
- [Ideation Log](docs/product/IDEATION.md) *(separate doc)*
- [10. What Could Be Built Next](#10-what-could-be-built-next)
- [12. Feature Design: AI Recommendations](#12-feature-design-ai-recommendations)
  - [12.1 Problem](#121-problem)
  - [12.2 Goal](#122-goal)
  - [12.3 What "Personalised" Actually Means](#123-what-personalised-actually-means)
  - [12.4 The Rating Question](#124-the-rating-question)
  - [12.5 Discovery as a Design Principle](#125-discovery-as-a-design-principle)
  - [12.6 Trusted Sources](#126-trusted-sources)
  - [12.7 Technical Architecture](#127-technical-architecture)
  - [12.8 The Prompt Design](#128-the-prompt-design)
  - [12.9 Output UI (Deferred, Design Only)](#129-output-ui-deferred-design-only)
  - [12.10 What NOT to Build in v1](#1210-what-not-to-build-in-v1)
  - [12.11 Open Questions](#1211-open-questions)
  - [12.12 Build Order (when ready)](#1212-build-order-when-ready)
- [13. Feature Design: Multi-Curriculum Platform](#13-feature-design-multi-curriculum-platform)
  - [13.1 Why](#131-why)
  - [13.2 Permissions Model](#132-permissions-model)
  - [13.3 Three Ways to Get a Curriculum into Canon](#133-three-ways-to-get-a-curriculum-into-canon)
  - [13.4 Curriculum Identity](#134-curriculum-identity)
  - [13.5 State Shape Changes](#135-state-shape-changes)
  - [13.6 Migration Strategy](#136-migration-strategy)
  - [13.7 UI Touchpoints](#137-ui-touchpoints)
  - [13.8 Supabase Changes](#138-supabase-changes)
  - [13.9 The Generate Flow (in-app curriculum builder)](#139-the-generate-flow-in-app-curriculum-builder)
- [14. Feature Design: Reading Groups](#14-feature-design-reading-groups)
  - [14.1 Problem](#141-problem)
  - [14.2 Design](#142-design)
  - [14.3 Data Shape](#143-data-shape)
  - [14.4 UI Spec](#144-ui-spec)
  - [14.5 Design Mode](#145-design-mode)
  - [14.6 Build Order](#146-build-order)
  - [13.10 What's Out of Scope for v1](#1310-whats-out-of-scope-for-v1)
  - [13.11 Open Questions](#1311-open-questions)
  - [13.12 Build Order](#1312-build-order)

</details>

---

## 1. What this is

Canon is a personal learning workspace and a curation tool, with an experiment to ground it in cognitive science. The PM curriculum is the one running now.

**As an execution layer:**
- You bring your own curriculum, decide what to skip, and track progress by what you consume and what you produce
- Every module ends in a named artifact
- Curriculum-agnostic by design

**As a curation tool:**
- AI helps you find gold-standard practitioners and primary sources in any field
- The source discovery protocol: which voices to trust, how to go to the originator over the summary, what to exclude, is being developed and refined through active learning
- This is a work in progress

### Why each node is designed this way

Canon draws from the highest-evidence principles in cognitive science. Evidence ratings (H/M) are based on meta-analyses and systematic reviews. Whether each implementation actually achieves these effects in practice is still being tested.

| Principle | Evidence | Key paper | Hypothesis | Canon feature | Canon status |
|-----------|----------|-----------|------------|---------------|-------------|
| **Retrieval practice** | **H** | [Rowland 2014](https://pmc.ncbi.nlm.nih.gov/articles/PMC7889502/) — *Psychological Bulletin* | Testing yourself on material produces far better long-term retention than re-reading it | Evaluate tab: Ask Yourself self-check questions | WIP |
| **Spaced practice** | **H** | [Cepeda et al. 2006](https://doi.org/10.1037/0033-2909.132.3.354) — *Psychological Bulletin* | Spreading study sessions over time produces better retention than massed practice | Pace note banner encourages distributing study over time | To be explored |
| **Generation effect** | **H** | [Bertsch et al. 2007](https://link.springer.com/article/10.3758/BF03193441) — *Memory & Cognition* | Producing information produces better retention and recall than reading or re-reading | Every module ends in a named artifact. Reading alone does not count. | Yes |
| **Autonomy** | **H** | [Bureau et al. 2022](https://pmc.ncbi.nlm.nih.gov/articles/PMC8935530/) — *Psychological Bulletin* | People sustain learning when they control their own path | Skip/restore: decide what to skip and note why. Choose the curriculum, set the pace. | Yes |
| **Competence** | **H** | [Bureau et al. 2022](https://pmc.ncbi.nlm.nih.gov/articles/PMC8935530/) — *Psychological Bulletin* | Perceived effectiveness sustains motivation | Progress bars per module and overall | Yes |
| **Elaborative interrogation** | **M** | [Dunlosky et al. 2013](https://journals.sagepub.com/doi/abs/10.1177/1529100612453266) — *Psych Science in the Public Interest* | Asking "why does this work?" produces better retention than summarising | Reflection tab: open prompt after every module | Yes |
| **Deliberate practice** | **M** *(contested)* | [Macnamara et al. 2014](https://journals.sagepub.com/doi/abs/10.1177/0956797614535810) — *Psychological Science* | Targeting specific weaknesses with feedback improves performance | Evaluate tab: Weak / Strong / Ask Yourself rubric | WIP |

### What Canon is not

- **Not an algorithmic content aggregator:** Canon does not crawl feeds or surface content based on popularity. Discovery is intentional: gold-standard sources curated by field, with AI Recommendations that surface reading based on your own reflections, skip reasons, and notes. The curation methodology is part of what Canon is building.
- **Not a note-taking app:** Notes are scoped to specific readings and modules. They feed the reflection and assignment loops, not a general capture system.
- **Not a centralised course platform:** No prescribed path, no instructor, no certificate. You set the curriculum and change it when your thinking changes.
- **Not a learning community:** No social layer. Nobody else sees your progress or your work. That is the biggest behavioural gap (see §2).
- **Not a portfolio host:** Artifacts live inside Canon while you build them. Publishing them is a separate step.

---

## 2. Who it is for

Canon started as a personal project: one person, one curriculum, one experiment. The pace, what gets skipped and reflected on: all shaped around one person's gaps and habits. But the underlying design is based on learning principles that hold across people: established cognitive science for adult learners. Any PM who wants to curate and execute their own curriculum can use it.

**What the design covers:**

- **Autonomy:** you choose the curriculum, the pace, what to skip. Nothing is prescribed by someone else.
- **Competence:** progress bars, module completion, artifact production. Clear signals that things are moving forward.
- **Generation and retrieval:** every module ends in a named artifact. Ask Yourself questions in the Evaluate tab are a form of retrieval practice.

**What the design does not cover (yet):**

- **Relatedness:** people sustain learning when their work connects to someone they respect or a community they belong to. In Canon, nobody else sees what gets produced. That is the biggest behavioural gap and the hardest to close.
- **Spaced retrieval:** no algorithm surfaces prior material at optimal intervals. The pace note encourages spacing, but the system does not enforce it.

The admin account (`batra.surabhi@gmail.com`) can edit the curriculum, publish, and manage attachments. Multiple curricula and other users are not live yet.

### How I'll know it's working

**North Star:** Modules completed with an artifact produced, not marked done without output.

**Personal growth (Surabhi as designer):**

| Metric | Signal |
|--------|--------|
| Thinking quality | Artifacts in modules 6-13 show different problem framing than module 0. She can name the shift, not just feel it. |
| AI skill acquisition | She can demonstrate context engineering, prompt design, and multi-agent use from building Canon, not just describe them. |
| Applied learning | At least one Canon product decision is traceable to a specific reflection note or module artifact. |

**Tool effectiveness:**

| Metric | Definition | Signal |
|--------|-----------|--------|
| Artifact completion rate | % of completed modules with a non-empty assignment section | Are outputs actually being produced? |
| Reflection fill rate | % of completed modules with non-empty reflection notes | Is elaboration happening, or just checkbox-ticking? |
| Purposeful skip rate | % of skipped items with a reason logged | Are skips deliberate or just abandonment? |
| Curriculum refinement frequency | Design mode used at least once per curriculum pass | Is the curriculum getting sharper over time? |
| Rubric score distribution | Range of Weak / Strong ratings across modules | Honest self-assessment, or grade inflation? |

**Guardrail:** Skip rate overall. If the majority of readings are skipped, the curriculum was poorly curated, not a signal the tool is working.

**Retired:**
- *AI recommendation conversion:* feature not live yet. Revisit when §12 ships.
- *Return cadence:* habit signal, not a learning signal. Not meaningful at N=1.

N=1. No statistical significance possible. The baseline is how I studied before Canon: forty open tabs, plenty started, almost nothing finished, no output.

---

## 3. Architecture

**Single self-contained HTML file.** No build step, no dependencies to install, no framework.

| Layer | Technology |
|---|---|
| UI | Vanilla JS, CSS custom properties, no framework |
| Auth | Supabase email/password |
| Progress sync | Supabase `user_state` table (debounced 1.8s after any change) |
| Canonical curriculum | Supabase `canonical_curriculum` table |
| PDF blobs | IndexedDB (`pm_curriculum_attachments` / `files` store) |
| PDF rendering | PDF.js, lazy-loaded from CDN on first use |
| Local state | `localStorage` key `pm_curriculum_v2` |
| Deployment | GitHub Pages, auto-deploys on push to `main` |

### State shape
```js
{
  curriculum: { title, subtitle, paceNote, modules: [...] },
  progress: {
    [modId]: {
      complete, assignmentDone, reflectionDone,
      readings:   { [rId]: bool },
      watching:   { [wId]: bool },
      rejected:   { readings: { [rId]: bool }, watching: { [wId]: bool } },
      skipReasons: { readings: { [rId]: string }, watching: { [wId]: string } },
      readingNotes: { [rId]: string (HTML) },
      notes, assignmentNotes, reflectionNotes, evaluationNotes,
      assignmentSections: [{ id, title, content, collapsed }]
    }
  },
  attachments: { [modId]: { admin: [{dbId,name,size,addedAt}], student: [...] } },
  mode: 'student' | 'admin',
  selectedModule: string | null,
  activeTab: string,
  darkMode: bool
}
```

---

## 4. Curriculum Structure

**14 modules (0–13).** Spine modules (`spine: true`) are load-bearing, marked with a **Core** pill and referenced in the pace banner.

| # | ID | Title | Spine |
|---|---|---|---|
| 0 | m0 | Mindset & Meta-cognition | ✓ |
| 1 | m1 | Strategic Thinking & Systems Thinking | ✓ |
| 2 | m2 | Design Thinking & Human-Centred Research | |
| 3 | m3 | AI Fundamentals (Rigorous, Non-Technical) | |
| 4 | m4 | AI Tools, Hands On | |
| 5 | mpt | Product Taste & Craft | |
| 6 | m5 | Pricing & Monetization Strategy | |
| 7 | m6 | AI Product Strategy | ✓ |
| 8 | mpr | Prioritization & Roadmapping | |
| 9 | m7 | Execution & Stakeholder Management | |
| 10 | mwc | Written Communication & PM Craft | |
| 11 | m8 | Metrics, Evaluation & North Star Thinking | |
| 12 | m9 | Career Thesis | ✓ |
| 13 | m10 | Build Log & Publishing | |

Each module contains: `title`, `subtitle`, `whyItMatters`, `artifact`, `rubric` (weak/strong/selfCheck), `readings[]`, `watching[]`, `assignment`, `reflection`.

---

> Technical reference (feature spec, key functions, data storage, known issues, admin workflow): [docs/technical/ARCHITECTURE.md](docs/technical/ARCHITECTURE.md)

---

## 10. What Could Be Built Next

**High leverage:**

- **Spaced retrieval of your own thinking.** Surface reflection notes from earlier modules when a later one is relevant. "You wrote this in module 2. What would you say now?" Nobody else has built this.
- **In-app AI curriculum generator.** Currently built outside Canon via a prompt. Close the loop: 5 questions → structured curriculum, generated and loaded in one flow. Master prompt at `prompts/curriculum-generator.md`.
- **Relatedness layer.** Even one other person seeing your artifacts changes behaviour. Hardest behavioural gap. No design yet.
- **Multiple curricula.** The dropdown stub exists. The execution layer is already domain-agnostic. Natural next infrastructure step.

**Later:**

- **Retrieval practice.** Before showing a reading note, ask "what was the main thing you took from this?" One prompt that creates a retrieval loop.
- **Forgetting curve signal.** Surface modules not touched in X weeks. Quiet visual indicator, not a notification.
- **Implementation intentions.** "I will do one reading every Tuesday morning" beats "I will learn every day." A simple schedule commitment in the app.
- **Export progress**, PDF or CSV of completed readings, assignment notes, reflections.
- **Mobile layout**, current layout works but is not optimised for small screens.
- **Module reordering**, drag to reorder in Design mode.
- **Search**, full-text across modules, readings, and notes.
- **Offline PDF.js**, embed library to remove CDN dependency.
- **Per-reading skip reason editing**, currently set at skip time only.
- **Progress recovery safeguard**, auto-backup before any destructive operation.

---

> Changelog: [docs/technical/CHANGELOG.md](docs/technical/CHANGELOG.md)

---

## 12. Feature Design: AI Recommendations

**Status:** Designed, not started yet  
**Last updated:** 2026-06-09

---

### 12.1 Problem

The curriculum is curated at a point in time, for a generic learner. The actual learner has context the curriculum cannot see: what confused them, what they already knew, what they skipped and why, what they wrote in reflection. The canonical reading list cannot adapt to this on its own.

Three things specifically make it go stale:

1. **The field moves.** New tools, new essays, new practitioners emerge constantly. What Marty Cagan wrote in 2018 is different from what he's writing now. A new AI paper ships every week.
2. **The learner is not generalised.** What you already know, what you just reflected on, what you skipped and why, these signals make your ideal next reading completely different from someone else's at the same module.
3. **The admin's own discoveries don't flow back.** Surabhi reads and writes constantly. Good content she finds stays in her head, not in Canon.

A recommendation is only as good as its signal. Canon has richer data than most systems: skip reasons, reflection notes, rubric grades. This feature closes the feedback loop by routing those signals back into curation.

---

### 12.2 Goal

Surface 3–5 high-signal reading/watching directions per module, personalised to **this learner's current state**, on demand. Not a feed. Not a push notification. A button you press when you've done the work and want to go deeper.

**The output isn't a list of URLs.** It's a list of *directions*, author + concept + where to find it. The learner discovers the specific article themselves. This is intentional (see §12.5 on Discovery).

**If designed right, this is also the spaced repetition mechanism.** Cross-module surfacing ("your module 2 reflection mentioned network effects, this module 6 reading is relevant") is interleaving plus spaced retrieval in one feature. That is scoped out of v1 (see §12.10) but is the highest-leverage v2 direction.

---

### 12.3 What "Personalised" Actually Means

Recommendations should be generated from a layered context stack, in rough order of signal quality:

| Signal | What it reveals | Where it lives |
|---|---|---|
| **Reflection notes** | Genuine gaps, confusions, questions that emerged from doing the work | `progress[modId].reflectionNotes` |
| **Skip reasons** | What the learner already knows / found irrelevant | `progress[modId].skipReasons` |
| **Reading notes** | What landed, specific passages, frameworks they're internalising | `progress[modId].readingNotes` |
| **Assignment notes** | Where their thinking is applied, what they're wrestling with | `progress[modId].assignmentNotes` / `assignmentSections` |
| **User-added readings** | Their existing sources, taste, what they're already tracking | readings with `addedBy: 'student'` (or detected by ID pattern) |
| **Completed vs skipped** | Pacing, depth of engagement, what they're running from | `progress[modId].readings`, `.rejected` |
| **Module topic** | The subject domain, what's on-topic vs noise | `mod.title`, `mod.whyItMatters`, `mod.assignment` |
| **Trusted sources (admin-set)** | Preferred authors, blogs, newsletters, surface these first | New field: `state.curriculum.trustedSources[]` |

The reflection notes are the highest-signal input. A learner who writes "I still don't understand how network effects actually compound in a two-sided market" is asking a specific question. That question should drive the recommendation, not just "Strategic Thinking module → suggest Porter's Five Forces again."

**Corollary:** Learners who don't write reflections get generic recommendations. This is intentional. It incentivises the behaviour that builds the moat.

---

### 12.4 The Rating Question

You flagged this: you want feedback on recommendations, but you don't want to lock discovery to certain websites.

**The answer: rate the direction, not the source.**

The feedback question after each recommendation is not "was this a good article?" (which implies you've already read it and rates a URL). It's:

> "Was this direction right for me, right now?"

Three states per recommendation card:
- **Added to reading list**, strongest positive signal. Learner thought it was worth tracking.
- **Not for me right now**, mild negative. Not wrong, just not timely. Could resurface later.
- **No action**, neutral. Maybe they'll look later, maybe they dismissed it.

This avoids:
- Star ratings (which encode nothing useful about fit)
- URL-locking (you never rate a website, you rate a topic direction)
- Survey fatigue (two taps max, never required)

These signals feed back into the next recommendation call for that module as additional context.

---

### 12.5 Discovery as a Design Principle

You said: "always gotta be discoverable." This is the right constraint, and it rules out some obvious approaches:

❌ **Don't pre-fetch specific URLs.** They break, go paywalled, get deleted. Worse, they make Canon feel like a directory, not a thinking tool.

❌ **Don't integrate a search API in v1.** Tavily, Exa, Perplexity, these add infrastructure, API costs, and rate limits. They also return noisy results. Not worth it until the recommendation format is proven.

✅ **Output search directions, not URLs.** A recommendation reads: *"Ben Thompson's recent Stratechery pieces on vertical AI integration, search 'Ben Thompson Stratechery AI 2024/2025' or go to stratechery.com."* The learner clicks, searches, and discovers. The act of discovery is part of the learning.

✅ **Match the existing reading item format.** Recommended items look like existing reading items, `title`, `description`, `searchTerm`. They slot naturally into the UI. Adding one to your list feels the same as checking a box.

✅ **Surface trusted sources preferentially.** If Surabhi has registered her own blog, SVPG, Lenny's Newsletter, the recommendation engine tries to find relevant content *there* first before suggesting new sources.

---

### 12.6 Trusted Sources

Admin-configurable list of preferred authors, blogs, publications. Stored in `state.curriculum.trustedSources` (array of strings). Examples:

```
- Surabhi Batra (substack.com/surabhi or wherever)
- SVPG / Marty Cagan (svpg.com)
- Lenny Rachitsky (lennysnewsletter.com)
- Shreyas Doshi (twitter/substack)
- Ben Thompson (stratechery.com)
- a16z (a16z.com)
```

The recommendation prompt instructs the AI to check these sources first. If a trusted source has covered the topic relevantly, it should surface before generic suggestions.

This is also how "blogs Surabhi writes" gets incorporated. She adds her own blog as a trusted source. The AI can suggest her own writing as a recommendation when it's relevant, which also functions as a curriculum annotation ("you actually wrote about this, worth re-reading your own take").

---

### 12.7 Technical Architecture

**The core question:** where does the AI call happen?

**Option A, Client-side API call (v1 recommendation)**
- User stores their Anthropic API key in Canon settings (localStorage only, never sent to Supabase)
- The app calls Claude API directly from the browser
- Zero new infrastructure. Works today.
- Suitable for a single user or small group where each person provides their own key
- Security tradeoff: key is in browser localStorage (acceptable for a personal tool; not for a public SaaS)

**Option B, Supabase Edge Function (v2, multi-user)**
- Supabase Edge Function proxies the Claude API call
- API key is server-side only
- Correct for a product with real students
- Requires deploying a Deno edge function, modest but real infrastructure

**Recommendation: build v1 with Option A.** Get the UX and prompt right first. Migrate to Option B when Canon has paying users.

---

### 12.8 The Prompt Design

The prompt sent to Claude should be structured, not open-ended. Roughly:

```
You are helping a learner working through a PM curriculum.

MODULE: {{mod.title}}
MODULE CONTEXT: {{mod.whyItMatters}}
ASSIGNMENT BRIEF: {{mod.assignment | first 300 chars}}

WHAT THEY'VE ALREADY READ (don't re-recommend):
{{completed readings, titles only}}

WHAT THEY SKIPPED AND WHY:
{{rejected readings + skipReasons}}

THEIR REFLECTION ON THIS MODULE:
{{reflectionNotes | if present}}

THEIR READING NOTES (what landed):
{{top 3 reading notes by length | if present}}

THEIR OWN ADDITIONS TO THE CURRICULUM:
{{student-added readings, titles}}

PREVIOUS RECOMMENDATIONS THEY DISMISSED:
{{dismissed recs from prior calls | if present}}

TRUSTED SOURCES TO PRIORITISE:
{{trustedSources list}}

Return exactly 4 recommendations as JSON:
[{
  "title": "Author / Publication, Concept (approx date if known)",
  "description": "2 sentences: what it covers and why it's the right next read for this learner specifically, given their reflection.",
  "searchTerm": "specific search query to find it",
  "why": "1 sentence: why THIS learner, not a generic learner, needs this right now"
}]

Rules:
- Do not repeat anything already in their reading list
- Prioritise trusted sources where relevant
- At least one recommendation should address a gap or confusion mentioned in their reflection
- If their reflection is empty, return more general but still module-specific suggestions
- Search terms should be specific enough to find the right content within 3 results
```

The `why` field is shown in the UI as the personalisation hook, "Here's why this is for you." This makes generic recommendations feel personal even when the signal is thin.

---

### 12.9 Output UI (Deferred, Design Only)

Not speccing the UI in detail yet. Broad strokes:

- **Entry point:** A `✦ Get recommendations` button in the Reading tab header (not a persistent tab, on-demand only)
- **Loading state:** Spinner with message "Reading your reflections…", surfaces that it's using their data
- **Result panel:** Slides in below the existing reading list. 4 cards. Each card: title, description, `why` personalisation note, `+ Add to reading list` / `Not for me` actions
- **Added items:** Slot into the reading list as a new section "Suggested", visually distinct (lighter pill, different border) but functionally identical. Can be checked off, skipped, noted.
- **Privacy disclosure:** One-time tooltip on first use: "Your reflection notes and skip reasons are sent to the AI to personalise recommendations. Nothing is stored externally."

---

### 12.10 What NOT to Build in v1

- ❌ Real-time web search integration (Tavily, Exa, Perplexity), adds infrastructure before the format is proven
- ❌ Watching recommendations, video content is harder to describe by search term. Start with reading only.
- ❌ Cross-module recommendations for v1. Scoped out to prove the per-module format first. But surfacing "your module 2 reflection mentioned network effects, this module 6 reading is relevant" is interleaving plus spaced retrieval in one feature. Highest-leverage v2 addition (see §12.2).
- ❌ Automatic/scheduled refresh, this must always be a deliberate user action, not a feed
- ❌ Social/shared recommendations, what other Canon users found useful. Privacy concern, infrastructure concern, not in scope

---

### 12.11 Open Questions

1. **API key UX.** Where does the user enter their Anthropic key? Settings modal? First-use prompt when they click the button? The latter is lower friction but feels abrupt.

2. **What if reflection is empty?** The module-topic signal alone produces generic recommendations. Is that OK, or should the button be gated ("Write a reflection first to unlock recommendations")? The gate incentivises reflection but adds friction. Leaning toward: show the button always, but surface a nudge if reflection is blank ("Add a reflection to get personalised picks").

3. **How often can you call it?** Once per session? Unlimited? Each call costs API credits. For a personal tool this is fine. For a multi-user product, rate limiting matters.

4. **Do dismissed recommendations persist?** If you click "Not for me" and later refresh, should those items stay dismissed? Probably yes, store in `progress[modId].dismissedRecs: [{ title, searchTerm }]` so the next call doesn't re-surface them.

5. **Freshness signal.** Claude's knowledge has a cutoff. How do you tell it to prioritise recent content? The prompt can say "prefer 2024–2025 content where possible" but it can't verify this. For true freshness, you need a search API. Acceptable tradeoff for v1.

---

### 12.12 Build Order (when ready)

1. Settings modal with API key field (stored in localStorage only)
2. `getRecommendations(modId)` function, assembles context, calls Claude API, parses JSON response
3. `renderRecommendations(recs, modId)`, the result panel UI
4. Add-to-list + dismiss actions
5. Trusted sources field in admin curriculum settings
6. Privacy disclosure tooltip
## 13. Feature Design: Multi-Curriculum Platform

**Status:** In progress
**Last updated:** 2026-06-10

---

### 13.1 Why

The execution layer is domain-agnostic. Reflections, rubrics, assignments, skip logic, progress tracking: none of it is specific to PM. The PM curriculum is just the first one running.

Once any curriculum can plug into that layer, Canon works for learning anything.

---

### 13.2 Permissions Model

Two tiers. Simple. Binary.

| User type | What they can do |
|---|---|
| **Any signed-in user** | Execute any curriculum they have access to. Upload a private curriculum (.md or JSON). Generate a private curriculum with AI. See the public library and add a featured curriculum to their collection. |
| **Admin** (`batra.surabhi@gmail.com`) | Everything above, plus: publish a curriculum to the public library, making it available to all users. |

**Privacy is absolute:** no user can see another user's curriculum (if private) or their progress. Ever.

---

### 13.3 Three Ways to Get a Curriculum into Canon

#### 1. Featured Library (Canon-curated, admin-published)
A small set of high-quality curricula published by Canon's admin. Currently one: **PM, Sharpened**. Visible to all signed-in users. Browsable, previewable, addable to personal collection with one click. Canon's quality bar, these are the showcase.

#### 2. Upload Your Own
Any signed-in user uploads a `.md` file or a `.json` file matching the curriculum schema. The parser validates it, gives it a generated ID, and adds it to the user's private collection. Nobody else can see it. The `.md` parser already exists (`parseMarkdown`). JSON import is new.

#### 3. Generate with AI
An in-app guided form (5–7 questions: domain, level, goals, learning style, time horizon). Calls Claude API. Returns a structured curriculum JSON matching the Canon schema. User reviews the full structure before activating it. Private. The master prompt already exists at `prompts/curriculum-generator.md`.

---

### 13.4 Curriculum Identity

Every curriculum needs a stable ID. Currently there is none. Proposed:

| Source | ID format | Example |
|---|---|---|
| Canon-curated | `canon-{slug}` | `canon-pm-2026` |
| User upload | `user-{timestamp}` | `user-1749234567890` |
| AI-generated | `gen-{timestamp}` | `gen-1749234567890` |

The title "PM, Sharpened" maps to `canon-pm-2026`.

---

### 13.5 State Shape Changes

This is the most significant technical change. Current state assumes one curriculum. New state supports many.

**Current shape:**
```js
{
  curriculum: { title, modules: [...] },
  progress: { [modId]: { ... } },
  attachments: { [modId]: { ... } }
}
```

**New shape:**
```js
{
  curricula: [
    { id, title, subtitle, paceNote, source, isPublic, modules: [...] }
  ],
  activeCurriculumId: 'canon-pm-2026',
  progress: {
    'canon-pm-2026':  { [modId]: { ... } },
    'user-123456789': { [modId]: { ... } }
  },
  attachments: {
    'canon-pm-2026':  { [modId]: { admin: [], student: [] } },
    'user-123456789': { [modId]: { admin: [], student: [] } }
  },
  darkMode: bool
}
```

All existing functions that reference `state.curriculum` and `state.progress[modId]` need to resolve via `activeCurriculumId`. The active curriculum is a derived pointer, not a separate data copy.

---

### 13.6 Migration Strategy

Existing users have state in the old single-curriculum shape. On load, detect old shape and migrate automatically:

```js
// On loadState(), detect and migrate
if (state.curriculum && !state.curricula) {
  const legacyId = 'canon-pm-2026';
  state.curricula = [{ id: legacyId, ...state.curriculum }];
  state.activeCurriculumId = legacyId;
  state.progress    = { [legacyId]: state.progress    || {} };
  state.attachments = { [legacyId]: state.attachments || {} };
  delete state.curriculum;
  saveState(); // write migrated shape immediately
}
```

This is a one-time migration. Existing progress is preserved under `canon-pm-2026`. No data loss.

---

### 13.7 UI Touchpoints

#### A. Header, Curriculum Switcher
The dropdown stub already exists. Make it real.

- Shows active curriculum name + caret (current behaviour)
- Dropdown: list of user's curricula (active one checked)
- Bottom of list: **"+ Add a curriculum →"** CTA
- Clicking a curriculum switches `activeCurriculumId` and re-renders dashboard

#### B. "Add a Curriculum" Flow
Three-option screen (modal or inline panel):

```
┌─────────────────────────────────────────────────┐
│  Add a curriculum                               │
│                                                 │
│  ✦ Browse featured        Canon-curated, free   │
│  ↑ Upload your own        .md or .json file     │
│  ✱ Generate with AI       Answer 5 questions    │
└─────────────────────────────────────────────────┘
```

#### C. Featured Library
A simple browsable list of Canon-curated curricula. Each card: title, subtitle, module count, who it's for. "Add to my curricula" button. No progress shown (that's personal). Preview shows module list only.

Currently one item: **PM, Sharpened**, 14 modules, artifact-first, for PM career returners.

#### D. Dashboard, Active Curriculum Context
Small curriculum name badge above the pace note banner, so the user always knows which curriculum they're executing. Clicking it opens the switcher.

#### E. Design Mode, Admin Publishing
Admin-only: a "Publish to Library" button that takes the current active curriculum and writes it to Supabase as a public featured curriculum. Replaces the current single "Publish" flow. Admin can manage multiple published curricula.

---

### 13.8 Supabase Changes

| Current | New |
|---|---|
| `canonical_curriculum`, single row | `featured_curricula`, one row per published curriculum, with `id`, `title`, `data` (JSON), `published_at` |
| `user_state`, one row per user, with `curriculum` + `progress` | `user_state`, one row per user, with `curricula` (array) + `progress` (keyed by curriculum ID) + `activeCurriculumId` |

The `featured_curricula` table replaces `canonical_curriculum`. A migration script converts the existing single row to the new format on first deploy.

---

### 13.9 The Generate Flow (in-app curriculum builder)

Uses the Claude API (same API key as recommendations feature, set in Settings). Guided form, not open-ended:

1. **Domain**, what subject / skill area?
2. **Level**, beginner, intermediate, advanced, re-entry?
3. **Goal**, what should the learner be able to do at the end?
4. **Time horizon**, how many weeks? How many hours per week?
5. **Preferred format**, more reading, more watching, more doing?
6. **Prior knowledge**, what can we skip?
7. **One constraint**, what must be in this curriculum no matter what?

Outputs: full curriculum JSON in Canon schema. User sees the module list before activating. Can regenerate if not satisfied. On activation, added to private collection and set as active.

---

### 13.10 What's Out of Scope for v1

- ❌ Sharing a private curriculum with another user (makes privacy harder)
- ❌ Public user-generated curriculum directory (quality control problem)
- ❌ Collaborative curriculum editing (multiple admins on one curriculum)
- ❌ Curriculum versioning / update propagation for user-uploaded curricula
- ❌ Paid/gated featured curricula (business model question, not yet)
- ❌ Curriculum ratings or social proof

---

### 13.11 Open Questions

1. **What happens to progress when a user re-imports a newer version of a curriculum they already have?** Module IDs may have changed. Progress could orphan. Proposal: keep old progress as-is, treat the new import as a separate curriculum ID with a version suffix.

2. **Can a user have the same featured curriculum in their collection more than once?** No, check by curriculum ID before adding.

3. **Does the generate flow need the API key to already be set?** Yes, prompt the user to add their Anthropic API key in Settings before opening the generate flow. Same gating as recommendations.

4. **What is the maximum number of curricula a user can have?** No hard limit for now. Practically bounded by localStorage size. Worth watching.

---

### 13.12 Build Order

1. **State shape migration**, `loadState()` detects old shape and migrates. All functions updated to resolve via `activeCurriculumId`. No UI change yet. Foundational.
2. **Curriculum ID assignment**, assign `canon-pm-2026` to the existing CANONICAL_CURRICULUM. All module IDs stay the same.
3. **Header switcher**, make the dropdown stub real. Switch curricula, re-render.
4. **"Add a curriculum" modal**, three-option entry point.
5. **Upload flow**, .md and .json import, available to all users (not just admin).
6. **Featured library**, Supabase `featured_curricula` table, browse + add flow.
7. **Dashboard curriculum badge**, small active curriculum label above pace note.
8. **Admin: publish to library**, updated publish flow for multi-curriculum.
9. **Generate flow**, last, because it depends on API key settings (shared with recommendations).

---

## 14. Feature Design: Reading Groups

**Status:** Specced. Not built.

---

### 14.1 Problem

As modules grow, readings accumulate from different angles. Module 0 now has 8 readings: some about metacognition, some about first principles reasoning, some about how to study. They appear as a flat numbered list. There is no signal about which ones belong together or what to work through first.

A flat list of 8 is navigable. A flat list of 12 is not.

The hypothesis: if readings are grouped into named sections, students can approach a module with more intention. They can choose a cluster, work through it, and return. The module feels navigable rather than exhausting.

---

### 14.2 Design

No new entity. No "submodule" data type. Just a `group` field on each reading (and optionally watching item) — a short string label. Items with the same label are visually grouped under a shared section header in the Reading tab.

This is the lightest change that captures the intent. The alternative (proper submodule entities with their own IDs, completion states, and UI) would add significant complexity for a benefit that doesn't require it at the current scale.

The grouping is curatorial, not structural. It does not change how progress is calculated. It does not add a new layer of completion. It just makes the reading list scannable.

---

### 14.3 Data Shape

One new optional field on readings and watching items:

```js
// readings item (watching item: same)
{
  id, number, title, url, searchTerm, type, description,
  group: "How to Think"  // optional; omit or empty string = ungrouped
}
```

No schema migration needed. Items without a `group` field continue to render normally (ungrouped, at the top or bottom, designer's choice).

Module 0 example groupings:

| Group | Readings |
|-------|---------|
| How to Think | Feynman technique, Cook and Chef (Tim Urban), First Principles (Farnam Street) |
| Metacognition | Growth mindset (Dweck), learning how to learn material |
| Decision Quality | Anything in the Annie Duke / cognitive bias cluster |

---

### 14.4 UI Spec

**Reading tab (student view):**

- When two or more readings share a group label, a section divider appears above the first item in the group.
- Divider: small caps label, muted color, thin rule. Example: `HOW TO THINK`.
- Items within a group render as normal (checkbox, title, skip button, etc.).
- Ungrouped items appear first (or last — TBD on first build).
- Group headers are not interactive. No group-level completion tracking.
- Progress bar and `getModPct()` are unchanged. Groups are presentation only.

**Watching tab:** same pattern if `group` field is present.

---

### 14.5 Design Mode

**Inline reading editor (per reading row):**

- Add a `Group` input field to each reading row (small, right-aligned, or below the description).
- Placeholder: `Group (optional)`.
- Previously used group names within the same module surface as a `<datalist>` for autocomplete — reduces typos between readings that should share the same group.
- No separate group management UI. Groups are just strings; if you rename one, you change each reading manually.

**Reading editor layout change:**

The existing row layout is: `[emoji] [title input] [url input] [✕]`. The group field fits below or as a third input before the URL. Exact layout is design's call on first build.

---

### 14.6 Build Order

1. **Add `group` field to CANONICAL_CURRICULUM for Module 0.** Assign groups to the 8 readings as a proof of concept. No UI change yet.
2. **Reading tab renders group headers.** When `group` is present, group items and render `<h4>`-style dividers. No Design mode change.
3. **Design mode: add group input per reading.** `<datalist>` autocomplete using existing group names in the module.
4. **Watching tab:** apply same grouping logic if watching items have `group` field.

Step 1 alone is a 5-minute data change. Steps 2 and 3 are the real build. Step 4 is optional in v1.

---

