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
| Quiet text | `#8C98A6` | 7.2 | Secondary text, captions |
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

Concept: the site reinterprets a compositing workspace (the Fusion-inspired origin) as one instrument. The radar already draws a ring every 150px from the headline's full stop. The field keeps that scale: **A** is the radar's own rings, drawn finely (10px) with a major ring every 15th, and **B** is a second emitter (10.5px). Where the two overlap they interfere into curved fringes. Both are warped slightly by low-frequency distortion that grows with radius, so the lines bend instead of forming perfect circles.

Rules:
1. **Drawn once, never animated** (`fieldDraw`, about 5 ms). The radar's garnet pulse is the only moving element, and it is the same ring system, so nothing competes.
2. **Asymmetric density.** The field gathers where the radar's origin is (bottom-right of the home hero) or in the top-right of a page hero, and is masked to nothing elsewhere. The viewport edge crops it on purpose.
3. **Reading zones are adaptive.** `fieldHoles` measures every piece of text under 40px and the field clears itself, with soft edges, around it. Display type (40px and up) may sit over the field. Do not put the field behind small text by adjusting opacity.
4. **Intensity follows content:** home 1.5, discipline and case-study pages 1.3 and 1.15, About 0.55.
5. **Home:** floating thumbnails are kept out of the field's core (`HX.fz`) so captions never land on it.
6. Canvas layers never take pointer events.

Removed as decorative chrome: the 48px full-screen grid, the plus-sign cursor glow, the cursor spotlight orb, the warm background glow, the plus marks and dots the radar pulse used to draw, the scrolling marquee of discipline names, the redundant "Scroll" hint on the ARCEON page, and the tiny tracked-uppercase section labels on About (now ordinary 14px labels).

Kept on purpose: the radar and its pulse, the intro timeline with its timecode (the strongest literal editing-software reference), the Fusion node-graph labels on the Personal Reel page (project content), tile project counts, and all genuine metadata.

Not touched (mobile phase): the phone radar with its crosshair lines.

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
| Note | `--t-note` | 13px DM Mono, `--tr-note`. Margin notes and metadata. |

## Spacing, layout, images

- Space: `--sp-1…10` on a 4pt base. Page gutter `--gutter`, container `--container` (1440px), 12-column grid with 24px gaps (as today).
- Image ratios already in use: portrait 4/5, story 9/16, film 16/9, photo 3/2, UI 4/3 (`--ar-*`). Variety is a feature; do not force one ratio.
- Breakpoints (cannot be variables): 560 phone, 760 nav/header, 900 layout collapse, 1100 sticky-rail layouts. Consolidate in the responsive phase, not before.

## Evidence behind the values (Oct 2026 audit of `index.html`)

- Font sizes in use: 15px ×67, 13px ×22, 16px ×19, 12px ×17, 14px ×17, plus ~20 distinct fluid display sizes. Tokens collapse them into one scale.
- Weight 300 ×51, 400 ×31. Radius 3px ×20, 6px ×16, 999px ×11.
- Transition durations in use: .5s ×20, .4s ×15, .35s ×11, .2s ×10, .3s ×9, .9s ×8, .6s ×6, .8s ×6.
- Colour was already well tokenised (`--ink`, `--mute`, `--line`, `--o`, `--e` account for most var() uses), which is why the palette change was a handful of root values plus some hardcoded rgba/hex replacements.
