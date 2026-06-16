# Canon: Architecture & Technical Reference

Implementation-level detail for the `curriculum-tracker.html` single-file app. Useful when building or debugging.

---

## 5. Technical Reference

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
  - Done + no note: ghost `✏ note` button (hidden, appears on row hover)
  - Done + has note: `💬 [40-char plain-text preview]` chip (always visible)
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
- **↺ Reset to Canon**, resets curriculum to embedded `CANONICAL_CURRICULUM` constant. Does NOT touch progress (bug fixed 2026-06-04).
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
