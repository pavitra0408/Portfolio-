# Portfolio: pavitradesign.com (Pavitra Verma, communication designer)

Read this file, `DECISIONS.md`, `DESIGN-SYSTEM.md` and `HANDOFF.md` before any non-trivial task. Update `DECISIONS.md` at the end of the task.

## Project facts
- Single file site: everything is in `index.html` (inline CSS and JS, no build, no libraries). Hash routing: `#/`, `#/about`, `#/contact`, `#/work/:k`, `#/work/:k/:slug`. Content comes from the array `D` and the image map `IMG`.
- Deployed by Cloudflare Pages from the repo. **Never commit, push, merge, deploy or publish without explicit approval in chat.**
- Local test server: `python3 -m http.server 8765` in the repo root (it occasionally resets connections; re-check before calling an asset broken).
- Files that must not ship are listed in `.assetsignore` (docs, `.claude`, notes). Add any new notes file there.

## Identity (non-negotiable)
- Black foundation, slate secondary, garnet `#C03A4A` (`--o`) used sparingly (full stop, hairlines, markers, focus ring, large type). Outfit and DM Mono only.
- Keep the radar / interference-field concept and the existing artwork. No glassmorphism, gradients, glows, particles, parallax or persistent glitch effects.
- No fabricated roles, outcomes, metrics, dates, client names, tools or process stages. Keep `[ADD …]` placeholders in data until Pavitra supplies the facts; never replace them with invented text.
- Do not redraw logos or replace brand typefaces. Use the real assets.

## Workflow for every substantial task
1. **Inspect first**: read the relevant code and the four docs above. Reproduce a bug before fixing it; find the root cause, not a symptom patch (no z-index guessing).
2. **Skills**: list what is installed, read the SKILL.md of every relevant one, and say which were used. Useful here: `screenshot-visual-qa` (always, before reporting visual work as done), `visual-reference-art-direction`, `visual-storytelling-and-diagram-design`, `taste-critique`, `design-skills:accessibility-audit`, `emil-design-eng` (motion), `vectorlab-ux-skills:reduced-motion`. `bencium-controlled-ux-designer` says "ask before design decisions": when a direction is already approved in `DECISIONS.md`, treat it as that approval. `bencium-innovative-ux-designer` (ten new directions) is for new visual languages only, not for work inside an approved direction. Skills advise; project requirements win on conflict. Never claim a skill was used if it was not read.
3. **Critique before building**: state what is weak, what is distinctive, what to keep.
4. **Scope**: do only what was asked. Do not restyle other disciplines or pages "while there".
5. **Verify**: render and measure, do not just read code. Desktop 1440×900 plus a narrower desktop (1280×720 or 1024×768); mobile only for regressions unless asked. Check console, failed requests, keyboard, focus, reduced motion (emulate it), direct URL load and in-site navigation, Back/Forward. Report what was tested and what was not; never call untested behaviour verified.
6. **Record**: update `DECISIONS.md` (approved, rejected, bugs and fixes, constraints, QA, deferred).

## Implementation rules
- The background is the single persistent `#env` canvas (`envInit` at boot). Do not add per-route background canvases; call `envRoute()` (already in `route()`) and keep text selectors for quiet zones up to date in `envMask`.
- Reveal motion uses `[data-mv]` (see `mvInit`). Never hide an observed element to zero visible area.
- Reuse tokens (`--bg`, `--ink`, `--mute`, `--o`, `--edge`, `--ease-out`, spacing in `DESIGN-SYSTEM.md`). Do not name spacing tokens `--s*` (`--s1` is a surface colour).
- Every route calls `route()`, which stops per-page animation loops (`hxStop/rpStop/abStop/vzStop/fieldStop`) before rendering; add a matching stop for any new loop.
- Global click handler intercepts `a[href^="#/"]` for the morph transition; do not `preventDefault` on mouse clicks of those links.
- Respect `prefers-reduced-motion`; labels at least 14px; visible focus; 44px touch targets on mobile.
