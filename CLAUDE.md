# Canon — PM Curriculum Tracker

## What this is
A single-file HTML/CSS/JS curriculum tracker app. Pre-loaded with an 11-module AI-era PM curriculum. Users sign in via Supabase to sync progress across devices. Admin account (`batra.surabhi@gmail.com`) can publish a canonical curriculum that all users receive on next login.

## File location
```
/Users/surabhibatra/Claude Code/curriculum-tracker.html
```
Single self-contained file. No build step, no dependencies to install. Open directly in a browser or serve with the local Python server (see below).

## How to run locally
```bash
cd "/Users/surabhibatra/Claude Code"
python3 -m http.server 3456
# then open http://localhost:3456/curriculum-tracker.html
```
A `.claude/launch.json` is already configured for the preview panel.

## Architecture
**Single HTML file** with embedded CSS and JS. No frameworks, no bundler.

### Storage
| What | Where |
|------|-------|
| Curriculum structure + progress metadata + attachment metadata | `localStorage` key `pm_curriculum_v2` |
| Cloud copy of progress (signed-in users) | Supabase `user_state` table |
| Canonical curriculum (published by admin) | Supabase `canonical_curriculum` table |
| PDF file blobs | IndexedDB database `pm_curriculum_attachments`, store `files` |

### Key data shapes
```js
// localStorage / Supabase user_state
{
  curriculum: {
    title, subtitle, paceNote,
    modules: [{
      id, number, title, subtitle, whyItMatters, artifact,
      spine,        // bool — marks load-bearing "Core" modules
      rubric: { weak, strong, selfCheck },  // Evaluate tab content
      readings: [{ id, number, title, url, searchTerm, type, description }],
      watching: [{ id, title, url, searchTerm, description }],  // url is optional direct link
      assignment, reflection
    }]
  },
  progress: {
    [modId]: {
      complete, assignmentDone, reflectionDone,
      readings:   { [id]: bool },
      watching:   { [id]: bool },
      rejected:   { readings: { [id]: bool }, watching: { [id]: bool } },
      notes, assignmentNotes, reflectionNotes, evaluationNotes,
      assignmentSections: [{ id, title, content, collapsed }]
    }
  },
  attachments: { [modId]: { admin: [{dbId, name, size, addedAt}], student: [{dbId, name, size, addedAt}] } }
}

// IndexedDB record
{ id (auto), modId, role ('admin'|'student'), name, data: Blob }
```

## Features

### Auth
- Supabase email/password sign in / create account
- Session checked on load; if Supabase SDK fails (offline/CDN) → local-only mode
- Progress auto-syncs to cloud 1.8s after any change (debounced)
- Sync dot in header: idle / syncing / synced

### Student mode
- Pace note banner + progress bar on dashboard
- **Core** pill on spine modules (m0, m1, m6, m9)
- Module cards grid with status badges and per-module progress bars
- Click any module → detail panel with tabs:
  - **Reading** — checkboxes, verified links, search pills, **✕ Skip** button per item (struck-out + excluded from progress; ↩ Restore any time)
  - **Watching** — same skip/restore, direct "Watch directly ↗" link if `url` present, YouTube search fallback
  - **Assignment** — artifact goal banner, rich-text sections (draggable, collapsible, cite readings), mark done
  - **Reflection** — reflection prompt + notes textarea (auto-saves)
  - **Evaluate** — rubric with Weak / Strong / Ask Yourself blocks + self-evaluation textarea (auto-saves)
  - **Attachments** — Module Resources (admin PDFs) + My Files (student uploads, drag & drop)
  - **Notes** — general scratchpad (auto-saves)
- Mark module complete button
- Rejected items excluded from `getModPct()` calculation

### Admin (Design) mode
- Upload `.md` file → parses and replaces curriculum
- Add / Edit / Delete modules (form-based)
- Inline reading editor with per-reading PDF attachments
- 📎 Attachments modal per module for admin PDF resources
- **Publish** button → writes canonical curriculum to Supabase; all students receive it on next login
- Reset to CANONICAL_CURRICULUM button

### PDF viewing
- Uses **PDF.js** (loaded lazily from CDN on first use)
- Renders all pages as `<canvas>` elements inside a full-height modal
- No iframes (avoids browser PDF plugin blocking)
- ⬇ Download button in viewer header
- Graceful fallback (download link) if PDF.js CDN unreachable

### MD parser
- Parses curriculum markdown files with `# MODULE N: Title` headers
- Extracts: subtitle (italics), why it matters, readings (✅ verified / 🔍 search), watching (YouTube search terms), assignment, reflection

## Key functions (JS)
| Function | Purpose |
|----------|---------|
| `render()` | Top-level re-render dispatcher |
| `renderStudent()` | Student view: pace banner + module grid + detail panel |
| `renderDetail(mod)` | Detail panel with all tabs including Evaluate |
| `renderAttachmentsTab(modId, role)` | Attachments tab content |
| `renderAdmin()` | Design mode: module list with action buttons |
| `viewAttach(dbId, name)` | Opens PDF in modal via PDF.js canvas renderer |
| `uploadFiles(modId, role, files)` | Stores PDFs in IndexedDB, updates state metadata |
| `parseMarkdown(md)` | Parses .md file into curriculum object |
| `loadPdfJs()` | Lazy-loads PDF.js + worker from CDN |
| `saveState()` / `loadState()` | localStorage persistence + triggers cloud sync |
| `idbPut/Get/Delete()` | IndexedDB helpers for PDF blobs |
| `toggleRejected(modId, type, id)` | Skip/restore a reading or watching item |
| `getModPct(mod)` | Progress %; skips rejected items |
| `initAuth()` | Checks Supabase session; shows auth screen or renders app |
| `_syncFromCloud()` | Pulls user progress + canonical curriculum from Supabase |
| `_pushToCloud()` | Debounced upsert of progress to Supabase `user_state` |
| `publishCurriculum()` | Admin: writes curriculum to `canonical_curriculum` table |

## Embedded curriculum
`CANONICAL_CURRICULUM` const at the top of the script contains all 11 modules pre-parsed. Loaded into state on first visit (or when localStorage is cleared). Admin can push a new version to Supabase via the Publish button; non-admin users receive it on next login.

## Modules
0. Mindset & Meta-cognition
1. Strategic Thinking & Systems Thinking
2. Design Thinking & Human-Centred Research
3. AI Fundamentals — Rigorous, Non-Technical
4. AI Tools — Hands On
5. Pricing & Monetization Strategy
6. AI Product Strategy — The Layer Above the Tools
7. Execution & Stakeholder Management — Confidence Rebuild
8. Metrics, Evaluation & North Star Thinking
9. Pulling It Together — Artifact & Portfolio
10. Build Log & Publishing

## Known behaviours / gotchas
- Supabase SDK loads from CDN — if CDN fails, app runs in local-only mode (no auth, no sync).
- PDF.js loads from CDN — requires internet on first PDF open. Falls back to download link if offline.
- `localStorage` has a ~5-10MB limit. Only metadata is stored there; blobs go in IndexedDB.
- Clearing browser data wipes localStorage and IndexedDB (progress + attachments lost locally; cloud copy survives if signed in).
- The preview tool's click simulation fires before async functions resolve — always use a 500ms delay when testing clicks via eval.
- `let` variables in the script block are not on `window`, so they're invisible to `preview_eval` but accessible within the script closure.
- When testing with preview, after code changes: `localStorage.removeItem('pm_curriculum_v2'); window.location.reload()` to pick up new CANONICAL_CURRICULUM. Then `document.getElementById('authScreen').style.display='none'; render();` to bypass auth.
- Watching items now have an optional `url` field (direct link) alongside `searchTerm`. Both can coexist — the UI shows "Watch directly ↗" if `url` is present, "Search YouTube" if `searchTerm` is present.

## AI Curriculum Generator
Master prompt lives at `prompts/curriculum-generator.md`. Student fills in 5 context fields → pastes into Claude → gets back valid JSON matching the CANONICAL_CURRICULUM shape → imports into Canon. Future feature: build this as a form in the app.

## Things that could be added next
- Build the curriculum generator as an in-app form (see `prompts/curriculum-generator.md`)
- Export progress as PDF or CSV
- Module reordering (drag to reorder in admin)
- Search across modules
- Mobile-optimised layout improvements
- Offline PDF.js (embed the library to remove CDN dependency)
