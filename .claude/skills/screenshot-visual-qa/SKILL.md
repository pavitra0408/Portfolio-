---
name: screenshot-visual-qa
description: Verify a UI change by rendering it and measuring it, not by reading the code. Use after any layout, image, diagram or typography change, and before reporting a visual task as done. Requires screenshots at matching viewports, written mismatches, DOM-level checks for overlap and overflow, and an honest list of what was not tested.
---

# Screenshot visual QA

Complements `visual-reference-art-direction` (what to match) and `visual-storytelling-and-diagram-design` (what a diagram may claim). This skill is the proof step.

## Loop
1. **Baseline** before editing: screenshot the affected routes at the sizes that matter (reference size, 1440, 1024, 768, 390, 320).
2. **Edit** one concern at a time.
3. **Wait for the page to settle**: preloaders, route transitions, lazy images (`loading=eager` then await `complete`), fonts (`document.fonts.ready`). A screenshot taken mid-transition is not evidence.
4. **Screenshot, then write the three largest mismatches** against the reference or the intent, in words, with positions.
5. **Fix, re-render, repeat** until the remaining differences are small or caused by content limits.

## Measure, do not eyeball
Run in the page (Playwright `evaluate`) after every change:
- Horizontal overflow: `document.documentElement.scrollWidth > innerWidth`, then list offenders by `getBoundingClientRect`.
- Text under 14px that is visible.
- Broken images: `complete && naturalWidth === 0` after forcing eager load (ignore intentionally lazy-by-hover images).
- Overlap between labels and between labels and graphics: compare bounding boxes pairwise; for curves, sample `getPointAtLength` and test against label rects.
- Exclusion zones (faces, headlines): derive the zone from the real pixels (draw the image to a canvas, collect non-background pixels, dilate) and test every stroke point and label box against it. Report the minimum distance.
- Target size ≥ 44px for controls; keyboard focus ring visible after a real `Tab` press (programmatic `.focus()` can hide `:focus-visible`).
- Reduced motion: emulate it, reload, confirm content is visible and transitions are ~0.
- Console errors and failed requests. A flaky local server can reset connections; reload before blaming the code.

## Reporting
List only what you ran: pages, widths, interactions, counts. Name what was not tested (real devices, screen readers, other browsers). Never write "pixel-perfect" unless a pixel comparison was actually computed.
