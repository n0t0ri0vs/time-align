# CLAUDE.md — TimeAlign

## What this project is
Single-file frontend tool (`time-align.html`) for scheduling meetings across multiple time zones. Pure HTML/CSS/JS, no backend, no persistence, no build tools. Deployed as a static file — users open it directly in a browser or pin it as a tab.

## Absolute rules — never break these

- No backend, no server, no API calls from the app itself
- No localStorage, no sessionStorage, no cookies — zero persistence
- No `console.log` statements in production code
- No inline event handlers using `eval()` or `Function()`
- All external CDN resources must include SRI (`integrity` + `crossorigin` attributes)
- Single file only — no separate `.css` or `.js` files
- XSS prevention: escape all dynamic content inserted into the DOM; use `textContent` instead of `innerHTML` where possible; when `innerHTML` is required, sanitize input first
- CSP meta tag must be present in `<head>`

## Stack
- HTML5 / CSS3 / Vanilla JS (ES6+)
- Luxon for date/time/timezone handling — load via CDN with SRI
- No frameworks (no React, no Vue, no jQuery)
- No build tools (no Vite, no Webpack, no npm)

## Commit conventions
- `feat:` new functionality
- `fix:` bug fix
- `style:` visual/CSS changes only
- `refactor:` code restructure, no behavior change
- `docs:` changes to .md files only
- `audit:` security review pass

## Git workflow
Always work on `main`. No auxiliary branches. No worktrees.
Sequence: `git add -A` → `git commit -m "type: description"` → `git push origin main`

## Periodic audit
Every 10 commits, run a security audit pass with commit `audit: security review vX`. Check: XSS vectors, CSP header, SRI on all CDN resources, no persistence, no console.log.

## File structure
```
time-align/
├── time-align.html       ← entire application
├── CLAUDE.md             ← this file
└── .claude/
    └── SPEC.md           ← project state and roadmap
```
