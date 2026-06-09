# SPEC.md — TimeAlign

## Current state
Version: 0.0.0 — project initialized, no code yet.

## What to build

### Core concept
A browser tool that eliminates manual timezone calculation when scheduling international meetings. The user defines their own availability window; other participants are assumed to work 09:00–18:00 in their respective timezone. The tool finds the optimal intersection and outputs formatted text ready to paste into Teams or Outlook.

### Participants model
- Minimum 2 participants, maximum 4
- Participant 0 (the user): name, timezone (autodetected + confirmed on load), meeting window from/to (free input, overrides their working hours for this search)
- Participants 1–3 (others): name + timezone only; working hours assumed 09:00–18:00, not editable
- "+ Add participant" button, up to the 4-participant limit
- Each participant gets a distinct color for busy slot marking on the grid

### Timezone detection on load
On first load, detect `Intl.DateTimeFormat().resolvedOptions().timeZone`. Show a confirmation banner/modal: "We detected your timezone as X — is this correct?" with a dropdown to change it. Do not proceed until confirmed. This addresses systems with OS locale ≠ physical location.

### Tab 1 — Find slot
**Grid:**
- Week view, Mon–Fri, 30-minute slots
- Date range: June 2026 – December 2026 (real dates, not generic weekdays)
- Navigation: previous/next week arrows, current week highlighted
- Time rows labeled in the user's local time
- Each participant's busy slots shown in their distinct color
- Toggle between participants to mark their busy slots (same interaction as BreachNotify's pattern: click/drag to mark, click again to unmark)
- Slots outside any participant's working window shown as dimmed/unavailable

**Results:**
- Button "Find available slots" triggers intersection calculation
- Output: list of free slots sorted by quality (center of working day = best, edges = penalized)
- Each result shows: day + date, time in every participant's timezone, quality badge (Best / Good / Ok)
- "Copy for invite" per slot generates formatted text: e.g. `Tuesday 16 June · 10:00 CEST (Sergio) / 09:00 BST (Alice) / 04:00 ET (Bob)` — plain text, ready to paste

### Tab 2 — Convert time
- Select source timezone (client's)
- Select target timezone (yours, pre-filled from confirmed timezone)
- Pick date (calendar input) and time
- Output: converted time shown large on screen, with note if it crosses midnight (next day / previous day)

### Language toggle
EN (default) / ES. Applies to all UI labels. Timezone names and day names adapt to selected language.

## Roadmap

### Layer 1 (current)
Everything described above.

### Layer 2 (future)
- localStorage opt-in: save participant profiles (name + timezone) with a named profile system
- Shareable URL encoding: encode current setup in URL hash for quick sharing

### Layer 3 (future)
- Direct calendar integration (requires OAuth — out of scope until explicitly decided)

## Design principles
- One file, self-contained
- Works offline once loaded (except CDN resources — Luxon)
- No data leaves the browser
- Mobile-usable but desktop-first

## External dependencies
- Luxon (datetime/timezone): https://cdn.jsdelivr.net/npm/luxon@3/build/global/luxon.min.js
  - SRI hash to be added before first commit with actual CDN resource
