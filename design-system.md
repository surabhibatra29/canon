# Canon — Design System

## Brand

**Name:** Canon  
**Byline:** by Sur:B  
**Tagline:** Curate your curriculum. Execute it. Track it. Come out sharp.

**Logo gradient:** `135deg, #4F5AF0 → #7C3AED`  
Used on: logo icon, avatar, primary CTAs

---

## Color tokens

### Light mode (`:root`)

| Token | Value | Usage |
|-------|-------|-------|
| `--purple` | `#4F5AF0` | Primary brand, links, active states, progress |
| `--purple-dark` | `#3D48DC` | Hover on primary |
| `--purple-light` | `#EDEFFD` | Pill backgrounds, subtle highlights |
| `--green` | `#059669` | Complete, sync dot, success states |
| `--green-light` | `#D1FAE5` | Success pill backgrounds |
| `--amber` | `#B45309` | In-progress, syncing |
| `--amber-light` | `#FEF3C7` | Warning pill backgrounds |
| `--red` | `#DC2626` | Destructive actions, errors |
| `--gray` | `#78716C` | Muted text, icons |
| `--gray-light` | `#ECEEF9` | Hover backgrounds, subtle surfaces |
| `--border` | `#E0E2F0` | All borders |
| `--bg` | `#F3F4FB` | Page background |
| `--text` | `#1C1917` | Primary text |
| `--text-muted` | `#78716C` | Secondary text, labels |

### Dark mode (`[data-theme="dark"]`)

| Token | Value |
|-------|-------|
| `--purple` | `#818CF8` |
| `--purple-dark` | `#A5B4FC` |
| `--purple-light` | `#0F1030` |
| `--green` | `#34D399` |
| `--green-light` | `#064E3B` |
| `--amber` | `#FCD34D` |
| `--amber-light` | `#78350F` |
| `--red` | `#FCA5A5` |
| `--gray` | `#A8A29E` |
| `--gray-light` | `#16162A` |
| `--border` | `#1E1E35` |
| `--bg` | `#0E0E18` |
| `--text` | `#F4F1EB` |
| `--text-muted` | `#9B958D` |

---

## Elevation & shadows

| Token | Value | Usage |
|-------|-------|-------|
| `--shadow` | `0 1px 4px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04)` | Cards, small elements |
| `--shadow-md` | `0 4px 24px rgba(0,0,0,0.08), 0 2px 8px rgba(0,0,0,0.04)` | Dropdowns, modals |
| `--shadow-lg` | `0 20px 60px rgba(0,0,0,0.1), 0 8px 24px rgba(0,0,0,0.06)` | Overlays, PDF viewer |

---

## Shape

| Token | Value |
|-------|-------|
| `--radius` | `16px` (module cards, panels) |
| Dropdowns / pills | `8px–12px` |
| Buttons | `6px–8px` |
| Avatar | `50%` |
| Logo icon | `7px` (28px icon) / `30px` (120px icon) |

---

## Glassmorphism

Used on the header and module cards:

```css
background: rgba(255, 255, 255, 0.65);
backdrop-filter: blur(16px);
-webkit-backdrop-filter: blur(16px);
border: 1px solid rgba(255, 255, 255, 0.85);
```

Header specifically:

```css
background: rgba(255, 255, 255, 0.78);
backdrop-filter: blur(24px);
border-bottom: 1px solid rgba(255, 255, 255, 0.6);
box-shadow: 0 1px 24px rgba(79, 90, 240, 0.06);
```

---

## Page background

Subtle radial gradients anchored to opposing corners, fixed attachment:

```css
background: var(--bg);
background-image:
  radial-gradient(ellipse at 10% 80%, rgba(139,92,246,0.1) 0%, transparent 50%),
  radial-gradient(ellipse at 88% 12%, rgba(79,90,240,0.09) 0%, transparent 50%);
background-attachment: fixed;
```

---

## Typography

**Font:** Inter (Google Fonts), fallback: `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`

| Role | Size | Weight | Notes |
|------|------|--------|-------|
| Page heading | 26–28px | 700 | `letter-spacing: -0.5px` |
| About wordmark | 80px | 800 | `letter-spacing: -4px` |
| Module title | 15px | 700 | `line-height: 1.3` |
| Logo name | 17px | 700 | `letter-spacing: -0.4px` |
| Nav / label | 13–13.5px | 600 | |
| Body | 13–14px | 400 | `line-height: 1.6` |
| Module number | 11px | 600 | uppercase, `letter-spacing: 1px` |
| Meta / muted | 11–12px | 400–500 | `color: var(--text-muted)` |
| Micro label | 10–10.5px | 700 | uppercase, `letter-spacing: 0.7px` |

---

## Key component patterns

### Module card
```css
background: rgba(255,255,255,0.65);
backdrop-filter: blur(16px);
border: 1px solid rgba(255,255,255,0.85);
border-radius: 16px;
box-shadow: 0 4px 20px rgba(0,0,0,0.05), 0 1px 4px rgba(0,0,0,0.04);
transition: box-shadow 0.25s, transform 0.25s, border-color 0.25s;
```
Hover: `translateY(-4px)` + purple-tinted shadow  
Selected: `border-color: rgba(79,90,240,0.35)` + focus ring

### Spine / Core pill
```css
background: rgba(79,90,240,0.08);
color: var(--purple);
border: 1px solid rgba(79,90,240,0.2);
border-radius: 20px;
font-size: 11px;
font-weight: 600;
```

### Progress bar
```css
height: 4px;
background: var(--purple);
border-radius: 999px;
opacity: 0.6;
transition: width 0.5s;
```

### Mode toggle (Learn / Design)
```css
background: rgba(0,0,0,0.05);
border-radius: 8px;
padding: 3px;
```
Active button: `background: white; color: var(--purple); box-shadow: 0 1px 4px rgba(0,0,0,0.1);`

---

## About / splash screen

Centered column layout. Full-screen overlay with the page background gradient.

- Icon: 120×120px, `border-radius: 30px`, brand gradient, `box-shadow: 0 16px 48px rgba(79,90,240,0.25)`
- Wordmark: 80px / 800 weight / `letter-spacing: -4px`
- Tagline: 15px / 400 / `line-height: 1.75` / `color: #5a5a80` — three sentences, one per line
- Byline: 11.5px / `color: #b0b0cc` / `letter-spacing: 0.1em`
- Dismiss hint: 12px / `rgba(0,0,0,0.2)` / pinned to bottom center
