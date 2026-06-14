# Canon: Product Requirements Document

**Last updated:** 2026-06-14  
**Status:** Living document. I update it after any significant feature change.  
**Owner:** Surabhi Batra  
**Live URL:** https://surabhibatra29.github.io/canon/curriculum-tracker.html  
**Codebase:** single self-contained file, `curriculum-tracker.html`

---

## Status

Live: the execution layer runs end to end on one curriculum. Modules, readings, watching, assignments, reflections, the rubric, skip and restore, notes, the PDF viewer, cloud sync, and dark mode all work.

In progress: multiple curricula (§13) and generating a curriculum with AI in the app. Both are designed in detail below, and each section header says where it stands.

---

## 1. What this is

Canon is a workspace I built for myself to work through a curriculum and produce proof that I understood it. Most learning tools help you take in content. Canon is about the output. Every module ends in a named artifact, and the reflection layer is mine alone.

The PM curriculum is the one I am running now. The execution layer underneath it is not specific to PM, which is what makes running more than one curriculum a natural next step.

---

## 2. Who it is for

Me. I built it, I curate the curriculum, and I am the only user. Everything is shaped around how I actually study, and it changes when my habits do.

The admin account (`batra.surabhi@gmail.com`) is special-cased in code via `_isAdmin()`. It can edit the curriculum, publish, and manage PDF attachments. That structure is what would make multiple curricula, and one day other people, possible. Neither is live yet.

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

## 5. Feature Inventory

### 5.1 Authentication
- Supabase email/password sign in / create account
- Session checked on load; graceful local-only fallback if Supabase CDN fails
- Sync dot on avatar: idle / syncing (amber) / synced (green)
- Admin account (`batra.surabhi@gmail.com`) detected via `_isAdmin()`

### 5.2 Dashboard (Learn mode)
- **Pace note banner**, references spine module numbers, auto-generated from `paceNote` field
- **Progress bar**, overall completion % across all active (non-rejected) items
- **Module card grid**, `auto-fill, minmax(320px, 1fr)`, each card shows: module number, title, Core pill (if spine), status badge, per-module progress bar
- Click any card → opens detail panel

### 5.3 Module Detail Panel (7 tabs)

#### Reading tab
- Items sorted: **todo → done → skipped** (stable sort within each bucket, preserving curriculum order)
- Each item: checkbox + **reading-title-row** (title + link pill + PDF pill + note chip, all inline, flex-wraps on mobile) + collapsed description + skip button
- **Description collapse:** truncated to 100 chars with in-place `more` / `less` toggle
- **Link pills:** `✅ Open link ↗` (verified) or `🔍 search term` (search pill)
- **PDF pills:** `📄 Open PDF` (admin attachment) / `📄 My copy` (student upload)
- **Note chip:** 
  - Done + no note → ghost `✏ note` button (hidden, appears on row hover)
  - Done + has note → `💬 [40-char plain-text preview]` chip (always visible)
  - Clicking either opens the note modal
  - Only shown on done & non-rejected items
- **Skip flow:** clicking `✕ Skip` opens inline prompt (text input + Skip/Cancel). Reason is optional. Saved to `skipReasons`. Displayed as italic `↩ reason` on the faded rejected item. `↩ Restore` clears reason and moves item back to top bucket.
- Rejected items: faded (opacity 0.4), strikethrough title, `↩ Restore` button

#### Watching tab
- Same sort order (todo → done → skipped) and skip/restore flow as Reading
- Items: `▶ Search YouTube` (searchTerm) and/or `✅ Watch directly ↗` (direct url), both can coexist
- 7–9 items per module covering: Lenny Rachitsky podcast, Shreyas Doshi talks, Y Combinator lectures, Exponent mock interviews (topic-matched per module), Julie Zhuo, Marty Cagan, Brian Balfour, a16z, Lex Fridman AI interviews

#### Assignment tab
- **Artifact goal banner** at top (the named deliverable for this module)
- Rich-text sections: draggable (HTML5 drag-and-drop), collapsible, title editable inline
- **Cite readings** button per section, inserts a cited link to a reading
- `+ Add section` button
- **Mark assignment done** checkbox (contributes to module progress %)
- Auto-saves on every input

#### Reflection tab
- Reflection prompt displayed (from module data)
- Textarea, auto-saves, stores in `reflectionNotes`

#### Evaluate tab
- Rubric blocks: **Weak** (amber), **Strong** (green), **Ask Yourself** (purple)
- Self-evaluation textarea, auto-saves, stores in `evaluationNotes`

#### Attachments tab
- Two zones: **Module Resources** (admin PDFs) and **My Files** (student uploads)
- Drag & drop upload; files stored as blobs in IndexedDB, metadata in localStorage/Supabase
- PDF viewer opens on click (PDF.js canvas renderer, no iframes)

#### Notes tab
- General scratchpad per module, auto-saves, stores in `notes`

### 5.4 Reading Note Modal
- Opens via note chip click or `openNoteModal(modId, rId)`
- Header: module + reading title
- Toolbar: **B** (bold), *I* (italic), 1. (ordered list), • (unordered list)
- Body: `contenteditable` div, `Ctrl/Cmd+B/I` keyboard shortcuts
- Saves HTML to `state.progress[modId].readingNotes[rId]` on close
- Legacy plain-text/markdown notes auto-detected and rendered via `renderNoteMarkdown()`
- Chip updates in-place (no re-render) via `outerHTML` replacement

### 5.5 PDF Viewer
- PDF.js loaded lazily from CDN on first use
- Renders all pages as `<canvas>` elements inside full-height modal
- `⬇ Download` button in header
- Graceful fallback to download link if PDF.js CDN is unreachable

### 5.6 Header
- **Logo** (left), clicking returns to dashboard
- **Curriculum name + caret** (center), dropdown shows current curriculum, `+ New` option (placeholder)
- **Learn / Design mode toggle** (right)
- **Dark mode toggle** 🌙, persisted to state
- **Avatar**, shows user initials, click for dropdown (email + Sign out)
- **Sync dot** on avatar, amber while syncing, green when synced, hidden when idle

### 5.7 Progress Calculation
`getModPct(mod)`, counts: readings (excluding rejected), watching (excluding rejected), assignment, reflection. Returns 0–100. Module card shows per-module bar. Dashboard shows overall %.

### 5.8 Design (Admin) Mode
- **Curriculum meta**, edit title, subtitle, paceNote inline
- **Module list**, add, edit, delete modules via form
- **Inline reading editor**, add/edit/delete readings, attach admin PDF per reading
- **Attachments modal** per module, upload/manage admin PDF resources
- **Upload `.md` file** → `parseMarkdown()` parses curriculum markdown format into data structure
- **↺ Reset to Canon**, resets curriculum to embedded `CANONICAL_CURRICULUM` constant. **Does NOT touch progress** (bug fixed 2026-06-04).
- **Publish**, writes `state.curriculum` to Supabase `canonical_curriculum` table; all non-admin users receive it on next login

### 5.9 Cloud Sync
- `_pushToCloud()`, debounced 1.8s, upserts `{ curriculum, progress, attachments_meta }` to `user_state` table
- `_syncFromCloud()`, on login: restores progress from user's own row, curriculum from canonical table (non-admin) or own row (admin)
- Admin always gets their own curriculum; students always get canonical

### 5.10 Dark Mode
- CSS custom properties toggled via `data-theme="dark"` on `<html>`
- Persisted in `state.darkMode`
- Full dark mode coverage for all UI components

---

## 6. Key Functions Reference

| Function | Purpose |
|---|---|
| `render()` | Top-level re-render dispatcher |
| `renderStudent()` | Dashboard + detail panel |
| `renderDetail(mod)` | 7-tab detail panel |
| `renderAdmin()` | Design mode layout |
| `buildNoteChip(modId, rId, note)` | Returns compact note chip HTML (ghost or preview) |
| `toggleDesc(btn)` | In-place description expand/collapse |
| `showSkipPrompt(modId, type, id, btn)` | Replaces skip button with inline reason prompt |
| `confirmSkip(modId, type, id)` | Saves reason + marks rejected |
| `cancelSkip(modId, type, id, el)` | Restores skip button |
| `toggleRejected(modId, type, id)` | Restore flow, clears skip reason |
| `getModPct(mod)` | Progress % (excludes rejected) |
| `openNoteModal(modId, rId)` | Opens WYSIWYG note editor modal |
| `closeNoteModal()` | Saves note HTML, updates chip in-place |
| `viewAttach(dbId, name)` | Opens PDF in canvas modal |
| `uploadFiles(modId, role, files)` | Stores PDFs in IndexedDB, updates metadata |
| `parseMarkdown(md)` | Parses .md curriculum file to data object |
| `loadPdfJs()` | Lazy-loads PDF.js from CDN |
| `saveState()` / `loadState()` | localStorage + debounced cloud sync |
| `publishCurriculum()` | Admin: writes curriculum to Supabase |
| `loadDefault()` | Admin: resets curriculum to CANONICAL_CURRICULUM (safe, never touches progress) |
| `_syncFromCloud()` | Login sync: restores progress + curriculum |
| `_pushToCloud()` | Debounced upsert to Supabase user_state |

---

## 7. Data Storage

| What | Where | Key/Table |
|---|---|---|
| All state (curriculum + progress + metadata) | localStorage | `pm_curriculum_v2` |
| Progress + curriculum (cloud) | Supabase | `user_state` |
| Canonical curriculum | Supabase | `canonical_curriculum` |
| PDF file blobs | IndexedDB | `pm_curriculum_attachments` / `files` |

---

## 8. Known Issues & Gotchas

- **Supabase SDK from CDN**, if CDN fails, app runs local-only (no auth, no sync)
- **PDF.js from CDN**, requires internet on first PDF open; falls back to download link
- **localStorage ~5–10MB limit**, only metadata stored there; blobs in IndexedDB
- **Preview tool timing**, click simulation fires before async functions resolve; always use 500ms delay in `preview_eval`
- **`let` variables not on `window`**, invisible to `preview_eval` but accessible within the script closure
- **After CANONICAL_CURRICULUM changes:** must do `localStorage.removeItem('pm_curriculum_v2'); window.location.reload()` in dev to pick up new data, then bypass auth: `document.getElementById('authScreen').style.display='none'; render()`
- **Admin publish workflow:** Reset to Canon → Publish (Reset is now safe and does not wipe progress as of 2026-06-04 fix)

---

## 9. Admin Publish Workflow

When `CANONICAL_CURRICULUM` JS changes:
1. `git push` → GitHub Pages deploys in 1–3 min
2. Open live URL in incognito (or `Cmd+Shift+R` + `?v=N` to bust CDN)
3. Sign in as `batra.surabhi@gmail.com`
4. **Design mode → ↺ Reset to Canon** (safe, progress untouched)
5. **Publish** → pushes to Supabase `canonical_curriculum`

---

## 10. What Could Be Built Next

- **AI curriculum generator**, in-app form (context fields → generate curriculum JSON via Claude). Master prompt lives at `prompts/curriculum-generator.md`.
- **Export progress**, PDF or CSV of completed readings, assignment notes, reflections
- **Mobile layout**, current layout works but isn't optimised for small screens
- **Module reordering**, drag to reorder in Design mode
- **Search**, full-text search across modules, readings, and notes
- **Offline PDF.js**, embed library to remove CDN dependency
- **Multiple curricula**, the dropdown exists; the switching logic is a stub
- **Per-reading skip reason editing**, currently set at skip time only; no way to edit reason later
- **Progress recovery safeguard**, auto-backup of progress to a secondary Supabase column before any destructive operation

---

## 11. Changelog (recent)

| Date | Change |
|---|---|
| 2026-06-04 | **Bug fix:** `loadDefault()` no longer wipes `state.progress` |
| 2026-06-04 | Watching lists expanded to 7–9 items per module (Lenny, Shreyas, Exponent mock interviews, YC, etc.) |
| 2026-06-04 | Skip reason feature: inline prompt on skip, reason stored + displayed on rejected items |
| 2026-06-04 | Reading/watching sort order: todo → done → skipped |
| 2026-06-04 | Reading list IA: note chip (compact, hover ghost), inline link pills, description collapse |
| 2026-06-04 | Master prompt (`prompts/curriculum-generator.md`) updated: min 7 watching items, mock interview required per module |
| 2026-06-08 | PDF viewer: scroll position preserved on zoom; last-read page remembered per PDF via `pdf_pos_<id>` in localStorage |
| 2026-06-08 | URL hash navigation: refresh / back button restores module + tab (`#modId/tabName`) |
| 2026-06-09 | Added Cagan 'Commercial vs Internal Products' to Product Taste & Craft (mptr6) |

---

## 12. Feature Design: AI Recommendations

**Status:** Designed, not started yet  
**Last updated:** 2026-06-09

---

### 12.1 Problem

The curriculum is a snapshot. It was curated at a point in time, by one person, for a generalised learner. Three things make it go stale fast:

1. **The field moves.** New tools, new essays, new practitioners emerge constantly. What Marty Cagan wrote in 2018 is different from what he's writing now. A new AI paper ships every week.
2. **The learner is not generalised.** What you already know, what you just reflected on, what you skipped and why, these signals make your ideal next reading completely different from someone else's at the same module.
3. **The admin's own discoveries don't flow back.** Surabhi reads and writes constantly. Good content she finds stays in her head, not in Canon.

The canonical reading list can't solve these. It's a floor, not a ceiling.

---

### 12.2 Goal

Surface 3–5 high-signal reading/watching directions per module, personalised to **this learner's current state**, on demand. Not a feed. Not a push notification. A button you press when you've done the work and want to go deeper.

**The output isn't a list of URLs.** It's a list of *directions*, author + concept + where to find it. The learner discovers the specific article themselves. This is intentional (see §12.5 on Discovery).

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
- ❌ Cross-module recommendations ("based on Module 3, you might want to read X in Module 5"), scope creep. One module at a time.
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

Right now Canon has one curriculum baked in. I want to run more than one, because the execution layer is the interesting part, not the PM content specifically. Reflections, rubrics, assignments, skip logic, the PDF viewer, progress tracking: all of it works for any subject. The PM curriculum just happens to be the one I am running first.

Once any curriculum can plug into that layer, the same tool works for learning anything.

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

