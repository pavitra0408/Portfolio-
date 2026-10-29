---
name: visual-reference-art-direction
description: Rebuild a page so its rendered result matches a supplied visual reference image, using only real content and assets. Analyse the reference as a composition (regions, proportions, crops, hierarchy), map real assets to each region, build it in the existing stack, then screenshot at the reference's aspect ratio and iterate until material differences are gone. Use when a user supplies a reference image or mock as the visual target.
---

# Visual reference art direction

Pairs with `visual-storytelling-and-diagram-design` (what each diagram may claim) and `taste-critique` (judging screenshots). This skill is about **fidelity to a supplied reference without faking content**.

## 1. Read the reference as a composition
Write down, in reference pixels and as fractions of width/height:
- Canvas size and aspect ratio. Outer margins.
- Every region: its box, what it contains, what it aligns to.
- The visual centre of gravity (usually the largest image) and what overlaps it.
- Type roles: the one oversized word, section titles, labels, body. Approximate size, weight, case, tracking.
- Line language: rule weights, circles/orbits, short accent dashes, frames. Count them; they must stay few.
- Accent usage: where the accent colour appears and at what size. Small accent text usually fails contrast; move the accent to a rule or dot.
- Image treatment: monochrome or colour, crop ratios, how images meet the dark canvas (fades, masks).

## 2. Separate design from content
A generated reference contains invented people, logos, tools, labels and stories. Keep its **structure**; replace its **content** with the real equivalents:
- Portrait: the real person only. If the real photo has a light background, derive a dark-ground version locally (e.g. on macOS, Vision person segmentation composited on black) and save it as a new asset beside the original. Never mirror, warp or upscale the face.
- Tools: only tools the person lists. Use a typographic monogram in the site palette when no licensed logo asset exists in the repo; do not hotlink brand logos.
- Labels and microcopy: build them from real sentences (bio, project text). Delete reference microcopy that has no real source.
- Stories: pair images with text only where the source states the connection. Otherwise caption the images with their own projects.

Keep a mapping table: reference element → real asset or text → source location → confidence.

## 3. Build the composition, not components
- One container with `container-type: inline-size`; size type and spacing in `cqw` with `clamp()` floors (14px minimum for any label, 16px for running text).
- One desktop grid whose columns and rows follow the measured fractions. Let the anchor image span rows and sit behind neighbouring regions when the reference overlaps them; fade it with a mask, not a box.
- Shared devices for every region (e.g. index number + short accent rule + spaced title). Same rule weight, same node style, same label style across all diagrams.
- Diagrams: HTML for anything read or clicked; SVG for paths only, computed from DOM rects or a fixed `viewBox` with `vector-effect: non-scaling-stroke`.

## 4. Compare at the rendered level
1. Screenshot at the reference's aspect ratio (e.g. 1536×1024) and at the user's common widths.
2. Compare region by region: position, size, crop, type size, line weight, spacing, accent.
3. List the three largest differences, fix them, screenshot again. Repeat until the remaining differences are small or caused by real-content limits.
4. Never claim a match you did not see. Report remaining differences and why they remain.

## 5. Collapse deliberately
Below the desktop breakpoint, keep the anchor image prominent (not full-screen), keep the art-direction devices, turn spatial diagrams into ordered lists with the same nodes, and use snap rails rather than shrinking tiles until unreadable. No horizontal page overflow; 44px targets; reduced motion keeps everything readable.

## 6. Reject
Glows, neon, random particles, fake technical numbers, invented proficiency, decorative dots that mean nothing, a second visual language per section, and any content the person did not supply.

## 7. Hard rules learned on this project
- **Exclusion zones.** Orbit lines, connectors, ring arcs and labels must not cross a face or head. Draw geometry in the image's own coordinate system (an SVG `viewBox` equal to the image size, wrapped with the image in one `position:relative` box that is sized by the image, never by a taller grid cell). Make rings larger than the head instead of masking them, then verify with pixels (see `screenshot-visual-qa`).
- **Name once.** When the composition has a prominent name or signature, remove other visible repeats inside the composition (intro sentences, captions). Keep the shared header wordmark, `alt` text and metadata.
- **Genuine logos only.** No typed abbreviations. Source marks from a licensed repository (for example CC0 collections), store them locally, record the source in a text file beside them, include only tools the owner actually lists, and keep hardware (a camera) out of a software diagram. Single-colour marks may be recoloured for contrast; say so.
- **Connectors.** Edge to edge between nodes, one arrowhead per step, and a test that no connector point lies inside a label.
