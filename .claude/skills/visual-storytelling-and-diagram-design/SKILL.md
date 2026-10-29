---
name: visual-storytelling-and-diagram-design
description: Decide how information should be shown (diagram, image sequence, typography, spatial composition or plain text), pick one diagram structure, source and verify its relationships, and build it responsively in HTML, CSS and SVG inside an existing design system. Use when turning content such as disciplines, projects, tools or process into an interactive visual, or when reviewing a diagram that feels decorative.
---

# Visual storytelling and diagram design

Complements, never replaces: `human-centered-web-design` (audience, hierarchy), `color-intelligence` (contrast), `taste-critique` (judging screenshots), `emil-design-eng` (interaction polish). Use this one for the step they skip: **what form should this information take, and is every mark in it true?**

## 1. Choose the form (decide before drawing anything)

Ask of each piece of content: *what would the visitor do or understand with it?*

| If the content is... | Use | Not |
|---|---|---|
| One claim, a person, a stance | Typography (one large statement) | A diagram |
| Things that are seen (work, photos, products) | Real images in a composed field | Icons, cards of text |
| A sequence where order carries meaning and each step changes something | Image sequence or timeline | A circle-and-arrow flowchart |
| Membership: which items belong to which group | Spatial composition (columns, clusters), grouping by position | Lines for membership |
| A few verified links *between* items in different groups | A connector, drawn only for those links | A fully connected network |
| Many-to-many data with real counts | A true chart (see `dataviz`) | Decorative nodes |
| Facts with no structure (role, place, dates) | Concise text | Anything interactive |

Rules: one dominant form per page. If two sections need the same form, merge them. If a diagram would say what one sentence says, write the sentence.

## 2. Pick the structure

- **Grouping by position beats lines.** Put group members together; reserve lines for the exceptions.
- **A connector is a claim.** It must name its basis in plain words ("same event", "credited role"). If you cannot write the basis, delete the line.
- Prefer columns, strips, or a mosaic over radial maps and mind maps. Avoid hub-and-spoke unless one thing really is the hub.
- Size encodes something or nothing; never let size imply importance by accident. Equal crop ratios within a group.

## 3. Source and verify content

1. Single source of truth: the project's data object (here `D` in `index.html`). Render from it; do not retype titles, roles or images.
2. Relationships live in one small, named, editable list (here `ABREL`), each with `a`, `b`, `basis`. They are **curated**, not inferred. Never generate links by keyword matching and present them as fact.
3. Before shipping each link, read the two project pages and confirm the basis in their text. Record the basis string on screen.
4. Tools and credits: show a tool beside work only where the page names it. Otherwise list the tool as plain text with no connection.
5. Placeholders (`[ADD …]`) never render.
6. Report anything unverifiable instead of drawing it.

## 4. Build it (HTML, CSS, SVG)

- HTML for everything read or clicked (real `<a>`, `<button aria-pressed>`); SVG only for connectors, `aria-hidden`, `pointer-events:none`.
- Layout in CSS grid/flex. Compute connector paths from DOM rects (`getBoundingClientRect`) in one function, re-run via `ResizeObserver` + `requestAnimationFrame`. Never hardcode coordinates.
- Every relationship must also exist in text, visible without hover (a list under the diagram). The SVG is reinforcement.
- Interaction states: hover and keyboard focus do the same thing; tap does too. One selection state, one highlight mechanism. No tooltips that cover content.
- Mobile: re-compose, don't scale. Replace lines with a grouped list and the text notes.
- `prefers-reduced-motion`: no transitions beyond opacity at 0s; content complete without animation.
- Clean up: disconnect observers and cancel rAF on route change.
- Avoid custom-property collisions with the host design system (check `:root` before naming).

## 5. Stay inside the design system

Take colours, type, radii, easings and spacing from the existing tokens. Add no new colour, no new typeface, no glow, no gradient. If the system already uses hairlines sparingly, do not add boxes. A new treatment is allowed only if removing it would lose information.

## 6. Evaluate

Hierarchy: one thing is read first, second and third without effort. Readability: running text 16px+, nothing under 14px, contrast checked. Noise: count distinct treatments (borders, chips, labels, motion types); fewer than four. Interaction: each one explains or reveals; delete the rest.

## 7. Reject decorative complexity

Delete, in this order: connectors with no stated basis; labels that repeat the heading; numbering; second diagrams that restate the first; ambient motion; borders around things already separated by space. After deleting, do not refill the space with another ornament; enlarge the real content (images, type) instead.

## 8. Checklist before reporting

1. One dominant visual idea? 2. Every connector has a visible basis, verified against source? 3. Real images carry the composition? 4. Works with keyboard, touch, reduced motion? 5. Still convincing with all effects off? 6. Would a recruiter understand who this is and what they do in ten seconds? Run `taste-critique` on screenshots at 1440, 768, 390 and fix failures before saying done.

## 9. Geometry, logos and labels (added after the About rebuild)
- Draw connectors from computed DOM rectangles, edge to edge; add arrowheads when order matters; never route a line through text.
- Orbit or ring graphics are decoration unless they encode something. If they encode "named on a project page" versus "listed only", say so in one visible sentence and use a second visible cue (solid versus dashed ring) so it works without hover.
- Logos: real marks from a licensed source, stored locally, sized optically (tall marks like Figma smaller), never initials in a circle. Hardware is not software.
- Labels sit on a canvas-coloured background where a line passes behind them.
