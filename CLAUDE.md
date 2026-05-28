# holdco-portfolio-review

Quarterly and monthly portfolio review presentations for small holding companies (HoldCos) and SMBs. Built by Truss One Partners.

## What This Is

A Claude Code skill that generates self-contained, deployable HTML presentations from CSV/XLSX financial data. The output is a single `index.html` with an IB-style design system, interactive SVG charts, and a Buffett/Munger-lens strategic assessment.

## Key Files

- `.claude/skills/holdco-review.md` — The main skill. Two modes: `setup` (onboarding) and `generate` (build HTML)
- `design-system/DESIGN.md` — Typography, colors, layout, chart rendering patterns
- `design-system/slide-catalog.md` — HTML snippets for each slide type
- `prompts/outside-in-assessment.md` — The strategic assessment prompt (central question, evidence, recommendations)
- `example/` — Bridgepoint Capital Partners dummy data + pre-built output
- `templates/` — Empty CSV templates for users

## Design Principles

- **IB deck style**: Subtitle IS the takeaway. No separate takeaway boxes.
- **Buffett shareholder letter voice**: Honest, forward-looking, specific numbers, no AI patterns
- **Typography**: Instrument Serif for h1/h2. Instrument Sans for everything else.
- **Colors**: White bg, warm gray (#f8f7f5) cards, configurable accent. Per-company colors.
- **Charts**: SVG only. Line charts for trends, bars for annual. Cubic bezier curves.
- **Single file output**: The generated `index.html` is completely self-contained. No external dependencies except Google Fonts.

## Rules

- Never use em dashes in prose
- Never inflate significance ("transformative", "revolutionary", etc.)
- Frame struggling businesses operationally, not as failures ("the business works, the balance sheet is heavy")
- Always use specific numbers ("$15K/yr short of DSCR breakeven" not "slightly below target")
- Forward-looking over backward-looking
- CSV schemas are directional. Accept whatever files the user has.
- Must work with 1 company or 10.
