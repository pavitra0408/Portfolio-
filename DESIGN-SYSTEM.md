# Design system — Direction A: the Annotated Index

Pavitra's identity is the constraint (see HANDOFF.md and project memory). This file records the tokens added to `index.html` (`:root`, "DESIGN TOKENS" block) and the rules for using them. **Tokens are defined but not yet consumed**, so adding them changed nothing visually. Adoption happens phase by phase, on `design/direction-a`.

## Idea

The thesis is "I notice how things work." Direction A makes that literal: one continuous numbering across all work, and short margin notes beside media in a human voice. Notes may only be built from Pavitra's existing wording (project blurbs `m`, headings, reflections) or text she approves. Never invent claims, credits or metrics.

## Rules that matter more than the values

1. **Garnet (`--accent`, `#C03A4A`) means "current" or "next action", and only appears large.** It is 3.95:1 on black: fine for large type and graphics, not for small text. Allowed: the headline full stop, underlines and hairlines, markers, the focus ring, type of 24px and up (section numerals, big hover titles). Small text is white or slate, never garnet.
2. **Weight 300 is the identity.** Headings and body are 300; 400 only for small UI text that needs it; 500 is exceptional.
3. **Two voices only.** Outfit for everything read; DM Mono for numbers and margin notes. Do not introduce a third.
4. **Not everything shouts.** Use at most one `--t-hero` or `--t-display` element per screen. Section eyebrows may be uppercase; margin notes are sentence case.
5. **Radius:** `--r-2` (3px) for images and cards, `--r-pill` for nav and buttons. Odd values in the CSS today (14px, 10px, 4px, 99px) should converge on these when touched.
6. **Motion:** two easings (`--ease-out` for entrances, `--ease-inout` for transitions), four durations. Every interaction needs a reduced-motion alternative; content must be usable before any animation finishes.
7. **Touch targets** are at least `--hit` (44px).
8. **No tints, no beige, no brown, no pastels.** The page is pure black (`--tint:0%`). The old warm peach/blue background glows and per-discipline page washes were removed.

## Palette (Oct 2026: B2, Garnet & Slate)

Idea: warm/cool tension with one saturated colour. Cool slate-blue greys carry everything quiet; one deep garnet marks the thing that matters. Chosen over three other directions (verdigris, monochrome, mineral pigments) after rendering all of them on the real pages. Contrast is calculated against `#000`.

| Role | Value | Contrast on black | Use |
|---|---|---|---|
| Page | `#000000` | n/a | Canvas |
| Surfaces | `#0D1014`, `#161B21` | n/a | Tiles, panels (slate-tinted) |
| Text | `#FFFFFF` | 21.0 | All reading text |
| Quiet text | `#A2ADBA` | 9.3 | Secondary text, captions |
| Garnet (accent) | `#C03A4A` | 3.95 (3.25 on darkest surface) | Large type and graphics only |
| Motion marker | `#6E86A0` | 5.6 | Marker squares, hairlines |
| Visual marker | `#8199B3` | 7.1 | |
| UI / UX marker | `#9AB0C6` | 9.4 | |
| Photography marker | `#FFFFFF` | 21.0 | |
| Experiments marker | `#5A7088` | 4.1 | |
| Lines | `rgba(150,170,195,.16)` | n/a | Borders, dividers |

Disciplines are told apart by label and position, not by hue (the markers are slate steps). Page is pure black; no tints, no beige, no brown, no pastels. Small-text hover states turn white, not garnet.

Left alone on purpose: colours inside the work itself (ARCEON brand navy, the DigiLocker and Recurring app mockups, the Recurring notification timeline's amber and red). The Recurring timeline's blue marker now follows the UI/UX slate because it was recoloured with the site palette.

Not verified: comfort with real users; hover states by eye (checked in the stylesheet, not by screenshots of each).

## Background system: the interference field (Oct 2026)

Concept: the site reinterprets a compositing workspace (the Fusion-inspired origin) as one instrument. The radar already draws a ring every 150px from the headline's full stop. The field keeps that scale: **A** is the radar's own rings, drawn finely (10px) with a major ring every 15th, flowing slowly outward; **B** is a second emitter (10.5px) that drifts while its rings flow inward. Where they overlap they interfere into curved fringes that converge and disperse.

How it moves (continuous, no input needed): one loop (`fieldTick`, 30 fps) on the existing canvases (`#rg0` home, `#rp0` inner pages). Ring flow (about 3 px/s outward, 2 px/s inward), B's drift, B's ring spacing, the warp phase and the warp amplitude all run on different slow periods, so the pattern evolves instead of translating. The radar's garnet pulse brightens the field rings it crosses (`PULSE.r`). Speeds are in `fieldFrame`; they were set by measurement: much slower is invisible, much faster shimmers.

Rules:
1. **Behind everything, never interactive:** canvases have `pointer-events:none` and sit below content.
2. **Cheap:** about 2 ms per frame. Only rings inside the visible region are drawn, with precomputed angle tables and style strings; no filters. The mask is rendered once and applied by CSS, so a frame only strokes lines.
3. **Pauses:** when the tab is hidden, when the page is scrolled past the hero band, and on route change. With `prefers-reduced-motion` it draws one still frame and never loops (the radar pulse is also off).
4. **Asymmetric density.** Detail gathers around the radar's origin (bottom-right of the home hero) or the top-right of a page hero; the viewport edge crops it on purpose.
5. **Reading zones are adaptive.** `fieldHoles` measures every piece of text under 40px and the mask clears around it. It is measured again after the page settles, because text slides in after load. Display type (40px and up) may sit over the field. Never solve readability by lowering opacity.
6. **Intensity follows content:** home 1.5, discipline and case-study pages 1.3 and 1.15, About 0.55.
7. **Home:** floating thumbnails keep out of the field's core (`HX.fz`).

Removed as decorative chrome: the 48px full-screen grid, the plus-sign cursor glow and its glyph asset, the cursor spotlight orb, the warm background glow, the plus marks and dots the radar pulse used to draw, the marquee of discipline names, the homepage identity lines and the "start here" tags and caption squares, the uppercase descriptor and ordinal numbers in the hover preview, the "View" chips on photos, the "Scroll" hint on the ARCEON page, the preloader's ticking timecode, the cycling-colour squares on About chips, the crosshair lines on the phone radar, and dead CSS from an old node-wiring idea (pins, wires, grab and crosshair cursors).

Kept on purpose: the radar and its pulse; the intro timeline (clips named after the disciplines, playhead, and the fly-to-tile hand-off) without its timecode; node-style squares on the home tiles and the list dots on About; project counts on tiles; the Fusion node-graph labels on the Personal Reel page and all prototype and diagram text (project content); real metadata.

Not touched (mobile phase): the phone radar rings and the phone work strip.

## Type roles

| Role | Token | Notes |
|---|---|---|
| Hero | `--t-hero` | Discipline page titles, marquee. Rare. |
| Display | `--t-display` | Contact call, next-project, big figures. |
| Page / case title | `--t-h1` | Case-study titles, homepage thesis. |
| Section heading | `--t-h2` | The missing middle step. |
| Project title / sub-head | `--t-h3` | Card titles, lead lines. |
| Lead | `--t-lead` | Intro paragraphs. |
| Body | `--t-body` | 16px / `--lh-body`; line length `--measure`. |
| Caption | `--t-cap` | 15px — the size the site already uses most. |
| Note | `--t-note` | 14px (was 13). Metadata and the second line of captions. Nothing on the homepage is below 14px. |
| UI | `--t-ui` | 16px, weight 400. Header nav, back, back-to-top, footer, skip. |
| Node title | `--t-node` | 18px, weight 400. Titles on the homepage rectangles. |

## Spacing, layout, images

- Space: `--sp-1…10` on a 4pt base. Page gutter `--gutter`, container `--container` (1440px), 12-column grid with 24px gaps (as today).
- Image ratios already in use: portrait 4/5, story 9/16, film 16/9, photo 3/2, UI 4/3 (`--ar-*`). Variety is a feature; do not force one ratio.
- Breakpoints (cannot be variables): 560 phone, 760 nav/header, 900 layout collapse, 1100 sticky-rail layouts. Consolidate in the responsive phase, not before.

## Evidence behind the values (Oct 2026 audit of `index.html`)

- Font sizes in use: 15px ×67, 13px ×22, 16px ×19, 12px ×17, 14px ×17, plus ~20 distinct fluid display sizes. Tokens collapse them into one scale.
- Weight 300 ×51, 400 ×31. Radius 3px ×20, 6px ×16, 999px ×11.
- Transition durations in use: .5s ×20, .4s ×15, .35s ×11, .2s ×10, .3s ×9, .9s ×8, .6s ×6, .8s ×6.
- Colour was already well tokenised (`--ink`, `--mute`, `--line`, `--o`, `--e` account for most var() uses), which is why the palette change was a handful of root values plus some hardcoded rgba/hex replacements.

## Homepage finalisation (Oct 2026)

Four reading levels on the homepage: **1** the headline (display, weight 300); **2** the line under it (`--t-lead`); **3** rectangle titles and links (`--t-node` 18px / `--t-ui` 16px, weight 400); **4** counts and captions (`--t-cap` 15–16px, `--t-note` 14px). Nothing is smaller than 14px, and quiet text is slate `#A2ADBA`, never a lowered opacity.

**Rectangles:** 232×66 (was 168×52), title 18px, count 16px. After the random layout is placed, `layout()` pushes apart any pair closer than 26px, so no rectangle overlaps another or the headline at any width above 760px (checked over 10 random layouts each at 1440, 1024 and 768).

**Radar → Motion preview (`rvShow`):** when the ring first reaches a rectangle (nearest edge, once per rectangle per pulse) a Motion project from the real project data (`D`, key `motion`, no sub-entries, with a thumbnail) wipes open beside that rectangle in the direction the ring is travelling, with its title and "Motion". One preview at a time; 3.5s cooldown; a shuffled bag cycles through all Motion projects before repeating and never repeats back to back. It places itself away from other rectangles, the headline and the field core, and does nothing if no spot fits or the image fails. Fades after 4.8s or when the pulse ends. Reduced motion: radar and previews are off. Phones (≤760px) have no radar preview; the work strip stays.

**Controls:** header links 16px with a 44px+ hit area (`::before`), back and back-to-top 44px tall with an underline hover, contact links 44px, global 2px focus ring.

## About page (Oct 2026, third pass: built to the supplied reference)

Rebuilt to match a supplied reference image (1536 × 1024) using only real content. Workflow and rules: `.claude/skills/visual-reference-art-direction` (new) with `visual-storytelling-and-diagram-design`.

- **One composition** (`.ax`, a size container; type and spacing in `cqw` with px floors). Desktop grid: left 82fr / right 59fr; rows: hero + What I do, process + tools, Beyond design + production story, Back/Next.
- **Portrait**: `img/port_dark.jpg`, derived from `img/port.jpg` by local person segmentation (macOS Vision) composited on black so it meets the canvas. Not mirrored or reshaped. It spans the hero and process rows behind them, with a radial mask, fine orbit circles and one garnet arc.
- **Shared devices** in every region: index number, 30px garnet rule, spaced uppercase title, hairline frames on the right column. Images are greyscale and regain colour on hover/focus.
- **What I do**: four disciplines with an image from their own projects; descriptors from `D`. Experiments is not in this grid (still in Work).
- **Process**: Observe, Ideate, Create, Refine, traced through the MG Select film using that page's own sentences and stills; one SVG curve computed from the node images.
- **Tools**: inner orbit = tools named on project pages (Resolve, Sony A7 III, Photoshop); outer = listed only (After Effects, Figma, Illustrator). Monograms, not brand logos (none in the repo). Selecting one lists the pages that name it.
- **Beyond design**: images only where one of Pavitra's own photographs fits the word (Photography, Driving, Space and biotech); the rest typographic.
- **Production story**: Times Lifestyle Week 2026 (confirmed by Pavitra, 28 Oct 2026). Story text plus four stills from Personal Reel, all linking to its case study.
- Header: About gets `aria-current` and the existing garnet underline. The navigation stays where it is on every page (not centred as in the reference).

### Corrections after review (Oct 2026)
- **Face clearance:** the orbit is drawn in the portrait's own 800 × 1000 coordinate frame (`.axpi` wraps image, SVG and labels). Rings are larger than the head (radius 372 and 440 around a centre between face and neck). Checked against the real pixels of `port_dark.jpg`: no stroke point within 24 units of the head, minimum distance about 40 units.
- **Name once:** the large signature is the only prominent name in the composition; the intro no longer repeats it, and the header wordmark stays as shared navigation.
- **Logos:** real marks in `img/logos/` (sources and licences in `SOURCES.txt`): Figma, Photoshop, Illustrator, After Effects (gilbarbara/logos, CC0) and DaVinci Resolve (simple-icons, CC0, single-colour, recoloured light for contrast). Blender is not shown because no project or tool list names it. The Sony A7 III is hardware, shown as a separate text line.
- **Process connectors** run edge to edge with arrowheads.
- Removed dead About-era CSS (`.abg2`, `.abph2`, `.abt2`, `.abq`, `.abz`, `.abm`, `.aww`, `.abr`, `.ablbl`, `.ablead`, `.abh`, `.abp1`).

### Background and composition fix (Oct 2026)
- **Cause of the missing background:** the inner-page field lived in a canvas that scrolls away with the first screen and clears itself around every piece of small text. About is almost all small text, so the field was erased; Contact is the home page scrolled to its last section, so its field was in the hero, off screen. Fix: About and Contact now run the same interference field fixed behind the viewport (`body.bgfix`, `FLD.fix`, one mask concentrated toward the right edge instead of per-text clearing). Contact skips the hero field so it is not drawn twice. Phones keep the existing `#rpm` radial pattern, now also on Contact. The About radar pulse was removed because it crossed the portrait and nothing on the page used it.
- **Spacing scale** `--g1..--g5` (8, 16, 24, 40, 72, in `cqw` with px floors). Do not name spacing tokens `--s*`: `--s1` is the surface colour.
- **Composition:** two asymmetric columns (1.45fr / 1fr). Row 1 hero (text column | portrait column) beside What I do; row 2 process beside tools; row 3 interests beside production story; row 4 Back/Next. The portrait has its own column; the name appears once (text column), nothing sits on the portrait but two small labels at its lower corners.
- **Orbit:** one centre, two ellipses, five logos at 72-degree steps alternating outer and inner (outer: Figma, After Effects, Illustrator; inner: DaVinci Resolve, Photoshop). Static by design.

### About motion and type refinement (Oct 2026)
- **Portrait satellite:** the faint ring (r372) is static; a 60-degree red arc with a leading dot travels on r388 around the same centre as a pure CSS animation (`axorbit`, 150s linear, one full turn, no reset). Checked against the portrait pixels: a full turn stays at least 65 units from the head. Off under reduced motion (the arc then sits at its original position).
- **Software orbit:** logos are placed by `axOrbitAt(t)` with `translate` only, so they stay upright. Inner ellipse turns +7 deg/s, outer -4.5 deg/s; logos on one ellipse keep their spacing. Radii (74x50 and 205x166 in a 480x404 box) keep inner and outer logos far enough apart that no logo or label meets another at any phase (swept over 534 phases). One rAF loop, only while the orbit is on screen, the tab is visible and nothing in it is hovered or focused; torn down in `abStop`. Below 440px of orbit width (about 1280 wide and narrower in two columns) and on phones the layout is static / a list.
- **Type levels:** primary ABOUT (clamp 48-76) and the statement (21-24px, 300); secondary section titles (18-20px caps, 300); tertiary titles and software names (15px, 400, white); supporting text 14-15px, 300, `--mute`. The four-word annotation list was folded back into the bio sentence it came from.

### Final refinement (Oct 2026)
- **Orbit diagnosis:** in the file as found, the orbit did animate in the browser (positions changed between two samples at 1536, 1100 and 768 wide). The only silent stop was the hard `AB.o.w>=440` gate: the diagram's box is about 425px at 1280-1330 wide, so it froze there with no explanation. Fixed by sizing from geometry: labels hide (kept for screen readers; the name shows in the detail panel) below 460px of box width, the discs stay at least 56px apart down to 300px, and a swept check at widths 300-600 found 0 overlaps. Phones keep the static list. Source of truth for each logo: `AXT` (ellipse `o`, start angle `a`) plus `AXO` (radii and angular speed); JS sets `translate` only, CSS sets none.
- **One page edge:** `--container` is now 1560px (was 1440) so the header, About content, section titles and Back/Next share `--edge`. This also moves the page edge on every page above 1440px wide.
- **Type levels:** primary ABOUT and the statement; section titles are sentence case 24-28px (no longer tracked caps); item titles 17-19px; supporting text 15px, metadata in DM Mono 14px. Numbering and the location annotation were removed.
- **Copy:** facts come from the project pages and the resume. The resume says B.Des, expected 2027, which does not confirm "fourth year", so the year of study is no longer stated. "Available for full-time roles." replaces "Looking for a full-time role."

## Visual discipline page (Oct 2026): C + A
`visualPage()` / `vzInit()` in `index.html` replace the card wall for `#/work/visual`. One column of three titles (from `D`), one 4:5 plate, one aperture ring that relocates per project. Text, routes, meta and images come from `D`; `VZ` holds only the editorial bits: crop, ring geometry, one-word tag.
- **Ring geometry:** Chainsaw Man is fitted to the moon rim measured on `csmCard.jpg` (centre 50.0% / 32.1%, diameter 68.3% of width). ARCEON sits on the card's own concentric mark; The Hold sits in the dark foreground clear of the title and face.
- **ARCEON crop:** the landscape `arcId.jpg` is cropped to 4:5 on the card (`object-position:56% 50%`). The full image is on the case study.
- **Selection:** hover (mouse only), focus, arrow keys. Click and Enter open. Touch and pen: first tap on an unselected title selects, a second tap or the visible "Open project" link opens. No `preventDefault` for mouse or keyboard.
- **RGB split** runs once (~0.75s) on change; none in reduced motion, where plate and ring change instantly.
- No new library or font.
