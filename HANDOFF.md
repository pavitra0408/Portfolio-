# Handoff — pavitradesign.com portfolio

For any Claude Code session (or developer) picking this project up next. Written 2026-10-27 after a full site audit and a round of hide-consistency bug fixes.

## Project shape

- **Single file site**: everything lives in `index.html` — inline `<style>`, inline `<script>`, no build step. Deployed via Cloudflare Pages from this repo (auto-deploy on push to `main`). Repo: `github.com/pavitra0408/portfolio-` (GitHub shows it moved to `pavitra0408/Portfolio-`, but `origin` still resolves and pushes succeed — no action needed unless push starts failing).
- **Owner**: Pavitra Verma, communication designer / video editor, 4th year at Pearl Academy, Jaipur. Email in repo context: `rv200227@gmail.com`.
- **Routing**: hash-based, no server routing needed. Patterns: `#/`, `#/about`, `#/contact`, `#/work/:disciplineKey`, `#/work/:disciplineKey/:projectSlug`.
- **Content model**: everything flows from one array.
  - `const D = [...]` (~line 658) — array of **discipline** objects: `{k, n, s, h, w, hide?, p:[...]}` where `k`=key/slug, `n`=display name, `h`=accent hex color, `p`=array of **project** objects.
  - Each project: `{t:title, m:blurb, img, r:aspect-ratio, role, fmt, yr, custom?:'fnName', sec?:[...], vid?, ig?, ...}`.
  - `custom` field routes a project to a hand-written render function (see dispatch table at line ~1032); projects without `custom` fall through to the generic `study()` template using `GEN` (line 681) as default section content.
  - **`hide:1`** on a discipline object excludes it from the live site while keeping the code/content intact. This is the established pattern for "built but not ready to show" sections (used for Typography right now; was previously used for Experiments before it was unhidden).

## ⚠️ Critical gotcha: `hide:1` is NOT automatically respected everywhere

There is **no single source of truth** for "which disciplines are visible" — at least 4 separate places in the code independently enumerate `D` and each one had to be manually patched to skip `d.hide`:

1. Homepage hero chip grid — `D.filter(d=>!d.hide)` — was already correct.
2. About page "What I do" list (`DO` var, `function about()`, ~line 1276) — **fixed** this session: added `!x.hide&&` to the filter.
3. Homepage scrolling marquee (`function marquee()`, ~line 692) — **fixed**: `D.filter(d=>!d.hide&&cnt(d)>0)`.
4. Mobile/intro preloader clip-timeline labels (`function load()`, `CL.map(...)`, ~line 1516) — **fixed**: guarded with `d>=0&&!D[d].hide`.
5. Preloader's "fly to tile" completion animation (`function end()`, ~line 1519–1520) — **fixed**: this one had a second, independent bug (see below) layered on top of the hide-leak.

**If you ever add another `hide:1` flag (or unhide one), grep for every `D.filter`, `D.map`, `D.forEach`, and `D[` across the file and re-check each one respects `.hide`.** Don't assume the homepage chip grid's correctness generalizes.

## Bug fixed this session #2: stale preloader index after hiding a discipline

The preloader's completion "fly" animation looked up destination tiles via `$('.obj[data-i="${x.d}"]')`, where `x.d` was the **raw index into the full `D` array** (0–5). But `.obj` elements on the homepage are only rendered for *visible* disciplines and are keyed by their **position in the filtered/visible list**, not their raw index. Once Typography (`D[4]`) was hidden, every discipline after it (Experiments, `D[5]`) had its raw index and visible-position diverge, so the lookup silently returned `null` and the fly-animation no-op'd for Experiments specifically (no console error — `.filter(Boolean)` swallowed it).

**Fix applied**: build `const VD = D.filter(d=>!d.hide)` once, then map `x.d` → `VD.indexOf(D[x.d])` before using it as the `data-i` lookup key and before reading `D[x.d].h`. Already committed (see Recent commits below). If you add a 7th discipline or hide another one, this mapping should keep working automatically since it's computed live — but re-verify the Experiments tile's fly-to-destination animation still plays after any further `D` changes.

## 🐛 Known bug — NOT yet fixed — mobile header text wrap/overlap

**Confirmed via screenshots at 390×844 across every route** (home, about, Visual/ARCEON, Recurring, etc.): the "Pavitra Verma" name badge in `<header>` (markup at line 627: `<header><a href="#/" aria-label="Home"><b>Pavitra Verma</b></a><nav>...</nav></header>`) wraps onto two lines at phone widths (~390px and narrower). The two-line badge then collides with the adjacent nav pill and overlaps page content sitting underneath, which is visible through the header's gradient mask (`header::before` fades out after 52% rather than covering solid — see line 22).

**Root cause**: no `white-space:nowrap` and no responsive font-size reduction on the header name link (`header>a`, styled at line 578) or its `<b>`. The only mobile-specific header rule is `@media(max-width:760px){header nav{padding:9px 18px;gap:18px}}` (line 144) — it shrinks the *nav* pill but does nothing for the name badge.

**Suggested fix** (not yet applied — confirm with Pavitra before changing, since it's a visible design tweak): add `white-space:nowrap` to `header>a b` (or the `header>a` rule itself), and inside the existing `@media(max-width:760px)` block, drop the name badge's font-size a notch (e.g. `header>a{font-size:14px}` or similar) so "Pavitra Verma" fits one line down to ~360px. Test at 375px (iPhone SE), 390px, and 430px after any change.

## Data gaps — literal `[ADD ...]` placeholder text still live on the site

Not bugs — these need real information from Pavitra and should **not** be fabricated. Full inventory (line numbers may drift slightly after future edits — search for the bracketed text to relocate):

| Location (function / page) | Placeholder(s) |
|---|---|
| `GEN` default sections array, line 681 — used by any project without a `custom` renderer | `[ADD RESULT]` |
| `study()` generic template fallback, line 1035 | `[ADD ONE-LINE RESULT — only what is true]` |
| Per-project defaults, line 680 (`D.forEach(...)`) — fires for ANY project missing `role`/`fmt`/`yr` | `[ADD ROLE]`, `[ADD FORMAT]`, `[ADD YEAR]` |
| `MG` array (MG Select reel case study), line 657 | `[ADD: what worked, what did not...]` (Reflection), `[ADD]` (What I Do) |
| "Indian Travel Learning Platform" project object, line 671 (UI/UX discipline) | `role:'[ADD ROLE]'`, `[ADD FINDINGS]` in The Research section. **Also has no `img` key at all** → its card renders a gray placeholder box via `ph()` instead of a real cover image. |
| "Chainsaw Man" project object, line 669 | `role:'[ADD ROLE]'` |
| `postcase()` — Posters page, lines 775–777 | `[ADD POSTER 02/03/04: story and final poster]` |
| `podcase()` — Podcasts page, line 836 | `[ADD INSTAGRAM LINK: watch the full podcast on Instagram]` |
| `rccase()` — Recurring page, line 931 | `[ADD OUTCOME: was it charged, or did it fail?]` |
| `mgP()` — MG Select (photography) page, line 1065 | `[ADD CREDIT]` |
| `dlcase()` — DigiLocker 2.0 page, line 1132 | `[ADD ONE MORE REAL LESSON FROM THE PROJECT]`, `[ADD PDF LINK]` |
| `soul()` — Soul photo series, line 1144 | `[ADD APERTURE · ISO]` |
| `csm()` — Chainsaw Man page, line 1160 | `[ADD PHOTOGRAPH: MANGA]`, `[ADD PHOTOGRAPH: BOOK COVER]` (two missing images, rendered as styled placeholder tiles via a locally-shadowing `ph()` defined inside `csm()`, not the global one) |
| `portraits()`, lines 1167–1169 | `[ADD EVENT / CONTEXT]`, `[ADD ONE OR TWO SENTENCES ABOUT HOW YOU SHOOT PORTRAITS]`, `[ADD CREDITS]` |
| `reflect()` — Reflections photo series, line 1179 | `[ADD SHUTTER SPEED · APERTURE · ISO]`, `[ADD MODEL CREDIT]`, `[ADD DESIGNER]` |

Global placeholder helper: `const ph=(l='Image',r)=>...` at line 684 renders a gray box with `[Replace with final image]` whenever called with a non-empty label — this is what shows for any project missing an `img` key (see Indian Travel Learning Platform above). There's a second, differently-signatured `ph` defined locally inside `csm()` (~line 1150) that shadows the global one for that page only — don't confuse the two if editing either.

## UI issues found (not yet fixed)

- **Mobile tap targets undersized**: header nav links, "Back to top ↑" button, and "Watch on Instagram ↗" links measure ~20–26px tall at 390px viewport. Apple HIG / Android guidance wants ≥44px (iOS) / ≥48px (Android) for comfortable touch targets. Low priority, but worth a pass if doing a mobile-polish round.
- **No favicon**: zero `<link rel="icon">` tags anywhere in the file, no `favicon*` file in the repo root. Browser tabs show a generic/blank icon.
- **Sub-pixel horizontal overflow on `#/work/motion`**: `document.documentElement.scrollWidth` exceeds `innerWidth` by 4–6px at 1024px and 768px viewports. **Not user-visible** — `body{overflow-x:hidden}` (line 10) already clips it — but flagging in case it's a symptom of something that grows worse at other breakpoints later.

## Code cleanliness

- **Dead code**: the top-level `const MG=[...]` array (line 657) is referenced only once, at its own declaration (used as `sec:MG` on the MG Select reel project, line 660) — this is actually a live reference and IS used, correction from earlier analysis: `sec:MG` on line 660 feeds the MG Select × Chandigarh Times Fashion Week reel's custom page. Re-verify before deleting anything here — treat as **used**, not dead, pending a fresh double-check if you touch this area.
- **No automated tests / no build step** — validate any script edit with `node --check` on the extracted `<script>` block before committing (see Verification workflow below).

## Verification workflow used this session (repeat before any future commit)

```bash
cd /home/claude/portfolio-   # or wherever this repo is checked out

# 1. Extract the inline <script> block and check JS syntax
python3 -c "
content = open('index.html').read()
start = content.find('// ===== CONTENT')
script_start = content.rfind('<script', 0, start)
script_tag_end = content.find('>', script_start) + 1
script_end = content.find('</script>', script_tag_end)
open('/tmp/extracted.js', 'w').write(content[script_tag_end:script_end])
"
node --check /tmp/extracted.js && echo "SYNTAX OK"

# 2. Dynamic crawl of every live route with Playwright — catches pageerrors,
#    console.error, broken images, broken internal links. Build the route
#    list straight from the page's own `D` object so it never goes stale:
#    page.evaluate("() => D.filter(d=>!d.hide).map(d => ({k:d.k, slugs: d.p.map(p=>p.sl)}))")
```

No CI, no test suite — this crawl script (or an equivalent) is the only safety net. Worth saving a copy of it into the repo (e.g. `scripts/crawl-check.py`) if this becomes a recurring workflow.

## Recent commits (most recent first)

```
046c38c Fix hidden-discipline leaks and stale preloader index
48e2253 Update About tools list and refresh resume
6004b15 Fix squeezed making-of images on Kal Bhairav page
b26cc0e Add Kal Bhairav visualizer to Experiments
1dba3bd Fix mobile card navigation, add downloadable resume
```

`046c38c` is this session's work: added `hide:1` to the Typography discipline object, fixed the three hide-leak sites (About list, marquee, preloader labels), and fixed the stale preloader fly-animation index. Already pushed to `origin/main` — Cloudflare should auto-deploy it.

## Suggested next steps, roughly in priority order

1. **Fix the mobile header wrap/overlap bug** (see above) — this is the most visible/embarrassing bug on the live site right now, affects every page on common phone widths.
2. Get Pavitra to supply the real content for the data-gap table above, then fill it in (do not fabricate camera settings, credits, Instagram links, or reflections).
3. Add a favicon.
4. Optional polish: bump mobile tap target sizes for nav/CTA links.
5. Re-run the verification workflow (syntax check + full crawl) before and after any of the above, and screenshot at 390px width specifically to confirm the header fix actually resolves the overlap before pushing.
