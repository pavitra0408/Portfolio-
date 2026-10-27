# Design system — Direction A: the Annotated Index

Pavitra's identity is the constraint (see HANDOFF.md and project memory). This file records the tokens added to `index.html` (`:root`, "DESIGN TOKENS" block) and the rules for using them. **Tokens are defined but not yet consumed**, so adding them changed nothing visually. Adoption happens phase by phase, on `design/direction-a`.

## Idea

The thesis is "I notice how things work." Direction A makes that literal: one continuous numbering across all work, and short margin notes beside media in a human voice. Notes may only be built from Pavitra's existing wording (project blurbs `m`, headings, reflections) or text she approves. Never invent claims, credits or metrics.

## Rules that matter more than the values

1. **Orange (`--accent`) means "current" or "next action".** Active nav item, the project being hovered or focused, the primary link. Never decoration.
2. **Weight 300 is the identity.** Headings and body are 300; 400 only for small UI text that needs it; 500 is exceptional.
3. **Two voices only.** Outfit for everything read; DM Mono for numbers and margin notes. Do not introduce a third.
4. **Not everything shouts.** Use at most one `--t-hero` or `--t-display` element per screen. Section eyebrows may be uppercase; margin notes are sentence case.
5. **Radius:** `--r-2` (3px) for images and cards, `--r-pill` for nav and buttons. Odd values in the CSS today (14px, 10px, 4px, 99px) should converge on these when touched.
6. **Motion:** two easings (`--ease-out` for entrances, `--ease-inout` for transitions), four durations. Every interaction needs a reduced-motion alternative; content must be usable before any animation finishes.
7. **Touch targets** are at least `--hit` (44px).
8. **No tints, no beige, no brown, no pastels.** The page is pure black (`--tint:0%`). The old warm peach/blue background glows and per-discipline page washes were removed.

## Palette (Oct 2026, replaces the earlier charcoal / warm off-white / pastel set)

Idea: print process colour. Black ink on pure white, with cyan and magenta from the CMYK set, a mid green, an ultramarine, and orange as the single "act" colour. Contrast is measured on `#000`.

| Role | Value | Contrast on black |
|---|---|---|
| Page | `#000000` | n/a |
| Surfaces | `#0C0C0D`, `#161617` | n/a |
| Text | `#FFFFFF` | 21.0 |
| Quiet text | `#A6A6A6` (neutral grey) | 8.6 |
| Accent (current / next action) | `#FF5A1F` | 6.7 |
| Motion | `#F5139A` magenta | 5.4 |
| Visual | `#00AEEF` cyan | 8.3 |
| UI / UX | `#12B76A` green | 8.0 |
| Photography | `#FFFFFF` | 21.0 |
| Experiments | `#5B63FF` ultramarine | 4.7 |
| Typography (hidden) | `#A6A6A6` placeholder | 8.6 |

Discipline colours appear only as small markers (tile squares, hairlines, section numbers, hover). They never wash the page.

Left alone on purpose: colours inside the work itself (ARCEON brand navy, the DigiLocker and Recurring app mockups, the Recurring notification timeline's amber and red). Those are project content, not site chrome.

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
