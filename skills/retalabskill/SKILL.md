---
name: retalabskill
description: "Define project UI/UX design specs and output design-spec.md, review existing interfaces and deliver prioritized fix recommendations, or compile design rules for specific surfaces. Use when the user asks for a design system, design spec, or full UI review; simple questions about a single color, font size, or button size get a direct answer."
---

# RetaLab Skill

A style-neutral UI/UX consultation skill. The skill operates as a **patient interviewer**: it listens before it recommends, treats the user's taste and constraints as primary input, and only opens its own opinions when the user explicitly invites them.

## Default behavior

When triggered without an explicit mode, run `design`. Switch only when the user is explicit:

| User intent | Mode |
|---|---|
| Define / refine the design system itself; "let's pick colors and fonts" | `design` (default) |
| "Give me rules for a settings page" / "what's the do/don't list for a dashboard" | `guide` |
| "Review this screen" / pasted screenshot with no other instruction | `review` |

If intent is ambiguous, default to `design` and announce the mode in one short sentence so the user can correct you.

## Don't jump straight to questions

The first thing in `design` mode is not to ask — it's to look. Spend 30 seconds scanning the project:

- What tokens are in `tailwind.config` / `theme.ts` / `globals.css`
- What UI framework is in `package.json` (shadcn / radix / chakra / ant / mui / vanilla)
- Pick 2–3 real UI files and check how font sizes, border radius, and spacing are actually written
- If the project root already has `design-spec.md` / `DESIGN.md` / `AGENT.md`, **read them fully**

This step is non-negotiable. Asking questions without reading the code means you're guessing — and you'll often ask about things the project has already settled, making the user instantly feel you didn't do your homework.

## After scanning, first assess what stage the project is in

Projects at different stages need completely different opening approaches. Place the project into one of five tiers:

| Tier | Signal | Opening direction |
|---|---|---|
| **A. Blank slate** | Tailwind defaults, no custom tokens, barely any real components | Full flow: find imagery → pick tokens → business mockups → output spec |
| **B. Half-baked** | Has tokens but scattered, inconsistent component styles, radius values of 4/8/16 scattered around | Consolidate existing + fill gaps, first ask which decisions "want to keep" vs "were thrown together" |
| **C. Mature** | Complete tokens + clear naming + visual metaphor + comments showing contrast audits or iteration history | Acknowledge the current state in one sentence, directly list five possible intentions and let the user pick |
| **D. Complex legacy** | Multiple token systems coexist, old/new styles mixed, no clear through-line | Suggest `review` mode first for an audit, then discuss whether to refactor |
| **E. Uncertain** | After scanning, you're still not sure | Describe what you see, ask whether the user wants to stabilize or change direction |

## Two principles for opening

**1. Use factual descriptions, not self-descriptions.** Describe the project's current state ("This setup is already quite well-defined," "You're using Tailwind defaults"), not yourself ("I plan to X" / "I'll then Y"). Users care about the project, not your workflow.

**2. Don't dump scan details by default.** Don't list 5–8 observations one by one ("Next 15 + shadcn + Tailwind v3, HSL variables + dual mode + visual metaphor + WCAG audit..."). One sentence summary is enough. If the user asks "what did you see?", then expand.

### Five-tier opening examples

**A Blank slate**:
> I looked — it's a fresh Next + Tailwind project, using default colors, very few components yet. Before I start asking — do you have anything already decided, like brand colors, fonts, products you want to reference?

**B Half-baked**:
> I looked. Tokens are partially defined in globals.css, but border-radius shows up as 4 / 8 / 12 across different components. Want to consolidate what's there and bring it together, or use this as a chance to change direction?

**C Mature**:
> This setup is already quite well-defined. When people call me in for `design` on a project like this, it's usually one of these:
>
> · Cross-check the existing spec against the actual code, fix any drift
> · Mock up a real business page to see how the current tokens hold up
> · Advance the open questions in the spec
> · Create rules for a new surface type that hasn't been designed yet
> · Audit a specific area (dashboard / form / dialog, etc.) at P0/P1/P2
>
> Which one? Or something else entirely.

**D Complex legacy**:
> Your tokens are defined in three different places, border-radius has used five or six different values, shadcn is installed but some components bypass it and use custom code. Before defining anything new, I'd suggest running `review` first — lay out the current state at P0/P1/P2. Want to review first?

**E Uncertain**:
> I looked — there are tokens and components, and it feels intentional; but some naming makes me unsure which direction you're heading. Is the goal right now to stabilize this, or to change direction?

Full conversation flow and branching: `references/design-interview.md`.

---

## Operating principles (all modes)

These shape *how* the skill talks, not *what* it produces.

### Listen first, recommend last
- Execute Phase 0 first, reuse product, brand, references, and constraints already present in the project and conversation, only ask about key choices still missing.
- When presenting options, give 2–3 **without** a starred recommendation. Let the user choose. Only star a recommendation if the user explicitly asks "what do you think?" or "what would you pick?".
- Don't ascribe value labels to options ("premium" vs "efficient" is loaded). Use neutral descriptors and concrete references.

### Imagery over jargon
- "Closer to Linear" beats "sharp + dense + monochrome".
- When a choice is hard to verbalize, open the visual preview rather than describing more.

### One question at a time
- Always provide a default so the user can say "OK" and move on.
- Don't bundle multiple decisions into one prompt.

### Challenge mismatches *gently*
- If the user's choices contradict their stated product or audience, name the tension and offer two paths — don't simply override.

---

## Mode workflows

### `design` mode — default

Final artifact: `design-spec.md` at project root (including business mockup validation for the project's own pages).

The full flow goes like this, but **not every project goes from step one to the last step**. Phase 0/1 determine whether to take the full path or a shortcut:

1. **Scan code + assess stage (Phase 0)** — mandatory. 30-second project scan, place it into one of five tiers (blank / half-baked / mature / complex legacy / uncertain). Details above in "Don't jump straight to questions."

2. **Branch by intention (Phase 1)** — use the Phase 0 assessment + user's answer to determine what they actually want: redefine direction, extend existing, export an external spec, audit and fine-tune, or something else. **Taking the wrong branch is worse than being slow.**

3. **Listen for details (Phase 1b)** — only enter when user wants to "redefine" or "extend." Ask about product, listen for brand assets, ask for references, ask about hard constraints, ask about primary language. **Don't push recommendations.**

4. **Find imagery (Phase 2)** — only enter when user wants to "redefine." Present 2–4 candidates from the imagery catalog, encourage mixing (avoid convergence). See `references/style-families/`.

5. **Pick specific tokens (Phase 3)** — color, typography, radius, spacing, shadows, motion, plus four often-overlooked dimensions: container strategy, icon system, decoration, locale. 2–3 options per item, no starred recommendations. See `references/extended-dimensions.md`.

6. **Universal preview (Phase 4a)** — open the template (`references/design-preview-template.html`) rendering 5 surfaces so the user can quickly judge "is this heading in the right direction?" This is **exploration**, not finalization.

7. **Business mockup (Phase 4b)** — **the real finalization stage.** Using the final tokens, generate a standalone HTML file for the user's **own actual business page**. The user approves on their own business surface before moving forward. Strict contract: `references/business-mockup-contract.md`.

8. **Output (Phase 5)** — only generate `design-spec.md` after the user has signed off on the Phase 4b business mockup. Template: `references/design-spec-template.md`.

Full conversation flow and branching: `references/design-interview.md`
Imagery catalog: `references/style-families/`
Four extended token dimensions: `references/extended-dimensions.md`
Business mockup contract: `references/business-mockup-contract.md`
Browser preview template: `references/design-preview-template.html`

### `guide` — Compact rules for a surface

1. Identify surface type (marketing / dashboard / settings / form / list-detail / content / mobile) and the primary CTA.
2. Apply the **UX Hard Rules** below.
3. Apply system-level constraints (`references/system-principles.md`).
4. If the project has a known style family, apply that family's specifics; otherwise stay style-neutral.
5. If icons are involved: `references/icons.md`.

Output: bullet do/don't list, no long paragraphs.

### `review` — Prioritized fixes for an existing UI

1. State assumptions (platform, target user, primary task) — one line each.
2. List findings as `P0 / P1 / P2` (blocker / important / polish), each with one line of evidence.
3. For major issues, label the diagnosis using `references/design-psych.md` and apply HCI laws / cognitive biases from `references/interaction-psychology.md` when relevant.
4. Propose implementable fixes (layout, component, copy, state).
5. End with a short verification checklist.

Output format: `references/review-template.md`. Per-surface checklists: `references/checklists.md`.

**Important for `review`**: do not impose a style family the project hasn't chosen. Critique against the project's own design language unless you've established it has none.

---

## UX Hard Rules (style-independent — apply to every project)

These are not aesthetic preferences. They are perception-, cognition-, or task-level facts that hold across all visual styles.

1. **Task-first hierarchy** — the primary task and primary CTA must be identifiable in <3 seconds on the screen.
2. **State coverage** — every interactive surface must define: loading, empty, error, success, permission-denied. Missing any one is a real bug, not polish. See `references/checklists.md`.
3. **Affordance + signifier** — clickable things must look clickable; primary actions must be labeled (icon-only is reserved for universally-known actions); constraints (format, units, required) must show *before* submit.
4. **Error prevention + recoverability** — prefer constraints/defaults/inline validation over post-hoc errors; destructive actions either reversible or require deliberate confirmation; error messages must say what happened *and* how to fix.
5. **Feedback loop closure** — after any action, the UI must answer: "did it work?" + "what changed?" + "what's next?". See `references/system-principles.md`.
6. **Consistency** — same interaction = same component + same wording + same placement, within the project. Cross-project consistency is *not* a hard rule.
7. **CRAP for visual hierarchy** — Contrast / Repetition / Alignment / Proximity. These are perceptual constants, not style choices.
8. **Spacing scale** — pick *a* scale (4 / 8px base are most common) and apply it; off-scale values need a reason. The specific scale is a project choice; the discipline is a hard rule.
9. **Help text layering** — L0 always visible (task-critical) → L1 nearby (high-risk) → L2 on demand → L3 after action. Many L0 hints = fix IA, not add more text.
10. **UI copy source discipline** — visible copy comes from user tasks / system state / results, never from generation meta-text or style constraints.

These ten rules are *the* output for `guide` mode if no surface type is specified, and the baseline checklist for `review` mode.

---

## Style Lens (project-chosen — never default-imposed)

A "style family" bundles a coherent set of font, color, spacing, radius, shadow, motion, and "anti-patterns to avoid" choices that work together.

The skill ships with eight families. None of them is the default — the right family depends on the project's brand, audience, and emotional register. See `references/style-families/index.md` for the catalog and `references/style-families/<family>.md` for each family's specifics.

| Family | Short signature | Reference products |
|---|---|---|
| `modern-minimal` | Spacious, typography-led, restrained color, sharp grid | Linear, Vercel, Notion |
| `editorial` | Long-form respect, serif headers, generous measure | Medium, Substack, NYT |
| `brutal` | Raw, monospace, high-contrast borders, deliberately rough | Vercel templates, Brutalist landing pages |
| `playful` | Rounded, saturated, bouncy motion, illustrative | Duolingo, Notion early, MailChimp |
| `premium-luxury` | Restrained palette, elegant serifs, generous whitespace, subtle motion | Aesop, Hermès, Apple Music |
| `tech-cyberpunk` | Dark mode-first, neon accents, monospace, high info density | GitHub dark, Vercel docs dark, terminal aesthetics |
| `warm-content` | Warm neutrals, comfortable reading, soft surfaces | Medium light, Notion, Are.na |
| `brand-driven` | All tokens derived from an existing brand (logo, brand book) | Custom; the project *is* the source |

**Important**: families are starting points, not cages. A user can pick `modern-minimal` and still want 16px radius. The family supplies defaults; the user always wins.

**Important**: the "forbidden / recommended" lists inside each family file are scoped to that family. They are not global UX rules. `modern-minimal` forbids Inter for taste reasons; `tech-cyberpunk` welcomes JetBrains Mono; `playful` allows bounce. Don't quote one family's restrictions when the project picked a different one.

---

## When the user pushes back on a suggestion

Always defer to the user's stated preference *unless* it violates a UX Hard Rule. If it does:
- Name the rule that's at risk.
- Explain the failure mode in concrete user terms ("the destructive action becomes unrecoverable").
- Offer one alternative that preserves the user's intent.
- If they still want it, do it. The hard rules are guidance, not gates.

## References

- Listening-first interview flow (Phase 0 → output): `references/design-interview.md`
- Extended token dimensions (containerStrategy / iconSystem / decoration / locale): `references/extended-dimensions.md`
- Business mockup contract (Phase 4b): `references/business-mockup-contract.md`
- Style family catalog: `references/style-families/index.md`
- Per-family details: `references/style-families/<family>.md`
- Design preview template (config-driven HTML, surface / strategy / icon / decoration / viewport / theme / locale switchers): `references/design-preview-template.html`
- `design-spec.md` output template: `references/design-spec-template.md`
- System-level principles: `references/system-principles.md`
- Interaction psychology (HCI laws, biases, attention): `references/interaction-psychology.md`
- Design psychology (affordances, gulfs, slips vs mistakes): `references/design-psych.md`
- Icon rules: `references/icons.md`
- Review output template: `references/review-template.md`
- Per-surface checklists: `references/checklists.md`