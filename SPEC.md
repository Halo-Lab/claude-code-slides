# Technical Specification: Webinar Slide Engine

**Created**: 2026-03-31
**Status**: Draft

## Overview

A zero-dependency HTML/CSS/JS slide presentation engine for a 45-minute Halo Lab webinar. Slides are authored directly in HTML. The engine handles keyboard navigation, entrance animations, and fullscreen video playback. Designed for screen-sharing via Teams.

## Goals

- Open `index.html` in a browser and present — no server, no build step
- Halo Lab branded (Suisse Intl font, dark theme, yellow/blue/purple accents)
- Smooth, professional slide transitions
- Fullscreen video playback for pre-recorded demo walkthroughs
- Simple enough to hand-edit slides in HTML

## Non-Goals

- Not a reusable presentation framework
- No markdown/YAML parsing layer
- No presenter view, speaker notes, PDF export (v2)
- No build tooling, npm, or bundler

## Architecture

### File Structure

```
web-slides/
├── index.html          ← entire engine + all slides
├── logo.svg            ← Halo Lab full logo (white)
├── icon-halo.svg       ← Halo Lab icon only
├── fonts/
│   ├── SuisseIntl-Light.woff
│   ├── SuisseIntl-Regular.woff
│   ├── SuisseIntl-Medium.woff
│   ├── SuisseIntl-SemiBold.woff
│   └── SuisseIntl-Bold.woff
├── videos/             ← pre-recorded demo .mp4 files (added later)
├── PLAN.md
└── SPEC.md
```

### Single-File Architecture

`index.html` contains three sections:

```
<style>    — all CSS (fonts, layout, animations, slide types)
<body>     — all slides as <section> elements
<script>   — navigation, animation, video control logic
```

No external CSS or JS files. Font files and images are referenced by relative path.

## Technical Design

### HTML Slide Structure

```html
<div class="deck">
  <!-- Each slide is a <section> -->
  <section class="slide" data-animate="fade">
    <h1>Title</h1>
    <p>Content</p>
  </section>

  <!-- Video slide -->
  <section class="slide slide--video" data-animate="fade">
    <video src="videos/demo-compliance.mp4" data-fullscreen></video>
  </section>
</div>

<!-- Slide counter overlay -->
<div class="slide-counter">1 / 12</div>
```

### Slide Types (CSS classes)

| Class | Purpose | Layout |
|-------|---------|--------|
| `.slide` | Base slide | Centered flex column, full viewport |
| `.slide--title` | Title/intro slide | Large centered heading, logo, subtitle |
| `.slide--section` | Section divider | Large text, accent background |
| `.slide--content` | Text content | Left-aligned, max-width for readability |
| `.slide--two-col` | Two columns | CSS grid 1fr 1fr with gap |
| `.slide--video` | Video embed | Video centered, controls visible |
| `.slide--cta` | Call to action | Centered, prominent styling |
| `.slide--image` | Full-bleed image | Image covers slide area |

### CSS Design Tokens

```css
:root {
  --bg-dark:    #02021e;
  --blue:       #3719ca;
  --yellow:     #fdc448;
  --purple:     #7057d6;
  --white:      #ffffff;
  --white-70:   rgba(255,255,255,0.7);
  --grey:       #9a9aa5;
  --grey-light: #f5f5f7;
  --font:       'Suisse Intl', Arial, sans-serif;

  --slide-width:  1920px;
  --slide-height: 1080px;
}
```

### Navigation (JS)

**Controls:**
- `→` / `Space` / `PageDown` — next slide
- `←` / `PageUp` — previous slide
- `Home` — first slide
- `End` — last slide
- `F` — toggle video fullscreen (when on video slide)

**State:**
- `currentSlide` index (0-based)
- `goTo(n)` — navigate to slide n, trigger animation, update counter
- Navigation disabled during animation (300-400ms lock)
- Hash-based URL tracking (`#slide=3`) for easy resume

### Animation System

Animations are opt-in per slide via `data-animate` attribute.

| Value | Effect | Duration |
|-------|--------|----------|
| `fade` | Opacity 0→1 | 400ms |
| `slide-up` | Translate Y 40px→0 + fade | 400ms |
| `slide-left` | Translate X 60px→0 + fade | 400ms |
| `zoom` | Scale 0.95→1 + fade | 400ms |
| (none) | Instant show | 0ms |

**Implementation:** All slides are `display:none` except the active one. On transition:
1. Outgoing slide gets `display:none`
2. Incoming slide gets `display:flex` with animation class
3. Animation class removed after completion (via `animationend` event)

### Video Fullscreen

**Behavior:**
- Video slides show the `<video>` element with native controls (play/pause/seek/volume)
- Click the video or press `F` → enter browser Fullscreen API
- `Escape` → exit fullscreen (browser native)
- A small toggle button in the corner: "Expand" / "Shrink" to switch between full-slide and 70%-width centered view
- When navigating away from a video slide, video auto-pauses
- When navigating back, video stays paused at last position (user resumes manually)

**Fullscreen API:**
```js
video.requestFullscreen()  // enter
document.exitFullscreen()  // exit
```

Arrow keys are suppressed while video is playing to prevent accidental slide change.

### Slide Counter

- Fixed bottom-right: `3 / 12` in small muted text
- Updates on every navigation
- Semi-transparent, non-distracting

### Responsive Scaling

Target: 1920x1080 (16:9). The `.deck` container uses `transform: scale()` to fit the actual viewport while maintaining aspect ratio. This means slides always look the same regardless of screen resolution.

```css
.deck {
  width: 1920px;
  height: 1080px;
  transform-origin: top left;
  /* JS calculates scale factor based on window size */
}
```

## Assumptions

| Assumption | Rationale | Impact if Wrong |
|------------|-----------|-----------------|
| Chrome browser | Screen-sharing via Teams on desktop | Minor cross-browser fixes |
| 1920x1080 display | Standard laptop/monitor for webinar | Scale factor adjusts |
| Local file:// access | No server, just open HTML | Font loading may need `--allow-file-access-from-files` or simple HTTP server |
| Videos are .mp4 H.264 | Universal browser support | Transcode needed |
| Under 30 slides | Single webinar | No performance issue |

## Scope

### In Scope
- Slide engine (navigation, animations, counter, scaling)
- 8 slide type templates (title, section, content, two-col, video, cta, image)
- Fullscreen video with resize toggle
- Halo Lab branding (colors, font, logo)

### Out of Scope
- Presenter view / speaker notes
- PDF export
- Remote clicker / mobile control
- Build tooling / npm
- Slide content authoring (Phase 3 in PLAN.md — separate step)

## Open Questions

- [ ] Will videos need captions/subtitles?
- [ ] Exact number of slides TBD (depends on webinar outline finalization)
- [ ] Demo 2 replacement topic still TBD (per webinar CLAUDE.md)
