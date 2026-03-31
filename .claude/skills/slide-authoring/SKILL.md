---
name: slide-authoring
description: Create and edit webinar slides in index.html. Triggers on "add slide", "new slide", "edit slide", "slide content", "update slides".
---

# Slide Authoring

## When to Use

When the user asks to add, edit, or rearrange slides in the webinar deck.

## Instructions

1. Read `index.html` to understand the current slide order and structure
2. Read `AGENTS.md` for slide type classes and animation options
3. For content reference, check the webinar notes at `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/Reflect/Reflect/public-speaking/webinar-claude/`

### Adding a Slide

Insert a new `<section>` inside `<div class="deck">` at the correct position:

```html
<section class="slide slide--TYPE" data-animate="ANIMATION">
  <!-- content -->
</section>
```

### Slide Type Reference

- `slide--title`: Logo + large heading + subtitle. Use for opening/closing.
- `slide--section`: Large text on accent background. Use for topic transitions.
- `slide--content`: Left-aligned text with optional subheadings. Main content.
- `slide--two-col`: Two equal columns via grid. Use for comparisons or text+image.
- `slide--video`: `<video>` element with `data-fullscreen`. Use for demo walkthroughs.
- `slide--cta`: Centered call-to-action with prominent button/text.
- `slide--image`: Full-bleed background image.

### Brand Guidelines

- Background: `var(--bg-dark)` (#02021e) — default dark
- Section dividers can use `var(--blue)` or `var(--purple)` backgrounds
- Accent text: `var(--yellow)` for emphasis
- Body text: white or `var(--white-70)` for secondary
- Font weights: Light (300) for large display, Regular (400) for body, SemiBold (600) for headings
- Keep text concise — this is a presentation, not a document

### Checklist

- [ ] Correct slide type class applied
- [ ] `data-animate` attribute set
- [ ] Text is concise and presentation-appropriate
- [ ] Slide counter total updated if needed (auto-calculated in JS)
