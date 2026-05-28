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

### Step 2: HoldCo Goal

Ask: "What's your main goal with the holding company?" Give examples:
- Maximize distributions / cash flow within 5 years
- Grow portfolio revenue to $X
- Build to sell the portfolio in Y years
- Create generational wealth through long-term compounding
- Pay off all debt and own businesses free and clear
- Other (let them type)

Save this to config.json as `holdco_goal`.

### Step 3: Companies

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

### Step 4: Profitability Metric

Ask: "Do you track SDE (Seller's Discretionary Earnings) or EBITDA? Different operators prefer different metrics."
- SDE (common for owner-operated SMBs under $5M revenue)
- EBITDA (common for larger businesses or institutional-style reporting)

Save to config.json as `profitability_metric`. Use this metric consistently throughout the entire presentation -- every slide, every chart label, every subtitle, every computed ratio. Never mix metrics.

### Step 5: Slide Selection

Explain that these slides are **always included**:
- Portfolio Overview (skipped in single-company mode)
- Company Summary (one per company)
- P&L Deep Dive (one per company -- highly recommended, needs quarterly data)
- Valuation Explorer (highly recommended)
- Debt Schedule & DSCR Analysis
- Outside-In Strategic Assessment (the honest outside perspective)
- Where to Focus (priority-ranked levers)
- Next Steps / Discussion

Then walk through these slides in the main flow:

- **Journey Timeline** -- ask "Would you like a timeline slide showing key milestones? (acquisitions, launches, major events)" and if yes, ask for milestone dates and descriptions. Save to config.json or have them fill `data/milestones.csv`.
- **Team Photos** -- ask "Do you have any team photos from the journey? Drop image files in `data/photos/` and I'll create a photo slide."
- **Preamble** -- ask "Is there a central message or theme you want to set for this meeting?" Then say: "I'll find a relevant excerpt from a well-known holdco leader (Buffett, Mark Leonard, Brent Beshore, the Chenmark team, or others) that reinforces your message. You can approve or change it."

Then ask which **additional optional slides** they want:
- Distributions summary (salary, profit, reinvested)
- Partner economics (equity splits, ownership, vesting)
- Growth strategy (2-3 year growth paths)

### Step 6: Custom Metrics

Ask: "What metrics matter most for each company?" Offer defaults:
- Revenue
- EBITDA or SDE (whichever they chose in Step 4)
- Margins (gross, operating, EBITDA/SDE)
- Growth % YoY
- IRR
- MOIC

Then ask: "Any other metrics specific to your businesses?" (e.g., customer count, route density, recurring revenue %, unit economics, NPS)

Save per-company metric preferences to config.json.

### Step 7: Generate Templates

Create a `data/` directory with:
- `config.json` -- their choices from steps 1-6
- CSV templates pre-populated with their company names and appropriate headers (using the chosen profitability metric in column headers)

Then tell the user:

> Your data templates are in `data/`. Fill them in with your financials.
>
> **These CSV schemas are starting points, not requirements.** If you already have financials in a different format -- a QuickBooks export, an Excel P&L, bank statements, or even just a messy spreadsheet -- drop those files in the `data/` folder. I'll work from whatever you have.
>
> **Bring everything you have.** Beyond the CSV templates, drop any files you have into `data/`:
> - Asset purchase agreements
> - Equity worksheets
> - Bank loan documents
> - IRR models or investment memos
> - Tax returns or QuickBooks exports
> - Board decks or internal reports
> - Photos of the team
>
> I'll extract what I need from whatever format you provide. The more context I have, the better the strategic assessment will be.
>
> When you're ready, run `/holdco-review` again and I'll generate your presentation.

---

## Generate Mode

Read everything in `data/` and generate a complete, self-contained `output/index.html`.

### Step 1: Read and Interpret Data

Read ALL files in `data/`:
- `config.json` for structure, branding, `holdco_goal`, `profitability_metric`, and per-company metric preferences
- All CSV files matching the expected schemas
- Any OTHER files the user dropped in (Excel exports, PDFs, text files, etc.)

For non-standard files, interpret them best you can. Extract the financial data you need. If something is ambiguous, ask the user rather than guessing.

Read `profitability_metric` from config.json. Use it everywhere -- label it consistently as either "SDE" or "EBITDA" throughout all slides, charts, subtitles, and computations. Never hardcode one or the other.

Compute (using the configured profitability metric):
- TTM revenue, profitability metric, margin per company
- Quarterly margin trends
- DSCR per company (TTM profitability metric / annual debt service)
- FCFADS per company (profitability metric - debt service)
- Net equity per company at midpoint multiple
- Portfolio MOIC
- Revenue and margin trajectories
- Any custom metrics the user configured per company

### Step 2: Read the Design System

Read these files for reference:
- `design-system/DESIGN.md` -- CSS variables, typography, layout, chart patterns
- `design-system/slide-catalog.md` -- HTML structure for each slide type
- `prompts/outside-in-assessment.md` -- The strategic assessment prompt

### Step 3: Generate the HTML

Generate a single `output/index.html` that is:
- **Completely self-contained** -- no external dependencies except Google Fonts CDN
- **All CSS inline** in a `<style>` block
- **All JS inline** in a `<script>` block at the bottom
- **All chart data** embedded as JS arrays/objects
- **Ready to deploy** -- open in a browser or push to Vercel

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

#### Preamble Slide

If the user opted for a preamble:
- Read the user's central message from config.json
- Search for a relevant excerpt from a holdco leader. Use WebSearch or WebFetch to find a fitting quote/excerpt. If those tools aren't available, pick from this curated library:

**Curated Excerpts Library** (pick the best match for the user's central message):

On patience and compounding:
1. "Someone is sitting in the shade today because someone planted a tree a long time ago." -- Warren Buffett
2. "The big money is not in the buying and the selling, but in the waiting." -- Charlie Munger
3. "We are not combating entropy by getting more complex. We're doing so by staying smaller and staying simpler." -- Mark Leonard, Constellation Software President's Letter
4. "The best businesses are the ones you never have to sell." -- Brent Beshore, Permanent Equity
5. "Our goal is to build a company that we'd be proud to run for the next 50 years." -- Andrew Wilkinson, Tiny

On operational focus:
6. "We do not view the company through the lens of traditional financial metrics alone. We look at what the operators can control." -- Brent Beshore, Permanent Equity
7. "The work of running small businesses well is quiet, undramatic, and valuable." -- Chenmark Weekly Letter
8. "Focus on the process, not the outcome. The outcomes will take care of themselves." -- Chenmark Weekly Letter
9. "Operational excellence is not a one-time event. It is a daily discipline." -- Verne Harnish, Scaling Up

On capital allocation:
10. "The heads of many companies are not combating entropy. They are actively creating chaos through serial acquisitions and constant reorganization." -- Mark Leonard, Constellation Software
11. "Rule No. 1: Never lose money. Rule No. 2: Never forget Rule No. 1." -- Warren Buffett
12. "You don't have to swing at every pitch." -- Warren Buffett

On culture and honest assessment:
13. "We think transparency with our partners is a prerequisite, not a nice-to-have." -- Brent Beshore, Permanent Equity
14. "The most important thing in communication is hearing what isn't said." -- Peter Drucker (frequently cited by holdco operators)
15. "We believe in being candid about our mistakes. That's the only way to learn from them." -- Warren Buffett, Berkshire Hathaway Annual Letter

On small business value creation:
16. "Small businesses are the backbone of the economy, and running them well is both harder and more rewarding than most people realize." -- Brent Beshore
17. "We are in the business of buying and holding forever. That changes every decision you make." -- Chenmark Weekly Letter
18. "Acquiring a small business is easy. Operating it well for decades is the real work." -- Acquiring Minds podcast insight
19. "The goal isn't to build an empire. The goal is to build something durable." -- Andrew Wilkinson, Tiny
20. "Most of the value in the world is created by people running small businesses well, far from the spotlight." -- Chenmark Weekly Letter

Include the excerpt with proper attribution on the preamble slide, designed as a pull-quote with the user's central message framing it.

#### Journey Timeline Slide

If the user opted for a timeline:
- Read milestone data from config.json or `data/milestones.csv`
- Generate a horizontal timeline with dots and event descriptions
- Style with the accent color, clean typography, dates above and descriptions below
- If photos exist in `data/photos/`, create a separate Team Photos slide with a grid layout

#### Outside-In Assessment

- Use the user's `holdco_goal` from config.json to frame the central question (e.g., if the goal is "maximize distributions within 5 years," the assessment should evaluate each company through that lens)
- Use the configured profitability metric consistently
- Generate "Recommendations" not "What Would Start the Flywheel"
- No flywheel language anywhere in this slide or any other

#### Per-Company Summary Slides

- Show the metrics the user selected in Step 6, not a fixed set
- Chart types should adapt to the metrics chosen (e.g., bar charts for revenue, line charts for growth trends, gauge-style for margins)
- Label all profitability references with the configured metric name

#### Where to Focus

- Priority-ranked levers, framed around the holdco goal
- Use the configured profitability metric in all references
- No flywheel language

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

1. **Annual bar charts** -- revenue, gross margin %, profitability metric % by year
2. **Sequential timeline** -- line charts with area fill showing full quarter-by-quarter history
3. **YoY quarterly comparison** -- overlay line charts comparing same quarters across years

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
