# Slide Catalog

Reference for every slide type in the holdco-portfolio-review presentation system. Each entry documents structure, HTML, and data requirements so Claude can generate presentations without guessing at markup.

---

## 1. Welcome

**Description:** Full-bleed title slide. HoldCo name, review period, optional tagline. Clean and minimal — just branding and context.

**When to include:** Always. First slide in every presentation.

**HTML:**

```html
<section class="slide" data-index="0">
  <h3>HOLDCO NAME</h3>
  <h1>Offsite 2026</h1>
  <p>Quarterly portfolio review. June 2026.</p>
</section>
```

**Data requirements:** HoldCo name, review period/date. No CSV needed.

---

## 2. Portfolio Overview

**Description:** Aggregate metrics across all companies. Metric grid showing TTM Revenue, TTM SDE/EBITDA, Total Equity, Portfolio DSCR, Net Equity, MOIC.

**When to include:** Multi-company mode only. Skip if single-company (go straight to company detail).

**HTML:**

```html
<section class="slide" data-index="N">
  <h2>The Portfolio Today</h2>
  <p>Subtitle that IS the takeaway: e.g. "$17.8M revenue across four companies. $2.6M SDE/EBITDA on $2.7M equity."</p>
  <div class="metrics-grid">
    <div class="metric-card">
      <div class="value">$17.8M</div>
      <div class="label">TTM REVENUE</div>
      <div class="sub">Across 4 companies</div>
    </div>
    <!-- more metric cards -->
  </div>
</section>
```

**Data requirements:** `portfolio.csv` — aggregated financials across all companies.

---

## 3. Company Summary (one per company)

**Description:** Company-level scorecard. Two-column grid with stat rows covering operating metrics and capital structure. Company name styled with its color. Subtitle is the takeaway.

**When to include:** Always. One slide per company in the portfolio.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2><span class="company-color">Company Name</span></h2>
  <p>Subtitle takeaway: e.g. "TTM revenue $9.0M. SDE/EBITDA $1.0M at 11%. DSCR 1.26x. The margin gap is the opportunity."</p>
  <div class="scorecard">
    <div class="scorecard-section">
      <h4>OPERATING</h4>
      <div class="stat-row"><span class="stat-label">TTM Revenue</span><span class="stat-value">$9.0M</span></div>
      <div class="stat-row"><span class="stat-label">SDE/EBITDA</span><span class="stat-value">$1.0M</span></div>
      <div class="stat-row"><span class="stat-label">SDE/EBITDA Margin</span><span class="stat-value">11%</span></div>
      <!-- more stat rows -->
    </div>
    <div class="scorecard-section">
      <h4>CAPITAL STRUCTURE</h4>
      <div class="stat-row"><span class="stat-label">Total Debt</span><span class="stat-value">$3.0M</span></div>
      <div class="stat-row"><span class="stat-label">Annual Service</span><span class="stat-value">$792K</span></div>
      <div class="stat-row"><span class="stat-label">DSCR</span><span class="stat-value">1.26x</span></div>
      <div class="stat-row"><span class="stat-label">Equity Invested</span><span class="stat-value">$1.2M</span></div>
      <!-- more stat rows -->
    </div>
  </div>
</section>
```

**Data requirements:** `companies.csv` + financials for each company.

---

## 4. P&L Deep Dive (per company) — RECOMMENDED

**Description:** Three chart tiers stacked vertically. Each tier has 3 charts side by side (Revenue, Gross Margin %, SDE %). Top tier is annual bars, middle is full timeline line/area, bottom is YoY quarterly comparison.

**When to include:** Auto-included if quarterly financial data exists for the company. Highly recommended — this is where the trend story lives.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2><span class="company-color">Company Name</span> — P&L Deep Dive</h2>
  <p>Subtitle takeaway with trend narrative.</p>

  <h3>Annual Summary</h3>
  <div style="display:grid; grid-template-columns:1fr 1fr 1fr; gap:16px;">
    <div class="metric-card" style="padding:16px 20px;">
      <div style="font-size:11px; text-transform:uppercase; letter-spacing:2px; color:var(--text-dim); font-weight:600; margin-bottom:8px;">Revenue by Year</div>
      <svg id="svg-{company}-rev-annual" viewBox="0 0 300 200" style="width:100%; height:200px;"></svg>
    </div>
    <div class="metric-card" style="padding:16px 20px;">
      <div style="font-size:11px; text-transform:uppercase; letter-spacing:2px; color:var(--text-dim); font-weight:600; margin-bottom:8px;">Gross Margin %</div>
      <svg id="svg-{company}-gm-annual" viewBox="0 0 300 200" style="width:100%; height:200px;"></svg>
    </div>
    <div class="metric-card" style="padding:16px 20px;">
      <div style="font-size:11px; text-transform:uppercase; letter-spacing:2px; color:var(--text-dim); font-weight:600; margin-bottom:8px;">SDE/EBITDA %</div>
      <svg id="svg-{company}-sde-annual" viewBox="0 0 300 200" style="width:100%; height:200px;"></svg>
    </div>
  </div>

  <h3>Full Timeline</h3>
  <div style="display:grid; grid-template-columns:1fr 1fr 1fr; gap:16px;">
    <!-- Sequential line charts with area fill, same 3-column layout -->
    <div class="metric-card" style="padding:16px 20px;">
      <div style="font-size:11px; text-transform:uppercase; letter-spacing:2px; color:var(--text-dim); font-weight:600; margin-bottom:8px;">Revenue</div>
      <svg id="svg-{company}-rev-timeline" viewBox="0 0 300 200" style="width:100%; height:200px;"></svg>
    </div>
    <!-- Gross Margin %, SDE/EBITDA % -->
  </div>

  <h3>Quarterly YoY Comparison</h3>
  <div style="display:grid; grid-template-columns:1fr 1fr 1fr; gap:16px;">
    <!-- YoY overlay line charts with legend -->
    <div class="metric-card" style="padding:16px 20px;">
      <div style="font-size:11px; text-transform:uppercase; letter-spacing:2px; color:var(--text-dim); font-weight:600; margin-bottom:8px;">Revenue YoY</div>
      <svg id="svg-{company}-rev-yoy" viewBox="0 0 300 200" style="width:100%; height:200px;"></svg>
    </div>
    <!-- Gross Margin %, SDE/EBITDA % -->
  </div>
</section>
```

**Data requirements:** `financials/{company}.csv` — must have quarterly data (columns for period, revenue, COGS or gross margin, the chosen profitability metric).

**Notes:** SVG charts are drawn by JavaScript at render time. The `id` attributes follow the pattern `svg-{company}-{metric}-{tier}` so the chart engine can target them.

---

## 5. Valuation Explorer — RECOMMENDED

**Description:** Interactive grid of company valuation cards. Each card has a range slider for the SDE/EBITDA multiple. Computed values (Enterprise Value, Less Debt, Net Equity, MOIC) update in real-time via JavaScript.

**When to include:** Auto-included if the chosen profitability metric and debt data exist. Core to any PE review.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2>Valuation Explorer</h2>
  <p>Subtitle: e.g. "Adjust multiples to see how valuations shift across the portfolio."</p>
  <div class="val-grid">
    <div class="val-card">
      <h4><span class="dot" style="background:var(--company-1)"></span> Company Name</h4>
      <div class="slider-group">
        <label>Multiple</label>
        <input type="range" id="mult-1" min="4" max="10" step="0.5" value="7.5">
        <span class="slider-val" id="mult-1-val">7.5x</span>
      </div>
      <div class="stat-row"><span class="stat-label">Enterprise Value</span><span class="stat-value" id="ev-1">$7.5M</span></div>
      <div class="stat-row"><span class="stat-label">Less Debt</span><span class="stat-value">($3.0M)</span></div>
      <div class="stat-row"><span class="stat-label">Net Equity</span><span class="stat-value" id="neq-1">$4.5M</span></div>
      <div class="stat-row"><span class="stat-label">MOIC</span><span class="stat-value" id="moic-1">3.4x</span></div>
    </div>
    <!-- more company cards -->
  </div>
</section>
```

**Data requirements:** `companies.csv` — needs TTM SDE/EBITDA, total debt, equity invested per company.

**Notes:** JavaScript wires up each slider to recompute EV = SDE/EBITDA x Multiple, Net Equity = EV - Debt, MOIC = Net Equity / Equity Invested. Slider `id` attributes follow the pattern `mult-{N}`, and computed fields use `ev-{N}`, `neq-{N}`, `moic-{N}`.

---

## 6. Debt Schedule & DSCR Analysis — MANDATORY

**Description:** Per-company debt breakdown with terms, DSCR calculation, and amortization timeline showing when debt service drops.

**When to include:** Always. Every PE review must surface debt coverage.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2>Debt Schedule &amp; Coverage</h2>
  <p>Subtitle: e.g. "Portfolio DSCR 1.27x. ShelfGenie at 0.98x is the watch item."</p>
  <div style="display:grid; grid-template-columns:1fr 1fr; gap:20px; margin-top:24px;">
    <div class="metric-card">
      <h4>Company Name</h4>
      <div class="stat-row"><span class="stat-label">Total Debt</span><span class="stat-value">$3.0M</span></div>
      <div class="stat-row"><span class="stat-label">Annual Service</span><span class="stat-value">$792K/yr</span></div>
      <div class="stat-row"><span class="stat-label">TTM SDE/EBITDA</span><span class="stat-value">$1.0M</span></div>
      <div class="stat-row"><span class="stat-label">DSCR</span><span class="stat-value" style="color:var(--positive)">1.26x</span></div>
      <div class="stat-row"><span class="stat-label">Debt-free by</span><span class="stat-value">2030</span></div>
      <div class="sub" style="margin-top:12px;">Drops to $240K/yr in 2029. FCFADS goes from $208K to $1M.</div>
    </div>
    <!-- more companies -->
  </div>
</section>
```

**Data requirements:** `debt-schedule.csv` — loan balances, interest rates, annual service amounts, maturity dates per company.

**Notes:** Color-code DSCR values: `var(--positive)` for >= 1.25x, `var(--warning)` for 1.0-1.25x, `var(--negative)` for < 1.0x.

---

## 7. Outside-In Strategic Assessment — MANDATORY

**Description:** The crown jewel slide. Three sections: central question, evidence, recommendations. A Central Question card (accent-dim background, bold question), The Evidence (left panel, 3-4 numbered findings), and Recommendations (right panel, 3-4 recommendations). This slide is generated by the outside-in assessment prompt — it is NOT templated from data alone.

**When to include:** Always. This is the analytical core of the presentation.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2>An Outside-In View</h2>
  <p>Brief context. Claude's outside-in perspective.</p>

  <div class="metric-card" style="margin-top:24px; padding:24px; background:var(--accent-dim);">
    <div style="font-size:18px; font-weight:700; color:var(--accent); margin-bottom:8px;">The Central Question</div>
    <div style="font-size:14px; color:var(--text); line-height:1.7;">
      Honest, data-driven central question with specific numbers...
      <strong>The pointed question.</strong>
    </div>
  </div>

  <div style="display:grid; grid-template-columns:1fr 1fr; gap:20px; margin-top:20px;">
    <div class="metric-card" style="padding:28px;">
      <h4 style="font-size:11px; text-transform:uppercase; letter-spacing:2px; color:var(--accent); font-weight:600; margin-bottom:20px;">The Evidence</h4>
      <div style="margin-bottom:20px;">
        <div style="font-size:14px; font-weight:600; margin-bottom:6px;">1. Finding headline.</div>
        <div style="font-size:13px; color:var(--text-dim); line-height:1.6;">Analysis paragraph with specific numbers and honest assessment...</div>
      </div>
      <div style="margin-bottom:20px;">
        <div style="font-size:14px; font-weight:600; margin-bottom:6px;">2. Finding headline.</div>
        <div style="font-size:13px; color:var(--text-dim); line-height:1.6;">Analysis paragraph...</div>
      </div>
      <!-- 1-2 more findings -->
    </div>
    <div class="metric-card" style="padding:28px;">
      <h4 style="font-size:11px; text-transform:uppercase; letter-spacing:2px; color:var(--positive); font-weight:600; margin-bottom:20px;">Recommendations</h4>
      <div style="margin-bottom:20px;">
        <div style="font-size:14px; font-weight:600; margin-bottom:6px;">1. Recommendation headline.</div>
        <div style="font-size:13px; color:var(--text-dim); line-height:1.6;">Specific, numbers-driven recommendation...</div>
      </div>
      <div style="margin-bottom:20px;">
        <div style="font-size:14px; font-weight:600; margin-bottom:6px;">2. Recommendation headline.</div>
        <div style="font-size:13px; color:var(--text-dim); line-height:1.6;">Specific recommendation...</div>
      </div>
      <!-- 1-2 more recommendations -->
    </div>
  </div>
</section>
```

**Data requirements:** ALL data files. This slide is generated by a prompt that analyzes the full dataset, not filled from a template.

**Notes:** The central question must be honest and data-grounded. No softball questions. Evidence section uses `var(--accent)` header color; Recommendations section uses `var(--positive)`.

---

## 8. Where to Focus — MANDATORY

**Description:** Two parts. First: the Multiplier Effect — what +$100K SDE/EBITDA means at each company's multiple. Second: priority-ranked lever cards, each with company color border, key metrics, and a "Play" description.

**When to include:** Always. This is where the review becomes actionable.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2>Where Does $1 Go Furthest?</h2>
  <p>Subtitle takeaway with highest-impact lever.</p>

  <h3>The Multiplier Effect</h3>
  <div style="display:grid; grid-template-columns:repeat(N, 1fr); gap:16px;">
    <div class="metric-card" style="text-align:center; border-left:4px solid var(--company-1);">
      <div class="label company-1-color">Company (Xx)</div>
      <div style="margin:12px 0;"><span style="font-size:14px; color:var(--text-dim);">+$100K SDE/EBITDA =</span></div>
      <div class="value company-1-color" style="font-size:36px;">+$750K</div>
      <div class="sub">enterprise value</div>
    </div>
    <!-- one card per company -->
  </div>

  <h3>Where the Levers Are (in priority order)</h3>
  <div style="display:grid; grid-template-columns:1fr 1fr; gap:16px;">
    <div class="metric-card" style="border-left:4px solid var(--company-N);">
      <h4>#1 <span class="company-color">Company</span> — Lever Name</h4>
      <div class="stat-row"><span class="stat-label">Current SDE/EBITDA</span><span class="stat-value">$1.0M</span></div>
      <div class="stat-row"><span class="stat-label">SDE/EBITDA Margin</span><span class="stat-value">11%</span></div>
      <div class="stat-row"><span class="stat-label">Potential</span><span class="stat-value">+$200K</span></div>
      <div style="margin-top:12px; font-size:13px; color:var(--text-dim); line-height:1.6;">
        <strong style="color:var(--text);">Play:</strong> Specific action plan with numbers and timeline.
      </div>
    </div>
    <!-- more lever cards, numbered by priority -->
  </div>
</section>
```

**Data requirements:** ALL data files. Rankings are generated by analysis, not hardcoded.

**Notes:** Multiplier Effect grid columns should match the number of companies (`repeat(N, 1fr)`). Lever cards are numbered #1, #2, etc. in priority order — the sequencing itself is the insight.

---

## 9. Next Steps / Discussion — MANDATORY

**Description:** Closes the deck with decisions to make. "The Big Two" strategic questions (2-column, accent border), then "One Question per Company" (2-column, company-colored borders).

**When to include:** Always. Last slide in the presentation.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2>What Do We Decide Today?</h2>

  <h3>The Big Two</h3>
  <div style="display:grid; grid-template-columns:1fr 1fr; gap:16px; margin-top:12px;">
    <div class="metric-card" style="padding:28px; border-left:4px solid var(--accent);">
      <div style="font-size:20px; font-weight:700; color:var(--accent); margin-bottom:8px;">Strategic question?</div>
      <div style="font-size:14px; color:var(--text-dim); line-height:1.6;">Context for the question with relevant numbers.</div>
    </div>
    <div class="metric-card" style="padding:28px; border-left:4px solid var(--accent);">
      <div style="font-size:20px; font-weight:700; color:var(--accent); margin-bottom:8px;">Second strategic question?</div>
      <div style="font-size:14px; color:var(--text-dim); line-height:1.6;">Context for the question.</div>
    </div>
  </div>

  <h3>One Question per Company</h3>
  <div style="display:grid; grid-template-columns:1fr 1fr; gap:16px; margin-top:12px;">
    <div class="metric-card" style="padding:24px; border-left:4px solid var(--company-1);">
      <div style="font-size:18px; font-weight:700; color:var(--company-1); margin-bottom:6px;">Company Name</div>
      <div style="font-size:15px; color:var(--text); line-height:1.6;">Specific question for this company?</div>
    </div>
    <!-- one card per company -->
  </div>
</section>
```

**Data requirements:** Generated from analysis. Questions should surface the real tensions in the data.

---

## 10. Distributions — OPTIONAL

**Description:** Distribution history by company and type (salary, profit, reinvested). Metric cards for totals and breakdown.

**When to include:** When distribution data is provided and the audience wants to see capital returned.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2>Capital Returned</h2>
  <p>Subtitle: e.g. "Total distributions of $1.2M across salary and profit."</p>
  <div class="metrics-grid">
    <div class="metric-card">
      <div class="value">$1.2M</div>
      <div class="label">TOTAL DISTRIBUTED</div>
    </div>
    <div class="metric-card">
      <div class="value">$800K</div>
      <div class="label">SALARY</div>
    </div>
    <div class="metric-card">
      <div class="value">$400K</div>
      <div class="label">PROFIT</div>
    </div>
  </div>
  <!-- per-company breakdown cards if needed -->
</section>
```

**Data requirements:** `distributions.csv` — amounts, types, dates, recipient companies.

---

## 11. Partner Economics — OPTIONAL

**Description:** Partner table showing equity per company, ownership percentage, vesting status. Holdco-level ownership breakdown and per-partner totals.

**When to include:** When partner/ownership data is provided and the audience includes partners.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2>Partner Economics</h2>
  <p>Subtitle with total equity and partner count.</p>
  <table class="partner-table">
    <thead>
      <tr><th>Partner</th><th>Company 1</th><th>Company 2</th><th>Total</th><th>HoldCo %</th></tr>
    </thead>
    <tbody>
      <tr><td>Name</td><td>$335K</td><td>$120K</td><td>$455K</td><td>37.25%</td></tr>
      <!-- more rows -->
    </tbody>
  </table>
</section>
```

**Data requirements:** `partners.csv` — partner names, equity amounts per company, ownership percentages, vesting details.

---

## 12. Growth Strategy — OPTIONAL

**Description:** 3-column strategy grid with options (e.g., Organic / Hybrid / Full Acquisition). Each card has a description, bullet points, and projected run-rate. Key questions section at the bottom.

**When to include:** When the review includes a forward-looking growth discussion or acquisition pipeline.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2>Grow &amp; Harvest</h2>
  <p>Subtitle framing the strategic options.</p>
  <div style="display:grid; grid-template-columns:1fr 1fr 1fr; gap:20px; margin-top:24px;">
    <div class="metric-card" style="padding:28px;">
      <h4 style="color:var(--accent);">Option A: Organic</h4>
      <div style="font-size:13px; color:var(--text-dim); line-height:1.6; margin:12px 0;">Description of the organic growth path.</div>
      <ul style="font-size:13px; color:var(--text-dim); line-height:1.8; padding-left:16px;">
        <li>Key point one</li>
        <li>Key point two</li>
      </ul>
      <div class="stat-row" style="margin-top:16px;"><span class="stat-label">Projected Run-Rate</span><span class="stat-value">$X.XM</span></div>
    </div>
    <!-- Option B, Option C -->
  </div>
  <h3>Key Questions</h3>
  <div class="metric-card" style="padding:24px; margin-top:12px;">
    <div style="font-size:14px; color:var(--text); line-height:1.8;">
      1. Question one?<br>
      2. Question two?
    </div>
  </div>
</section>
```

**Data requirements:** Strategic context and projections. Often generated rather than data-driven.

---

## 13. Timeline / Journey

**Description:** Horizontal timeline showing acquisition dates, launches, and key milestones. Dot markers on a line with alternating above/below content. Good for annual reviews showing the year's key events.

**When to include:** Always. Shows the holdco's journey and provides context for the numbers that follow.

**HTML:**

```html
<section class="slide slide-scroll" data-index="N">
  <h2>The Year in Review</h2>
  <p>Subtitle with date range.</p>
  <div style="position:relative; margin:40px 0; padding:0 40px;">
    <!-- Horizontal line -->
    <div style="position:absolute; top:50%; left:40px; right:40px; height:2px; background:var(--border);"></div>
    <div style="display:flex; justify-content:space-between; position:relative;">
      <div style="text-align:center; width:120px;">
        <div style="width:12px; height:12px; border-radius:50%; background:var(--accent); margin:0 auto 12px;"></div>
        <div style="font-size:12px; font-weight:600; color:var(--text);">Q1 2025</div>
        <div style="font-size:11px; color:var(--text-dim); margin-top:4px;">Milestone description</div>
      </div>
      <!-- more milestone dots -->
    </div>
  </div>
</section>
```

**Data requirements:** List of milestones with dates and descriptions.

**Notes:** Milestones should include acquisition dates, product/service launches, and key inflection points. Optionally, a Team Photos slide can follow this one if team photos are provided — it humanizes the portfolio before diving into financials.

---

## 14. Preamble — OPTIONAL

**Description:** Context-setting narrative slide. Includes the user's central message plus a relevant excerpt from a holdco leader (Buffett, Constellation Software, Permanent Equity, Chenmark, Decada Group, Acquiring Minds, etc.). Minimal layout.

**When to include:** When the presentation benefits from a philosophical or contextual opening before diving into numbers.

**HTML:**

```html
<section class="slide" data-index="N">
  <h2>Before We Begin</h2>
  <blockquote style="font-size:18px; font-style:italic; color:var(--text); line-height:1.8; border-left:4px solid var(--accent); padding-left:24px; margin:40px 0;">
    "A quote or framing statement that sets the tone for the review."
  </blockquote>
  <p style="font-size:14px; color:var(--text-dim); line-height:1.7;">
    Optional narrative paragraph providing context.
  </p>
</section>
```

**Data requirements:** None. Content is authored, not data-driven.

---

## Layout Notes

| Concern | Rule |
|---------|------|
| Vertical overflow | Use `slide-scroll` class on any slide that might exceed viewport height |
| Navigation | Always set `data-index` attributes sequentially for sidebar/keyboard nav |
| Company colors | Set as CSS custom properties (`--company-1`, `--company-2`, etc.) and reference throughout |
| Single-company mode | Omit Portfolio Overview, use simpler sidebar, company summary becomes the opening data slide |
| Subtitle convention | Every `<p>` immediately after an `<h2>` IS the takeaway, not a description. Write it like an IB subtitle. |

## Slide Ordering

**Standard multi-company deck:**

1. Welcome
2. Preamble (if used)
3. Timeline / Journey
4. Portfolio Overview
5. Company Summary (per company)
6. P&L Deep Dive (per company, if quarterly data exists)
7. Valuation Explorer
8. Debt Schedule & DSCR Analysis
9. Outside-In Strategic Assessment
10. Where to Focus
11. Distributions (if data exists)
12. Partner Economics (if data exists)
13. Growth Strategy (if relevant)
14. Next Steps / Discussion

**Single-company deck:** Drop slide 3 (Portfolio Overview). Everything else applies.
