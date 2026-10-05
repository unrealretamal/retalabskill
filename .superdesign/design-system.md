# RetaLab Skill — Design System

## Product Context

**RetaLab Skill** is a style-neutral UI/UX consultation tool for coding agents. It helps developers define design systems, review interfaces, and get prioritized fix lists — all through conversation with AI. The landing page must convey precision, design credibility, and tool-like sophistication.

**Target audience:** Developers, product designers, and teams using AI coding agents (Codex, Claude Code, Cursor, Windsurf).

**Key pages:**
- Landing page (hero + feature showcase + examples)
- Documentation (README-style reference)

**JTBD:** "When I'm building a project, I want to quickly define its design system and review my UI, so I can ship consistent, polished interfaces without being a designer."

## Branding & Styling

### Primary Style: Architectural Type System

A minimalist, monochrome aesthetic with rigid grid structures, hairline borders, and extreme typographic hierarchy. Think Swiss brutalist meets IDE precision.

### Color Palette

- **Background:** #000000 (pure black)
- **Foreground/Text:** #FFFFFF (pure white)
- **Accent:** #6366f1 (Indigo)
- **Secondary text:** rgba(255, 255, 255, 0.40) to rgba(255, 255, 255, 0.60)
- **Hairline borders:** rgba(255, 255, 255, 0.15) at 0.5px
- **Card hover background:** rgba(255, 255, 255, 0.03)

### Typography

- **Headlines:** 'Inter Tight', weight 900, tracking -0.06em, uppercase for hero
- **Section titles:** 'Inter Tight', weight 700-800, 24-32px
- **Metadata/Labels:** 'JetBrains Mono', weight 500, tracking 0.2em-0.4em, 8px-11px, underscores instead of spaces
- **Body text:** 'Inter', weights 300-400, 14-16px
- **Buttons/CTAs:** 'JetBrains Mono' or 'Inter Tight', uppercase, tracking 0.2em

### Spacing & Layout

- Grid-based: 12-column with 0.5px hairline dividers
- Section heights: 100vh for hero, generous whitespace throughout
- Content pinned to grid edges for architectural feel
- Zero border-radius on containers (except pill buttons)
- No shadows or gradients — depth achieved through line-work and contrast

### Motion & Interaction

- Hover transitions: 300ms ease
- Color swaps: black/white inversion or shift to indigo accent
- Icon rotations: 12-45 degrees on hover (700ms cubic-bezier)
- Subtle opacity shifts on cards: 3% to 5% background fill on hover

### Specific Requirements

- Global noise texture overlay at 5% opacity for tactile feel
- Hairline borders (0.5px) as primary visual separator
- Monospace labels with underscore format: e.g., DESIGN_MODE, P0_FIX, STYLE_FAMILY
- Only ONE accent color (indigo) for primary CTAs and key data points
- No emoji, use geometric shapes or Lucide-style line icons
- Components demonstrated inline: form examples, dashboard mockups, review cards

## Anti-patterns (DO NOT USE)

- Rounded corners on grid cells or input fields
- Shadows, gradients, or glow effects
- Multiple accent colors
- Decorative illustrations or photos
- Serif fonts in UI elements
- Spacing between grid cells (use hairline borders instead of gaps)