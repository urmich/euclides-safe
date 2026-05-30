# Project Plan — "הכספת של אוקלידס" (Euclid's Safe)

## Goal
Hebrew RTL geometry escape-room web game (grades 9–10). Deploy to Azure Static Web Apps from a private subscription, deployable from the local machine.

## Architecture decision
- **Single self-contained `index.html`** — inline CSS, vanilla JS, inline SVG. Zero dependencies, zero build step.
- Rationale: easiest local testing (just open the file / static server) and most robust Azure Static Web Apps deploy (nothing to compile, upload folder as-is). PRD suggested React/Vite/Tailwind but allows any client-side SPA; a no-build single file removes all toolchain friction given no Node toolchain is installed locally.

## Game spec (from PRD)
- Global state: `currentRoom` (0 intro, 1–5 puzzle rooms, 6 keypad/finish), `currentPhase` (1–3), `collectedCodes` (4 numeric answers), `gameTimer` (MM:SS), `wrongAttempts`.
- 3-stage lock per room: (1) identify structure — 4-option MC; (2) reasoning/theorem — 3-option MC; (3) numeric input.
- Rooms & expected answers:
  - Room 1 "חדר המראות" — intersecting lines, adjacent angles → **65**
  - Room 2 "מסדרון המקבילים" — parallel lines + transversal, corresponding angles → **73**
  - Room 3 "הגשר המשולש" — congruent triangles SAS → **48**
  - Room 4 "מגדל הצללים" — similar triangles + angle sum → **75**
  - Room 5 "מעבדת הטעויות" — find logical error (single MC, correct = option 2)
  - Room 6 final keypad sequence: **65734875**
- Theme: dark "digital vault / control room", Hebrew sans (Rubik/Assistant), neon green/cyan/orange accents, mobile-first, RTL.

## Required features (PRD §6)
- a. localStorage persistence (room, phase, timer, codes) — resume on refresh.
- b. Hint system — floating "בקש רמז" button after 3 consecutive wrong attempts; per-room targeted hint.
- c. Web Audio API — success/error/victory sounds (generated, no asset files).
- d. Input validation — type="number", trim, ignore special chars.
- e. Keyboard — Enter submits numeric stages.
- SVG drawings inline per room with centered text labels.
- CSS shake animation on error.
- Confetti + victory message + final time on success.

## Local testing
- Open `index.html` directly, or run `python3 -m http.server 8000` in the folder.

## Azure deploy (from local machine)
- Provide `staticwebapp.config.json` for SPA + RTL.
- Deploy options documented in repo: Azure CLI `az staticwebapp` or SWA CLI `swa deploy`. Private subscription — user runs `az login` and selects subscription.

## Status
- [ ] Build index.html (full game)
- [ ] staticwebapp.config.json
- [ ] README with run + deploy steps
- [ ] Local smoke test
