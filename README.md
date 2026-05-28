# holdco-review

**Quarterly and monthly portfolio review presentations for holding companies and SMBs.**

Built by [Truss One Partners](https://truss1.com) — a small holding company that acquires and operates essential service businesses. We built this tool for our own offsites and open-sourced it so other operators can use it too.

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Farshankar1%2Fholdco-review)

---

## What It Does

You provide your financial data (CSV, XLSX, or whatever you have). A Claude Code skill generates a complete, interactive HTML presentation you can deploy to Vercel or open locally. One command, one file, done.

**The output includes:**
- Portfolio and per-company financial summaries with interactive SVG charts
- P&L deep dives with quarterly trends and YoY comparisons
- Interactive valuation explorer with adjustable multiples
- Debt schedules and DSCR analysis
- An honest, Buffett-lens strategic assessment that identifies your portfolio's central tension and ranks where your next dollar goes furthest
- Partner economics, distributions, and growth strategy slides

**Works with 1 company or 10.** Single-operator SMBs and multi-company holdcos alike.

## What Makes It Different

Most presentation tools give you templates. This gives you analysis.

The strategic assessment doesn't just display your numbers — it reads them, identifies what's working and what isn't, and writes an honest outside-in view of your portfolio. It tells you where $1 of effort goes furthest, what your DSCR trajectory means, and what would actually start the flywheel. The voice is Buffett shareholder letter, not consulting deck.

## Quick Start

### 1. Clone the repo

```bash
git clone https://github.com/arshankar1/holdco-review.git
cd holdco-review
```

### 2. Run the skill

Open the project in [Claude Code](https://claude.ai/code) and run:

```
/holdco-review
```

The skill will walk you through setup:
- Your holding company name, branding, and accent color
- How many companies you operate (1+)
- Which optional slides you want (distributions, partner economics, growth strategy)
- What data you have available

### 3. Add your data

The skill generates CSV templates tailored to your companies. Fill them in with your financials.

**CSV schemas are starting points, not requirements.** If you already have a QuickBooks export, an Excel P&L, or bank statements in a different format, just drop those files in the `data/` folder. The skill will work from whatever you have.

### 4. Generate your presentation

```
/holdco-review generate
```

This reads your data, runs the strategic assessment, and outputs a single `index.html` in `output/`.

### 5. Deploy

```bash
vercel --prod
```

Or just open `output/index.html` in your browser.

## Example

The repo ships with a complete example: **Bridgepoint Capital Partners**, a fictional 3-company holdco with realistic SMB financials. See `example/` for the data and `example/output/index.html` for the generated presentation.

## Slide Types

### Always Included
| Slide | Description |
|-------|-------------|
| Welcome | Title slide with holdco name and review period |
| Portfolio Overview | Aggregate revenue, SDE, equity, DSCR across all companies |
| Company Summary | One per company — key metrics, margins, debt position |
| P&L Deep Dive | Quarterly charts, annual bars, YoY comparisons (per company) |
| Valuation Explorer | Interactive sliders for SDE multiple, shows enterprise value and net equity |
| Debt Schedule / DSCR | Debt terms, amortization timeline, DSCR trajectory |
| Outside-In Assessment | Buffett-lens strategic analysis — central question, evidence, flywheel |
| Where to Focus | Priority-ranked levers showing where $1 goes furthest |
| Next Steps | Discussion questions for the offsite |

### Optional (Selected During Setup)
| Slide | Description |
|-------|-------------|
| Distributions | Salary, profit, and reinvested distributions by period |
| Partner Economics | Equity splits, ownership percentages, vesting schedules |
| Growth Strategy | 2-3 year growth paths (organic, add-ons, new platforms) |
| Timeline / Journey | Key milestones since founding |
| Preamble | Context-setting narrative for the review |

## Design System

The presentation uses an IB-style design system:

- **Typography**: Instrument Serif (headings), Instrument Sans (body)
- **Layout**: Sidebar navigation with keyboard shortcuts, full-bleed slides
- **Charts**: Pure SVG with smooth cubic bezier curves
- **Voice**: Subtitle is the takeaway. Honest prose. Specific numbers. No AI patterns.

Colors, accent, and per-company colors are all configurable.

## Data Format

All data lives in CSV files. See `templates/` for the schemas.

| File | What It Contains |
|------|-----------------|
| `companies.csv` | Company names, industries, locations, acquisition dates, colors |
| `financials/{company}.csv` | Quarterly P&L (revenue, COGS, gross profit, opex, SDE) |
| `debt-schedule.csv` | Debt terms, rates, payments, maturity dates per company |
| `portfolio.csv` | Portfolio-level aggregates |
| `partners.csv` | Partner names, equity, ownership, vesting |
| `distributions.csv` | Distribution history by period and type |

**These schemas are directional.** The skill can interpret data in other formats too.

## Requirements

- [Claude Code](https://claude.ai/code) (for the `/holdco-review` skill)
- A browser (to view the output)
- Optionally, [Vercel CLI](https://vercel.com/cli) for deployment

## About Truss One Partners

[Truss One Partners](https://truss1.com) is a small holding company that acquires and operates essential service businesses. We believe small business acquisition should be transparent, data-driven, and honest about what's working and what isn't. This tool reflects how we think about our own portfolio.

## License

MIT
