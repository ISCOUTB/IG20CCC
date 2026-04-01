# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**20CCC Instagram Template Generator** is a lightweight, browser-based tool for creating institutional graphics for the 20th Colombian Computer Science Congress (20CCC 2026). It's a single-file, zero-dependency application built with vanilla HTML, CSS, and JavaScript that allows real-time preview and editing of multiple social media slide templates.

### Key Philosophy
- **100% browser-based**: No build tools, databases, or backend required
- **Vanilla stack**: Pure HTML/CSS/JavaScript for maximum portability
- **Live preview**: Reactive data binding between form inputs and visual output
- **Institutional branding**: Pure white backgrounds across all slides, navy/green corporate palette, Inter font

## Architecture

### Single File Structure: `instagram-ad-template.html`

The entire application is contained in one HTML file (~1915 lines) with three integrated sections:

#### 1. **CSS Styling** (Lines 8–1350)
- **CSS Variables** define the design system:
  - Colors: `--navy-900`, `--cyan-500`, `--coral-500` (green `#7ab648`), `--border-mid`, `--shadow-card`, `--footer-dark`
  - Typography: `--ui-font` (Inter via Google Fonts, fallback Segoe UI)
  - Layout: `--frame` (1080px width for Instagram)
- **localStorage persistence**: form values saved under `ccc_<target>` keys; reset button clears and reloads
- **Layout System**: CSS Grid for main shell (`app-shell`), Flexbox for components
- **Component Classes**: `.controls` (left sidebar), `.poster` (preview canvas), `.preview-area` (right side)
- **Responsive Scaling**: Posters scale dynamically based on viewport using `transform: scale()` (see `updatePosterScale()`)

#### 2. **HTML Structure** (Lines ~1354–1865)

**Left Panel (`.controls`):**
- **Format Toggle**: Radio buttons to switch between 4:5 (Instagram, `portrait-aspect`) and 9:16 (Stories/Reels, `story-aspect`) aspect ratios
- **Tab Navigation**: 5 tabs trigger form panel visibility
  - `data-view` attribute links buttons to panels
  - Active tab is highlighted with `.tab-button.active`
- **Form Panels**: 5 sections with `data-panel` attributes:
  1. **Cover** (`cover`): Title, subtitle, location, dates, sponsor labels
  2. **Benefits** (`benefits`): 3-item benefits list with custom titles/descriptions
  3. **Topics** (`topics`): 8-card grid for research topics
  4. **Events** (`events`): Call for papers, up to 5 event types with title/copy/date
  5. **CTA** (`cta`): Final call-to-action with deadline breakdown and action buttons
- **Input Fields**: All inputs use `data-target` attribute (e.g., `data-target="cover-title"`) to bind to preview

**Right Panel (`.preview-area`):**
- 5 preview frames (one per tab), each containing a `.poster` article
- Frames toggle visibility with `.preview-frame.active`
- Each poster has 5 distinct design templates styled with different CSS classes

#### 3. **JavaScript Logic** (Lines ~1870–1983)

**Key Functions:**

1. **`updatePosterScale()`**: Scales all posters to fit available viewport
   - Calculates max scale from available width/height
   - Applies `transform: scale()` to all `.poster` elements

2. **`setView(view)`**: Switches active tab and form panel
   - Toggles `.active` class on buttons, panels, and frames
   - Calls `updatePosterScale()` after switch

3. **`setFormat(format)`**: Switches poster aspect ratio
   - Adds/removes `.portrait-aspect` or `.story-aspect` classes
   - Updates preview label text

4. **`updateBoundText(target, value)`**: Reactive data binding
   - Finds all elements with matching `data-bind` attribute
   - **Special case**: `cta-deadline-copy` splits lines and creates structured HTML
   - Other multiline text replaces `\n` with `<br>`

## Design System

### Colors
- **Navy/Blue**: `#041a47` (dark), `#08265f` (medium), `#10397d` (lighter)
- **Green (Coral)**: `#7ab648` (institutional primary — icon fills, borders, date badges)
- **Cyan**: `#24d7ea` (topic-chip, bio-chip accents)
- **Borders**: `--border-light` `rgba(16,57,125,0.1)` / `--border-mid` `rgba(16,57,125,0.18)` — use mid on white backgrounds
- **Surface tint**: `#f0f5fd` — used for brand-pill, glass-panel, deadline-box, springer-badge to differentiate from white poster backgrounds
- **Poster backgrounds**: `#ffffff` for all 5 slides (hero, benefits, topics, events, cta)

### Aspect Ratios
- `portrait-aspect`: 1350px height (4:5 Instagram)
- `story-aspect`: 1920px height (9:16 Stories/Reels)
- Base width: 1080px (fixed)

## Common Development Tasks

### Editing Template Content

1. Find the input field in the left panel with label (e.g., "cover-title")
2. The `data-target` attribute tells you what element to update
3. Find the corresponding `<div data-bind="cover-title">` in the preview
4. Changes sync automatically via the `input` event listener

### Adding a New Slide Template

To add a new slide:

1. **Add a tab button** in controls with `data-view="newslide"`
2. **Add a form panel** with `data-panel="newslide"` and input fields with `data-target` attributes
3. **Add a preview frame** with `data-frame="newslide"` containing a `.poster` article with `data-bind` elements
4. **Style the `.newslide` poster class** in CSS

The JavaScript will automatically wire up tab switching, form binding, and data synchronization.

### Modifying Colors

Edit `:root` CSS variables at the top of `<style>` to change the entire design system globally.

### Exporting Slides

Users right-click the `.poster` element and use browser dev tools or extensions to capture as PNG at full resolution (1080px width).

## Important Implementation Details

### Data Binding System
- `data-bind` ↔ `data-target` pairing synchronizes content
- One input maps to one or more preview elements (1:N)
- Special parsing only for `cta-deadline-copy` (splits on newlines and colons → structured `.date-row` HTML)
- Other fields use `textContent` for safety (no HTML injection risk)

### Aspect Ratio Switching
- `.poster` width is fixed at 1080px
- Height changes: 1350px (portrait) or 1920px (story)
- `.preview-frame` height is recalculated dynamically in `updatePosterScale()`
- Resize listener is debounced (120ms) via `_resizeTimer`

### Layout: Brand-pill and Content Padding
- `.brand-pill` is `position: absolute; top: 26px; left: 26px` on the poster — its visual bottom is ~218px
- All inner content containers (`.benefits-inner`, `.topics-inner`, `.cta-inner`, `.events-inner`) must have `padding-top ≥ 230px` to avoid overlapping the brand-pill
- Current values: base 230px / portrait 250px / story 270px
- If the brand-pill logo size changes, recalculate: brand-pill_height = 26 + 24 + img_height + 24

### White Background Contrast Rules
- All 5 poster backgrounds are `#ffffff`
- Cards on white (`.benefit-item`, `.topic-card`, `.event-row`): use `border: 1px solid var(--border-mid)` + `box-shadow: var(--shadow-card)`
- Panels on white (`.brand-pill`, `.glass-panel`, `.deadline-box`, `.springer-badge`): use `background: #f0f5fd` + `border: ... var(--border-mid)`
- Never use plain `rgba(255,255,255,0.x)` backgrounds on these elements — they become invisible on the white poster

### Performance
- Scaling uses `transform: scale()` (GPU-accelerated)
- No CSS transitions on data binding (instant updates)
- Responsive layout uses CSS Grid and Flexbox

## Notes

- **No external dependencies**: Vanilla HTML/CSS/JavaScript only
- **Single file**: All code in `instagram-ad-template.html`
- **Local images**: Logo files (20ccc, SCO2, UTB, Escuela) are committed as JPEG/PNG in repo root
- **Language**: Interface and defaults in Spanish
