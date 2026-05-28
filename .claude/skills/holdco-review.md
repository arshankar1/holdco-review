---
name: holdco-review
description: Generate quarterly/monthly portfolio review presentations for holding companies and SMBs. Setup mode creates data templates; generate mode builds the HTML presentation.
user-invocable: true
---

# holdco-review

You are generating a quarterly or monthly portfolio review presentation for a holding company or SMB. This skill has two modes:

## Detecting Mode

- If no `data/config.json` exists in the project directory, run **Setup Mode**
- If `data/config.json` exists, run **Generate Mode**
- If the user says "setup", "init", or "start over", run Setup Mode regardless
- If the user says "generate", "build", or "create", run Generate Mode regardless

---

## Setup Mode

Walk the user through configuring their portfolio review. Be conversational, not robotic. You're helping an operator who probably has a spreadsheet open in another window.

### Step 1: Holding Company Basics

Ask:
- What's your holding company called?
- What's the review period? (e.g., "Q2 2026", "Annual 2025", "Monthly May 2026")
- Do you have a tagline or one-liner? (optional)
- What accent color do you want? (default: #2563eb blue, offer a few options)
- Do you have a logo file? (optional, text header works great)

### Step 2: Companies

Ask:
- How many companies do you operate? (works with 1 or more)
- For each company: name, industry, location, when acquired, how much equity invested

Assign colors automatically from a curated palette:
```
Company 1: #1d4ed8 (blue)
Company 2: #047857 (green)
Company 3: #b45309 (amber)
Company 4: #7c3aed (purple)
Company 5: #0891b2 (cyan)
Company 6: #be123c (rose)
```

Let the user override if they want.

### Step 3: Slide Selection

Explain that these slides are **always included**:
- Portfolio Overview (skipped in single-company mode)
- Company Summary (one per company)
- P&L Deep Dive (one per company — highly recommended, needs quarterly data)
- Valuation Explorer (highly recommended)
- Debt Schedule & DSCR Analysis
- Outside-In Strategic Assessment (the honest outside perspective)
- Where to Focus (priority-ranked levers)
- Next Steps / Discussion

Then ask which **optional slides** they want:
- Distributions summary (salary, profit, reinvested)
- Partner economics (equity splits, ownership, vesting)
- Growth strategy (2-3 year growth paths)
- Timeline / journey (key milestones)
- Preamble (context-setting narrative)

### Step 4: Generate Templates

Create a `data/` directory with:
- `config.json` — their choices from steps 1-3
- CSV templates pre-populated with their company names and appropriate headers

Then tell the user:

> Your data templates are in `data/`. Fill them in with your financials.
>
> **These CSV schemas are starting points, not requirements.** If you already have financials in a different format — a QuickBooks export, an Excel P&L, bank statements, or even just a messy spreadsheet — drop those files in the `data/` folder. I'll work from whatever you have.
>
> When you're ready, run `/holdco-review` again and I'll generate your presentation.

---

## Generate Mode

Read everything in `data/` and generate a complete, self-contained `output/index.html`.

### Step 1: Read and Interpret Data

Read ALL files in `data/`:
- `config.json` for structure and branding
- All CSV files matching the expected schemas
- Any OTHER files the user dropped in (Excel exports, PDFs, text files, etc.)

For non-standard files, interpret them best you can. Extract the financial data you need. If something is ambiguous, ask the user rather than guessing.

Compute:
- TTM revenue, SDE, SDE margin per company
- Quarterly margin trends
- DSCR per company (TTM SDE / annual debt service)
- FCFADS per company (SDE - debt service)
- Net equity per company at midpoint multiple
- Portfolio MOIC
- Revenue and margin trajectories

### Step 2: Read the Design System

Read these files for reference:
- `design-system/DESIGN.md` — CSS variables, typography, layout, chart patterns
- `design-system/slide-catalog.md` — HTML structure for each slide type
- `prompts/outside-in-assessment.md` — The strategic assessment prompt

### Step 3: Generate the HTML

Generate a single `output/index.html` that is:
- **Completely self-contained** — no external dependencies except Google Fonts CDN
- **All CSS inline** in a `<style>` block
- **All JS inline** in a `<script>` block at the bottom
- **All chart data** embedded as JS arrays/objects
- **Ready to deploy** — open in a browser or push to Vercel

The HTML must include:
1. Full CSS from the design system (with user's accent color and company colors)
2. Sidebar navigation with all slides listed
3. Keyboard navigation (arrow keys, Page Up/Down, Home/End)
4. Slide counter
5. All selected slides with real data
6. SVG chart rendering JS for P&L deep dives
7. Interactive valuation explorer JS (if included)
8. XIRR calculation (if partner economics included)

### Step 4: The Analytical Slides

For the Outside-In Assessment and Where to Focus slides, follow the prompt in `prompts/outside-in-assessment.md` exactly. These slides are generated from analysis, not templates. Read all the data, compute the metrics, and write honest, specific, forward-looking analysis.

Key reminders:
- The subtitle on every slide IS the takeaway
- No em dashes in prose
- No AI writing patterns
- Specific numbers, not vague qualifiers
- Frame operationally, not as failure
- Forward-looking: what does this mean for the next 2-3 years?

### Step 5: Output

Write the file to `output/index.html`. Create the `output/` directory if it doesn't exist.

Tell the user:
> Your presentation is ready at `output/index.html`. Open it in a browser to preview, or deploy with `vercel --prod`.

### Single-Company Mode

When config shows only 1 company:
- Skip Portfolio Overview slide
- Outside-In Assessment focuses on the single business
- Where to Focus shows internal operational levers, not company-vs-company ranking
- Valuation explorer shows just the one company
- Simpler sidebar with fewer items
- Adjust language: "your business" not "the portfolio"

### Chart Generation

For P&L deep dive slides, generate JavaScript that renders SVG charts client-side. The chart JS should:

1. **Annual bar charts** — revenue, gross margin %, SDE % by year
2. **Sequential timeline** — line charts with area fill showing full quarter-by-quarter history
3. **YoY quarterly comparison** — overlay line charts comparing same quarters across years

Follow the chart patterns in DESIGN.md exactly:
- Smooth cubic bezier curves (midpoint control points)
- Nice round Y-axis ticks (snap to 1/2/5/10 multiples)
- Dots at data points (r=3, white stroke)
- Grid lines at #e5e7eb
- Area fill at 0.08 opacity for sequential charts
- Rotated X-axis labels for sequential charts

### Attribution

At the very bottom of the HTML body, add:
```html
<div style="position:fixed; bottom:8px; left:50%; transform:translateX(-50%); font-size:9px; color:#ccc; font-family:sans-serif;">
  Built with <a href="https://github.com/arshankar1/holdco-review" style="color:#ccc;">holdco-review</a> by Truss One Partners
</div>
```
