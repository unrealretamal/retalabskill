# Retalab Skill 


Define your project's design system, review UI and interaction issues, and output design documentation with prioritized, actionable fixes and rules.

- Doesn't assume what your product should look like
- Doesn't impose a single-style font/color/radius preference
- Listens to your product, brand, references, and constraints first, then offers options
- Options are presented as equal alternatives, without "starred recommendations" — unless you explicitly ask "what do you think?"

## Use Cases

- **New projects**: Co-create a design system (color / typography / radius / spacing / shadows / motion) through conversation, generating `design-spec.md`
- **Existing projects**: Review current UI and deliver a prioritized `P0 / P1 / P2` fix list
- **Single surfaces**: Provide "do / don't" rules for a specific page type (dashboard / form / long-form content, etc.)

## Three Modes (default: `design`)

When no mode is specified, the skill defaults to `design`. Other modes require explicit triggering.

| Mode | Purpose | Default? |
|---|---|---|
| `design` | Co-create a project-specific design spec through conversation; outputs `design-spec.md` at project root | Default |
| `guide` | Given a page type, output "do / don't" rules | Explicit trigger |
| `review` | Review an existing UI and output a `P0 / P1 / P2` fix list | Explicit trigger |

**Lightweight questions won't trigger the skill** — "which blue looks better?" gets a direct answer without launching the full conversation flow.

## Core Paradigm: UX Hard Rules vs Style Lens

The older version treated "modern minimal" style token preferences as global hard rules (banning Inter, banning pure black, banning radius >12px, enforcing OKLCH, etc.). That doesn't generalize — the same rules applied to children's products, luxury, gaming, or brand-heavy projects become constraints, not guidance.

The new version strictly separates rules into two layers:

### UX Hard Rules (10 rules, cross-style, non-negotiable)

Task priority / state coverage / affordance / error prevention / feedback closure / consistency / CRAP / spacing discipline / help text layering L0–L3 / UI copy discipline — these are perceptual and cognitive facts, not aesthetic opinions.

### Style Lens (8 style families, project-chosen)

| Family | One-liner | References |
|---|---|---|
| `modern-minimal` | Whitespace + typography + restrained color + sharp grid | Linear, Vercel, Notion |
| `editorial` | Long-form friendly, serif headings, generous measure | Medium, Substack, NYT |
| `brutal` | Raw, monospace, hard shadows, deliberately rough | Indie maker sites, Vercel brutal templates |
| `playful` | Large radius, saturated colors, bouncy motion, illustration | Duolingo, MailChimp, early Notion |
| `premium-luxury` | Elegant serifs, whitespace as value, slow motion | Aesop, Hermès, Apple Music |
| `tech-cyberpunk` | Dark-first, neon accents, monospace, dense | GitHub dark, Vercel docs, Cursor |
| `warm-content` | Warm neutrals, comfortable reading, soft surfaces | Are.na, Notion light, Craft |
| `brand-driven` | All tokens derived from existing brand assets | Project's own brand book |

Each family has its own font recommendations / color tendencies / radius ranges / motion vocabulary / anti-patterns. **These rules are family-internal and don't cross-apply** — `modern-minimal` dislikes Inter, `tech-cyberpunk` embraces Geist Mono, `playful` allows bounce. The skill won't use one family's rules to critique a project that chose another.

## `design` Mode Conversation Flow

```
Phase 0  Scan code (silent, mandatory)
   ↓
Phase 1  Listen — open-ended questions, no recommendations pushed
   ↓
Phase 2  Style family — confirm if user stated one, otherwise show 2–4 parallel families
   ↓
Phase 3  Visual choices — 2–3 options per token, no stars
   ↓
Phase 4  Full preview — render across dashboard / marketing / content / form / pricing surfaces, toggle dark mode + viewport
   ↓
Phase 5  Output design-spec.md
```

**Hard prerequisite**: Upon entering `design`, the skill silently scans the codebase first (Tailwind / theme / CSS variables / UI framework / key UI files), reusing already-established product and brand choices, only asking for missing info; forms a factual assessment of existing design token consistency (not a good/bad judgment), then opens with a summary. **It never asks questions before reading the code.**

Full flow: `skills/retalabskill/references/design-interview.md`.

## Visual Preview Template

`references/design-preview-template.html` is a **style-neutral static preview template** — its chrome is deliberately kept in grayscale tones so it doesn't compete. Each iteration only rewrites the JSON config; the user refreshes the browser.

Template supports:

- **Compare mode** — multiple candidate token sets rendered side-by-side on the **same real surface** (dashboard / marketing / content / form / pricing) for easy comparison
- **Full mode** — complete design system applied across the 5 surfaces above, with **viewport switching** (desktop / tablet / mobile) and **dark mode toggle**

## Cross-Tool Support (Codex / Claude Code / Cursor / Windsurf)

- `AGENTS.md` as the cross-tool shared instruction entry
- `CLAUDE.md`, `.cursor/rules/*.mdc` bridge to `AGENTS.md`
- `skills/retalabskill/SKILL.md` is the true definition of skill behavior

## Installation

### One-click install to multiple Agents via `skills` CLI

```bash
npx skills add unrealretamal/retalabskill --list
npx skills add unrealretamal/retalabskill -a codex -a claude-code -a cursor -a windsurf
# Global
npx skills add unrealretamal/retalabskill -g -a codex -a claude-code -a cursor -a windsurf
```

### Manual copy

```bash
git clone https://github.com/unrealretamal/retalabskill ~/.codex/skills/retalabskill
```

## How to Trigger

Two ways:

1. Explicit mention — `Please use $retalabskill to define this project's design system.`
2. Describe the task — "Help me pick colors and fonts for this project" / "Review this dashboard" / "Give me UX rules for a form page"

When only describing the task without specifying a mode, the skill defaults to `design`. Lightweight questions ("Is this button color right?") won't trigger the full flow.

## Recommended Prompt Templates

### `design` (default — customize project design spec)

```text
Please use $retalabskill to define this project's design system.
Context: [one-liner product / target user]
Requirements: Scan existing design tokens first before asking me;
I want you to hear my answers before offering options — don't jump to style recommendations.
Final output: design-spec.md at project root.
```

### `review` (review existing UI)

```text
Please use $retalabskill in review mode.
Context: Web admin dashboard, target users are new users completing initial setup.
Output: P0/P1/P2 issue list + executable fixes per issue + verification checkpoints.
Note: Don't judge us against any particular style family — we haven't settled on one yet.
```

### `guide` (rules first)

```text
Please use $retalabskill in guide mode.
Page type: B2B long form (8 fields).
Output: do/don't rules covering: CTA hierarchy, states, affordance, error prevention, help text layering, spacing.
Format: bullet points only, no long paragraphs.
```

## Repository Structure

```
.
├── AGENTS.md                                       # Cross-tool shared instructions
├── CLAUDE.md                                       # Claude Code entry point
├── .cursor/rules/retalabskill.mdc                  # Cursor bridge
├── agents/openai.yaml                              # Codex Skill metadata
├── index.html                                      # Skill landing page
└── skills/retalabskill/
    ├── SKILL.md                                    # Main rules
    ├── evals/evals.json                            # Test cases (8)
    └── references/
        ├── design-interview.md                     # design mode full flow
        ├── design-preview-template.html            # Browser preview template
        ├── design-spec-template.md                 # design-spec.md output template
        ├── system-principles.md                    # System-level principles
        ├── interaction-psychology.md               # HCI laws / cognitive biases
        ├── design-psych.md                         # Design psychology diagnostic terms
        ├── icons.md                                # Icon rules
        ├── review-template.md                      # Review output template
        ├── checklists.md                           # Per-surface checklists
        └── style-families/                         # 8 style families
            ├── index.md
            ├── modern-minimal.md
            ├── editorial.md
            ├── brutal.md
            ├── playful.md
            ├── premium-luxury.md
            ├── tech-cyberpunk.md
            ├── warm-content.md
            └── brand-driven.md
```

## Reference Docs

- skills CLI (cross-agent distribution): <https://github.com/vercel-labs/skills>
- Claude Code memory mechanism (`CLAUDE.md`): <https://docs.anthropic.com/en/docs/claude-code/memory>
- Cursor rules & `AGENTS.md`: <https://docs.cursor.com/context/rules-for-ai>
- Windsurf `AGENTS.md` support: <https://docs.windsurf.com/windsurf/cascade/memories>

## License

Apache License 2.0, see `LICENSE.txt`.

## Configuration, Dependencies & Usage Boundaries

Pure text rules and HTML preview template, no standalone accounts or API keys; requires reading the target project, preview capability is optional.

UX universal constraints and style-specific preferences are maintained separately; read the project first, then ask for missing information; never use one style's preferences to negate another.

Usage example:

```text
Review the current project's design system, reuse existing tokens first.
```
