# Implementation Plan: Webinar Slide Engine

**Created**: 2026-03-31
**Status**: Draft

## Overview

A plain HTML/CSS/JS slide presentation engine for a 45-minute webinar: "How to Use Claude Code to Solve Real-Life Founders' Problems". Halo Lab branding, keyboard navigation, light animations, and fullscreen video support.

## Scope

### In Scope
- Single `index.html` with inline CSS/JS (zero dependencies, just open in browser)
- Keyboard navigation (arrow keys, space) + click controls
- Slide counter / progress indicator
- Light entrance animations (fade, slide-in) on slide transitions
- Video slides: embed local `.mp4`/`.webm`, click to toggle fullscreen, option to scale down
- Halo Lab branding: colors, font, logo
- Responsive to screen-share resolution (1920x1080 target)

### Out of Scope (v2)
- Presenter view / speaker notes
- PDF export
- Remote control / mobile clicker integration
- Build tooling (no bundler, no npm)

### Non-Goals
- Not a reusable framework — optimized for this one webinar
- No markdown parsing — slides are authored directly in HTML

## Approach

Single HTML file with embedded CSS and JS. Each slide is a `<section class="slide">` element. JS handles navigation, animations, and video fullscreen. CSS handles layout and transitions.

Why single file: easy to open anywhere, easy to screen-share, no server needed, no build step.

## Brand Tokens

```
--bg-dark:    #02021e
--blue:       #3719ca
--yellow:     #fdc448
--purple:     #7057d6
--white:      #ffffff
--grey:       #9a9aa5
--grey-light: #f5f5f7
--font:       'Suisse', Arial, sans-serif
```

## Phases

### Phase 0: Scaffold + Navigation

**Goal**: Working slide deck with 3 placeholder slides, keyboard/click navigation, slide counter.

**Files**:
- Create: `index.html`

**Tasks**:
- HTML structure: slides container with `<section class="slide">` elements
- CSS: fullscreen layout, slide sizing (1920x1080 aspect), centering, brand colors/font
- JS: arrow key + space navigation, slide counter display (e.g. "3 / 12")
- 3 placeholder slides to test with

**Done Means**:
- [ ] Open `index.html` in browser, see first slide
- [ ] Arrow keys / space navigate between slides
- [ ] Slide counter updates correctly
- [ ] Halo Lab colors and font applied

---

### Phase 1: Animations

**Goal**: Light entrance animations when transitioning between slides.

**Depends on**: Phase 0

**Files**:
- Modify: `index.html`

**Tasks**:
- CSS transitions/keyframes for slide entrance (fade + slight translate)
- Different animation options via data attributes (e.g. `data-animate="fade"`, `"slide-up"`, `"zoom"`)
- Smooth, subtle — not distracting for a professional webinar

**Done Means**:
- [ ] Slides transition with animation instead of hard cut
- [ ] At least 2-3 animation variants available via data attribute
- [ ] Animations feel professional, ~300-400ms duration

---

### Phase 2: Video Slides

**Goal**: Slides that contain video with fullscreen toggle and resize option.

**Depends on**: Phase 0

**Files**:
- Modify: `index.html`

**Tasks**:
- Video slide type: `<section class="slide slide--video">` with `<video>` element
- Click on video → browser Fullscreen API
- Press Escape or click → exit fullscreen
- Toggle button or key to switch between "fit to slide" and "smaller/centered" view
- Video controls visible (play/pause/seek)
- Auto-pause video when navigating away from slide

**Done Means**:
- [ ] Video plays inline on the slide
- [ ] Click or keypress enters browser fullscreen
- [ ] Can exit fullscreen cleanly
- [ ] Video pauses when navigating to another slide

---

### Phase 3: Slide Templates + Content Authoring

**Goal**: Build out the actual webinar slides using the engine.

**Depends on**: Phase 0, 1, 2

**Files**:
- Modify: `index.html`

**Tasks**:
- Title slide (Halo Lab logo, webinar title, presenter name)
- Section divider slides
- Content slides (text + optional image/icon)
- Two-column layout slides
- Code/terminal screenshot slides
- Video demo slides (compliance demo, FHIR demo)
- CTA slide
- Q&A / closing slide

**Done Means**:
- [ ] Full slide deck matches webinar outline
- [ ] Videos are embedded and play correctly
- [ ] All slides look polished at 1920x1080

## Dependency Graph

```
Phase 0 (Scaffold + Nav)
    ↓         ↓
Phase 1    Phase 2
(Animations) (Video)
    ↓         ↓
    └────┬────┘
         ↓
Phase 3 (Content)
```

## Checkpoint Protocol

After each phase:
- [ ] Open in Chrome, test at 1920x1080
- [ ] All navigation works
- [ ] No console errors
- [ ] User confirms "proceed"
- [ ] Commit
