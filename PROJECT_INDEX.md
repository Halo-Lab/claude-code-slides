# Project Index

**Last updated**: 2026-03-31

## Overview

Single-file HTML/CSS/JS slide presentation engine for a Halo Lab webinar on using Claude Code for healthtech founders.

## Tech Stack

- **Language**: Vanilla HTML, CSS, JS (ES6+)
- **Framework**: None
- **Database**: None
- **Testing**: Manual (open in Chrome at 1920x1080)
- **Server**: `python3 -m http.server 8000` for local dev

## Directory Structure

```
web-slides/
├── index.html              # Entire engine + all slides (CSS, HTML, JS)
├── logo.svg                # Halo Lab full logo (white on dark)
├── icon-halo.svg           # Halo Lab icon only
├── fonts/
│   ├── SuisseIntl-Light.woff
│   ├── SuisseIntl-Regular.woff
│   ├── SuisseIntl-Medium.woff
│   ├── SuisseIntl-SemiBold.woff
│   └── SuisseIntl-Bold.woff
├── videos/                 # Pre-recorded demo .mp4 files (TBD)
├── .claude/
│   └── skills/
│       └── slide-authoring/SKILL.md
├── AGENTS.md               # Development guidelines
├── SPEC.md                 # Technical specification
├── PLAN.md                 # Implementation phases
└── PROJECT_INDEX.md        # This file
```

## Entry Points

- `index.html` — the only file that matters. Open in browser to present.

## Key Modules (all in index.html)

| Section | Purpose |
|---------|---------|
| `<style>` | Font-face, layout, slide types, animations, utilities |
| `<div class="deck">` | All slides as `<section>` elements |
| `<script>` | Navigation, animation engine, video fullscreen, scaling |

## Commands

- `python3 -m http.server 8000` — serve locally (needed for font loading)
- Open `http://localhost:8000` in Chrome at 1920x1080

## External Content

- Webinar notes: `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/Reflect/Reflect/public-speaking/webinar-claude/`
