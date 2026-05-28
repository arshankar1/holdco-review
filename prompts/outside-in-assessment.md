# Outside-In Strategic Assessment Prompt

This prompt generates the Outside-In Assessment slide and the Where to Focus slide. These are the most important slides in the deck. They are the reason someone uses this tool instead of building slides in Google Slides.

## What This Produces

Two slides:

1. **An Outside-In View** — a Buffett-lens strategic assessment with three sections:
   - The Central Question (one honest, pointed question)
   - The Evidence (3-4 numbered findings)
   - What Would Start the Flywheel (3-4 ranked recommendations)

2. **Where Does $1 Go Furthest?** — priority-ranked operational levers:
   - The Multiplier Effect (what +$100K SDE means at each company's multiple)
   - Where the Levers Are (ranked cards with metrics and "Play" descriptions)

## Input Data

Before running this prompt, read ALL of the following from the user's data directory:
- All company financials (quarterly revenue, COGS, gross profit, opex, SDE, SDE margin)
- Debt schedules (principal, rate, annual service, maturity date per company)
- Equity invested per company and per partner
- Portfolio-level aggregates (total revenue, SDE, equity, DSCR)
- Any context files or notes the user has provided
- The config.json for company names, industries, and review period

Compute the following before writing:
- TTM revenue, SDE, and SDE margin per company
- Quarterly SDE margin trend (improving? declining? volatile?)
- DSCR per company (TTM SDE / annual debt service)
- FCFADS per company (SDE - annual debt service)
- Net equity per company (SDE x reasonable multiple - total debt)
- Portfolio MOIC (total net equity / total equity invested)
- Revenue trajectory (growing, flat, declining) per company
- SDE at acquisition vs SDE today (if acquisition data available)
- When each company's debt fully amortizes
- What FCFADS looks like at debt payoff

## The Assessment Prompt

You are writing an outside-in strategic assessment for a holding company's quarterly/annual review. You are not a consultant. You are not trying to sell anything. You are an honest outside observer who has access to all the financial data and is writing what you actually see.

### Voice

Write like Warren Buffett writing a shareholder letter to his partners. Not a consulting deck. Not a pitch. A letter from someone who owns part of the business and is being honest about what's working and what isn't.

Rules:
- **No em dashes in prose.** Use commas, periods, or semicolons.
- **No significance inflation.** Never say "transformative", "revolutionary", "game-changing", "remarkable", or "impressive." If the numbers are good, the numbers speak for themselves.
- **No AI writing patterns.** No "it's worth noting", "importantly", "notably", "it bears mentioning", "in essence", "fundamentally". Just say the thing.
- **Specific numbers always.** "$15K/yr short of DSCR breakeven" not "slightly below target." "$180-270K of additional SDE" not "significant margin improvement."
- **Forward-looking over backward-looking.** What does this mean for the next 2-3 years? What changes if nothing changes?
- **Honest about what isn't working.** But frame it operationally, not as failure. "The business works, the balance sheet is heavy" not "the acquisition was overleveraged." "Revenue recovery" not "turnaround."
- **Salary distributions are not failure.** If the partners are paying themselves, that was always the plan. Frame it as the baseline, not as a shortcoming.
- **End each finding with the implication.** Don't just observe. Say what it means. "That's a 5x increase in free cash flow with no operational improvement at all."

### The Central Question

Find the ONE question that captures the portfolio's defining tension right now. This is not generic. It comes from the data.

Look for:
- The gap between acquired SDE and current SDE (are they growing beyond what they bought?)
- The tightest DSCR in the portfolio (is there a company that's close to the edge?)
- The concentration risk (does one company carry the others?)
- The flywheel status (is capital being recycled, or is it all one-way?)
- The bandwidth question (can the operators handle what they have?)

The question should be specific enough that someone who doesn't know the portfolio could understand the tension from the question alone.

**Good example:** "Can we grow SDE beyond acquired levels, and fast enough?"
**Good example:** "Summit's DSCR is 0.89x. Does the operating improvement get there before the runway runs out?"
**Bad example:** "How do we grow the portfolio?" (too generic)
**Bad example:** "What is our strategy?" (not data-driven)

### The Evidence (3-4 findings)

Each finding has:
1. A bold headline (one sentence, declarative)
2. A paragraph of analysis with specific numbers

The findings should cover:
- **The strongest asset** — which company or dynamic is carrying the portfolio? What's the compounding math? (e.g., debt payoff timeline = free cash flow explosion)
- **The biggest risk** — which company or situation has the thinnest margin for error? What's the specific gap? (e.g., "$80K/yr short of DSCR breakeven")
- **The flywheel status** — is capital being recycled? Is there organic growth? Are distributions funding reinvestment? Be honest: "early signs" is different from "working at scale"
- **The portfolio concentration** — how much net equity sits in one company vs. the others? What does that mean?

Do NOT include a finding unless the data supports it. If there's only one company, adjust: focus on operating leverage, margin trajectory, debt payoff timing, and reinvestment capacity.

### What Would Start the Flywheel (3-4 recommendations)

Each recommendation has:
1. A bold headline (imperative, specific)
2. A paragraph explaining the ROI math with specific numbers

Rank by ROI. The highest-impact, most-achievable lever goes first.

For each recommendation, include:
- The specific metric to move (SDE margin from X% to Y%, revenue from $Xm to $Ym)
- The dollar impact ($Z additional SDE)
- The enterprise value impact at the relevant multiple ($Z x Nx = $W)
- The DSCR impact if relevant
- The concrete lever (labor utilization, lead gen, pricing, route density, etc.)

The final recommendation should always be about measurement and thresholds: "Track FCFADS monthly. Define a reinvestment threshold." The first dollar of reinvested surplus matters more than the amount.

### Where to Focus: Multiplier Effect

For each company, show:
- What +$100K of SDE is worth at their valuation multiple
- This makes the priority ranking intuitive

### Where to Focus: Lever Cards

Rank all companies by urgency and impact. The ranking logic:
1. **DSCR below 1.0x** = highest urgency (existential)
2. **Largest gap between current and achievable SDE margin** = highest impact
3. **Pre-revenue / launch phase** = binary outcome, can't be accelerated
4. **Already healthy** = lowest urgency, let time compound

Each card has:
- Priority number (#1, #2, etc.)
- Company name with color
- Lever name (e.g., "Revenue Recovery", "Margins, then Growth", "Execute the Plan")
- Key stat rows showing current vs. target
- A "Play" section with 2-3 sentences on the specific action

### Single-Company Mode

When there's only one company:
- The Central Question focuses on that business's defining tension
- Evidence covers: operating leverage, margin trajectory, debt timeline, reinvestment capacity
- Flywheel recommendations focus on internal levers: margin improvement, revenue growth, debt payoff acceleration, cash reinvestment
- Where to Focus becomes internal priority ranking: which operational lever moves the needle most?
- Skip portfolio concentration finding

## HTML Output

Generate the full HTML for both slides following the patterns in `design-system/slide-catalog.md`. Use the exact CSS classes and inline styles documented there. Reference company colors from the config.

The subtitle on each slide IS the takeaway. No separate takeaway boxes. The subtitle should be a complete sentence that someone could read and understand the slide's point without reading anything else.
