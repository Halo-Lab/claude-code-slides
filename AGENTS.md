# AGENTS.md — Webinar Slide Engine

## Project

Single-file HTML/CSS/JS slide engine for a Halo Lab webinar. No build tools, no npm, no frameworks. Open `index.html` in Chrome and present.

## Tech Stack

- **Language**: Vanilla HTML, CSS, JS (ES6+)
- **Font**: Suisse Intl (local `.woff` files in `fonts/`)
- **Target**: Chrome, 1920x1080, screen-shared via Teams
- **Server**: `python3 -m http.server 8000` for local dev (font loading needs HTTP)

## File Structure

```
index.html          ← entire engine + all slide content
logo.svg            ← Halo Lab logo (white, for dark bg)
icon-halo.svg       ← Halo Lab icon
fonts/              ← Suisse Intl .woff (Light/Regular/Medium/SemiBold/Bold)
videos/             ← pre-recorded demo .mp4 files
SPEC.md             ← technical specification
PLAN.md             ← implementation phases
```

## How to Work on This Project

### Everything lives in `index.html`

- `<style>` block: all CSS (font-face, layout, slide types, animations)
- `<body>`: all slides as `<section class="slide">` elements
- `<script>` block: navigation, animation, video control

### Adding a new slide

Add a `<section>` inside `<div class="deck">`:

```html
<section class="slide slide--content" data-animate="fade">
  <h2>Slide Title</h2>
  <p>Content here</p>
</section>
```

### Slide types (CSS classes)

| Class | Use for |
|-------|---------|
| `slide--title` | Opening/closing slides with logo |
| `slide--section` | Section dividers (large text, accent bg) |
| `slide--content` | Standard text content |
| `slide--two-col` | Two-column layout |
| `slide--video` | Video embed with fullscreen |
| `slide--cta` | Call to action |
| `slide--image` | Full-bleed image |

### Animation options (`data-animate`)

`fade`, `slide-up`, `slide-left`, `zoom`, or omit for instant.

### Video slides

```html
<section class="slide slide--video" data-animate="fade">
  <video src="videos/demo-name.mp4" data-fullscreen></video>
</section>
```

Click video or press `F` to fullscreen. Auto-pauses on nav away.

## Brand Tokens

```css
--bg-dark:    #02021e;
--blue:       #3719ca;
--yellow:     #fdc448;
--purple:     #7057d6;
--white:      #ffffff;
--white-70:   rgba(255,255,255,0.7);
--grey:       #9a9aa5;
--grey-light: #f5f5f7;
--font:       'Suisse Intl', Arial, sans-serif;
```

- Dark background (`--bg-dark`) is default
- White text on dark
- Yellow for accents/highlights
- Blue/purple for section dividers or emphasis backgrounds

## Key Conventions

- No external dependencies. Everything is inline or relative-path.
- Keep CSS organized: font-face → reset → layout → slide types → animations → utilities
- Keep JS minimal: navigation state, animation triggers, video control. No abstractions.
- Test at 1920x1080 in Chrome. Use `python3 -m http.server 8000` to serve.

## CRITICAL Patterns

- **Font loading**: Suisse Intl won't load via `file://` protocol. Always serve via HTTP for dev/testing.
- **Video fullscreen**: Must use Fullscreen API (`element.requestFullscreen()`). Only works on user gesture (click/keypress).
- **Arrow key conflicts**: Suppress arrow key navigation while video is playing to prevent accidental slide change.
- **Scaling**: The deck is fixed 1920x1080 and scaled via `transform: scale()`. Never use viewport units inside slides — use px.

## Confidence Check (Required Before Implementation)

Before starting any non-trivial task, assess confidence:

1. **Duplicate check**: Search codebase for similar functionality
2. **Architecture check**: Verify approach fits existing patterns
3. **Docs check**: Read official documentation, don't assume
4. **Examples check**: Find working code samples
5. **Root cause check**: Understand the actual problem

Proceed only at ≥90% confidence. Below 70%, stop and ask questions.

## Error Recovery Protocol

1. **STOP** — Don't re-run the same command
2. **Investigate** — Read error message, check logs, search docs
3. **Hypothesize** — "Error caused by X because evidence Y"
4. **Design different approach** — Not the same approach again
5. **Document** — Record learning for future prevention

## Wrap-Up Checklist (REQUIRED per phase)

1. **Verify**: Open in Chrome at 1920x1080, no console errors
2. **Test**: Navigation works, animations play, videos fullscreen
3. **User confirmation**: Walk through test steps, wait for explicit sign-off
4. **Commit**: Clean diff, descriptive message, no debug code
