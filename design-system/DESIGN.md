# Holdco Portfolio Review -- Design System

Reference for generating presentation HTML. All slides follow this system exactly.

---

## CSS Variables

```css
:root {
  --bg: #ffffff;
  --sidebar-bg: #ffffff;
  --card-bg: #f8f7f5;
  --accent: #f26522;          /* configurable per user */
  --accent-dim: rgba(242, 101, 34, 0.06);
  --accent-hover: rgba(242, 101, 34, 0.10);
  --text: #141414;
  --text-dim: #64625e;
  --border: #e8e6e1;
  --border-light: #f3f1ed;
  --positive: #047857;
  --negative: #b91c1c;
}
```

Per-company colors are assigned in config and set as CSS variables:

| Variable       | Default  | Meaning |
|----------------|----------|---------|
| `--company-1`  | `#1d4ed8` | Blue   |
| `--company-2`  | `#047857` | Green  |
| `--company-3`  | `#b45309` | Amber  |

- `--accent` is configurable (default `#f26522` orange).
- Positive values use `--positive` (`#047857`).
- Negative values use `--negative` (`#b91c1c`).
- All data-heavy cards use `var(--card-bg)` background.

---

## Typography

### Font Import

```css
@import url('https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Instrument+Sans:wght@400;500;600;700&display=swap');
```

### Font Rules

| Element          | Family           | Size  | Weight | Other                                      |
|------------------|------------------|-------|--------|--------------------------------------------|
| h1               | Instrument Serif | 64px  | 400    | line-height 1.08, letter-spacing -1px      |
| h2               | Instrument Serif | 44px  | 400    | letter-spacing -0.5px                      |
| h3 (section hdr) | Instrument Sans  | 11px  | 600    | uppercase, letter-spacing 2.5px, color text-dim |
| Body text        | Instrument Sans  | 16px  | 400    | line-height 1.7, color text-dim            |
| Data values      | Instrument Sans  | 28px  | 700    | letter-spacing -0.5px                      |
| Labels           | Instrument Sans  | 10px  | 600    | uppercase, letter-spacing 2px, color text-dim |
| Sub text         | Instrument Sans  | 12px  | 400    | color text-dim                             |

**Hard rules:**

- Instrument Serif is used for h1 and h2 ONLY. Italic variant is available.
- Instrument Sans is used for everything else: body, labels, nav, numbers, cards.
- Bold 700 weight for all data values. Never use serif on numbers.

---

## Layout

| Element        | Spec                                                                 |
|----------------|----------------------------------------------------------------------|
| Sidebar        | 220px fixed width, `border-right`, nav items with 2px left border on active |
| Main content   | `flex: 1`, slides absolute positioned with fade/translateY transition |
| Slide padding  | `56px 72px`                                                          |
| Cards          | background `var(--card-bg)`, border-radius 6px, no border, padding 24px |
| Metric grid    | `auto-fit minmax(220px, 1fr)`, gap 12px                             |
| Stat rows      | flex space-between, padding 9px 0, border-bottom `var(--border-light)`, font-size 14px |
| Strategy grid  | `repeat(3, 1fr)`, gap 20px                                          |
| Scorecard      | 2-column grid, gap 20px                                             |

---

## Chart Patterns

### SVG Line Chart (YoY Comparison)

- `viewBox="0 0 300 240"`
- Padding: top 20, right 20, bottom 30, left 55
- Smooth cubic bezier curves: `C cpx,prev.y cpx,curr.y curr.x,curr.y` (where cpx = midpoint between prev.x and curr.x)
- Dots: `r=3`, `fill=color`, `stroke=white`, `stroke-width=1.5`
- Grid lines: `#e5e7eb`, `stroke-width: 0.5`
- Y-axis labels: `font-size: 9`, `fill: #6b7280`
- Nice round tick calculation: divide range by 5, snap to 1/2/5/10 multiples
- Y-axis formats: percentage = `X%`, millions = `X.XM`, thousands = `XK`

### SVG Sequential Timeline Chart

- `viewBox="0 0 300 200"`
- Padding: top 15, right 15, bottom 35, left 50
- Same smooth bezier curve pattern as line chart
- Area fill below line: same color at 0.08 opacity
- X-axis labels: rotated -45deg for space, `font-size: 7`

### SVG Annual Bar Chart

- `viewBox="0 0 300 200"`
- Bars: `border-radius: 3px` on top corners
- Bar width: available width / N bars with gaps
- Value labels above bars: `font-size: 10`, weight 600
- X-axis labels below bars: `font-size: 9`

### Number Formatting (`fmt` function)

| Range        | Format                          | Example             |
|--------------|---------------------------------|---------------------|
| Under $1K    | Show as-is                      | `$850`              |
| $1K -- $999K | Round to nearest K              | `$125K`             |
| $1M -- $10M  | 2 decimal places                | `$4.25M`            |
| $10M+        | 1 decimal place                 | `$12.5M`            |
| Negative     | Parentheses, `color: var(--negative)` | `($42K)` in red |

---

## Interactive Elements

### Valuation Explorer

- Range sliders for SDE multiples per company.
- Real-time calculation of enterprise value, net equity, MOIC.
- Color-coded: negative values in `--negative`, positive in `--positive`.

### Navigation

- Sidebar with bullet nav items.
- Keyboard: arrow keys, Page Up/Down, Home/End.
- Slide counter at bottom of sidebar.
- Key hint in bottom-right corner.

---

## Component Patterns

### Metric Card

```html
<div class="metric-card">
  <div class="value">$9.0M</div>
  <div class="label">TTM REVENUE</div>
  <div class="sub">+18% organic growth YoY</div>
</div>
```

### Stat Row

For data tables within cards.

```html
<div class="stat-row">
  <span class="stat-label">Annual Debt Service</span>
  <span class="stat-value">$792K/yr</span>
</div>
```

### Takeaway Box

```html
<div class="takeaway">
  <strong>KEY TAKEAWAY</strong>
  Narrative text here...
</div>
```

Style: `var(--card-bg)` background, 2px left border in `var(--text)`, 13px font, line-height 1.65.

### Partner Table

```html
<table class="partner-table">
  <thead><tr><th>...</th></tr></thead>
  <tbody><tr><td>...</td></tr></tbody>
</table>
```

Style: `border-collapse: separate`, rounded corners, th uppercase 10px.

### Strategy Card

```html
<div class="strategy-card">
  <h4 style="color:var(--positive)">Strategy Name</h4>
  <p>Description</p>
  <ul>...</ul>
  <div class="projection">$20-25M</div>
  <div class="projection-label">Projected run-rate by 2028</div>
</div>
```

---

## Voice and Content Rules

These rules govern all generated text in presentations.

1. **Subtitle IS the takeaway** (IB convention). No separate takeaway boxes on most slides.
2. **Humanized prose.** No em dashes, no significance inflation, no AI slop.
3. **Forward-looking over backward-looking.**
4. **Specific numbers always.** Write "$15K/yr short" not "slightly below."
5. **Bold 700 weight for all data values.** Never use serif on numbers.
