# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Single-file HTML/CSS/JS slide engine for a Halo Lab webinar: "How to Use Claude Code to Solve Real-Life Founders' Problems". No dependencies, no build tools.

## Development

```bash
python3 -m http.server 8000   # required — fonts won't load via file://
# Open http://localhost:8000 in Chrome at 1920x1080
```

## Architecture

Everything lives in `index.html`:
- `<style>` — font-face declarations, layout, slide type styles, animations
- `<div class="deck">` — slides as `<section class="slide slide--TYPE" data-animate="ANIM">` elements
- `<script>` — navigation (keyboard), animation triggers, video fullscreen control, viewport scaling

The deck is fixed at 1920x1080px and scaled to fit the viewport via `transform: scale()`. All dimensions inside slides use `px`, never viewport units.

## Slide Authoring

Add slides inside `<div class="deck">`:
```html
<section class="slide slide--content" data-animate="fade">
  <h2>Title</h2>
  <p>Content</p>
</section>
```

Types: `slide--title`, `slide--section`, `slide--content`, `slide--two-col`, `slide--video`, `slide--cta`, `slide--image`

Animations: `data-animate="fade|slide-up|slide-left|zoom"` (omit for instant)

## Gotchas

- **Fonts need HTTP**: Suisse Intl `.woff` files in `fonts/` won't load via `file://` protocol. Always use the dev server.
- **Fullscreen API requires user gesture**: Video fullscreen (`requestFullscreen()`) only works from click or keypress handlers.
- **Arrow keys suppressed during video playback**: Prevents accidental slide navigation while a video is playing.
- **px only inside slides**: The deck uses `transform: scale()` — viewport units (`vw`/`vh`) will break sizing.

## Brand

Font: Suisse Intl (Light/Regular/Medium/SemiBold/Bold in `fonts/`)
Colors: dark bg `#02021e`, blue `#3719ca`, yellow accent `#fdc448`, purple `#7057d6`, white text
Logo: `logo.svg` (full wordmark, white), `icon-halo.svg` (icon only)

## Webinar Content Reference

Topic notes and demo data: `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/Reflect/Reflect/public-speaking/webinar-claude/`
