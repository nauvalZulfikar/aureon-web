# Aureon — Website Build Document

> **Instructions for agentic AI (Claude in VS Code or similar):**
> Build a single-file static website (`aureon.html`) that exactly matches the design spec below.
> Output is one self-contained HTML file — no frameworks, no build tools, no dependencies beyond Google Fonts.
> Read this entire document before writing a single line of code.

---

## Project Overview

**Client:** Aureon — a Data, AI & Automation IT agency  
**Founder:** Nauval Zulfikar, Senior Data Scientist based in Bandung, Indonesia  
**Output:** `aureon.html` — single file, fully self-contained  
**Stack:** Vanilla HTML + CSS + Canvas JS. Zero npm, zero build step.

---

## Design System

### Fonts
Load from Google Fonts — include both in a single `<link>` tag:

```
https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600&family=Space+Mono:ital,wght@0,400;0,700;1,400&display=swap
```

- **Space Grotesk** — body copy, headings, buttons
- **Space Mono** — all UI chrome: eyebrows, nav, tags, code blocks, stat numbers, monospace labels

### Color Palette (CSS custom properties)

```css
:root {
  /* Zinc scale — dark backgrounds */
  --zinc-950: #0D0D0F;   /* page background */
  --zinc-900: #111113;   /* secondary surface */
  --zinc-800: #1C1C20;   /* card backgrounds, borders */
  --zinc-700: #3A3A42;   /* subtle borders */
  --zinc-600: #52525E;   /* disabled/muted borders */
  --zinc-500: #888896;   /* placeholder text */
  --zinc-400: #ABABBA;   /* secondary text */
  --zinc-300: #C8C8D4;   /* muted text */
  --zinc-200: #E0E0E8;   /* body text on dark */
  --zinc-100: #F2F2F6;   /* primary text on dark */

  /* Gold accent — primary brand color */
  --gold:        #F5C542;
  --gold-bright: #FFD966;
  --gold-dim:    rgba(245,197,66,0.12);
  --gold-border: rgba(245,197,66,0.25);
  --gold-deep:   #1A1500;  /* text ON gold backgrounds */

  --white: #FFFFFF;
  --radius: 8px;
  --radius-lg: 14px;
}
```

### Typography Rules

- `body`: Space Grotesk, `font-weight: 500`, `color: var(--zinc-100)`, `background: var(--zinc-950)`
- Section titles: `clamp(28px, 3.5vw, 46px)`, `font-weight: 600`, `letter-spacing: -1px`, `color: var(--white)`
- Hero H1: `clamp(46px, 6.5vw, 84px)`, `font-weight: 600`, `letter-spacing: -2.5px`, `line-height: 1.0`
- Eyebrow labels: Space Mono, `10px`, `letter-spacing: 2px`, `text-transform: uppercase`, `color: var(--gold-bright)` — always preceded by an `::before` pseudo-element: `width: 18px; height: 1px; background: var(--gold-bright)`
- Body text: `13–16px`, `font-weight: 300–400`, `color: var(--zinc-200)`, `line-height: 1.65–1.75`
- Monospace labels/tags: Space Mono, `10–11px`, `letter-spacing: 0.5–1.5px`

### Buttons

**Primary** (gold, dark text):
```css
background: var(--gold); color: var(--gold-deep);
padding: 11px 24px; border-radius: var(--radius);
font-size: 14px; font-weight: 600; font-family: 'Space Grotesk', sans-serif;
transition: background 0.15s, transform 0.1s;
/* hover: background: var(--gold-bright); transform: translateY(-1px) */
```

**Outline** (transparent, zinc border):
```css
background: transparent; color: var(--zinc-200);
border: 1px solid var(--zinc-700); padding: 11px 24px;
border-radius: var(--radius); font-size: 14px;
/* hover: border-color: var(--zinc-200); color: var(--white) */
```

---

## Page Structure

Build these sections **in this exact order**:

```
1. <nav>          — fixed top navigation
2. <section.hero> — full-height hero with binary rain canvas
3. <div.ticker>   — scrolling tech ticker bar
4. <section#services .sec-mid>   — services grid + stats row
5. <section#approach .sec-dark>  — approach steps + YAML code block
6. <section#sectors .sec-mid>    — industry sectors grid
7. <section#work .sec-dark>      — case studies grid
8. <section#team .sec-mid>       — founder profile (NOT a team grid)
9. <section#contact .cta-sec>    — CTA with binary rain canvas
10. <footer>      — minimal footer
11. <script>      — binary rain animation (inline, at bottom of body)
```

### Section Background Pattern

Alternate between two surface classes to create visual depth:

```css
.sec-dark { background: var(--zinc-950); padding: 96px 5%; }
.sec-mid  { background: var(--zinc-900); border-top: 1px solid var(--zinc-800);
            border-bottom: 1px solid var(--zinc-800); padding: 96px 5%; }
```

---

## Section Specs

### 1. Navigation (`<nav>`)

- `position: fixed`, `height: 60px`, `z-index: 100`
- `background: rgba(13,13,15,0.9)`, `backdrop-filter: blur(20px)`
- `border-bottom: 1px solid var(--zinc-800)`
- **Logo:** Space Mono, `16px`, `letter-spacing: 1px`, text: `AUREON` with a small gold dot `●` (8×8px circle, `background: var(--gold-bright)`, `box-shadow: 0 0 10px var(--gold-bright)`) to the left
- **Nav links:** `font-size: 13px`, `color: var(--zinc-200)`, gap: `32px`
- **CTA button** (last link): `background: var(--gold)`, `color: var(--gold-deep)`, `font-weight: 600`, `border-radius: var(--radius)`, text: `Contact`
- Links: Services, Approach, Sectors, Work, Founder, Contact

### 2. Hero Section

**Layout:** `display: flex`, `align-items: center`, `min-height: 100vh`, `padding: 100px 5% 80px`

**Background layers (stacked, all `position: absolute; inset: 0; pointer-events: none`):**
1. `.rain-canvas` — binary rain (Canvas element, `id="hero-rain"`, `z-index: 0`, `opacity: 0.18`)
2. `.hero-grid` — gold grid lines: `background-image: linear-gradient(rgba(245,197,66,0.04) 1px, transparent 1px), linear-gradient(90deg, ...)`, `background-size: 80px 80px`
3. `.hero-glow` — `width: 800px; height: 600px; background: radial-gradient(ellipse, rgba(245,197,66,0.09) 0%, transparent 65%); top: -100px; right: -150px`
4. `.hero-scanlines` — `background-image: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(255,255,255,0.013) 2px, rgba(255,255,255,0.013) 4px)`

**Left content (`.hero-left`):** `position: relative; z-index: 1; max-width: 680px; flex: 1`
- Eyebrow: `DATA · AI · AUTOMATION AGENCY`
- H1 (three lines):
  ```html
  We build<br>
  <span class="dimmed">intelligence</span><br>  <!-- color: var(--zinc-400) -->
  <span class="accent">that ships.</span>        <!-- color: var(--gold-bright) -->
  ```
- Subtext: `"End-to-end data science, ML pipelines, and intelligent automation for organizations that measure success in production — not proof of concepts."`
- Two buttons: `See our work` (primary → `#work`) and `Start a project →` (outline → `#contact`)
- Apply fade-up entrance animations with staggered delays (0.05s, 0.15s, 0.27s, 0.4s)

**Right content (`.hero-terminal`):** `width: 360px; flex-shrink: 0; margin-left: 60px`
- Fake macOS window with three colored dots (red `#FF5F57`, amber `#FEBC2E`, green `#28C840`)
- Title bar text: `aureon_pipeline.py` (Space Mono, centered, `color: var(--zinc-200)`)
- Code body (Space Mono, 12px, line-height 1.85) showing Python code:
  ```python
  # Aureon ML Pipeline v2.4
  
  from aureon import Pipeline
  from aureon.ml import AutoML
  
  # Init pipeline
  pipe = Pipeline(env="prod")
  
  # Connect sources
  pipe.ingest([
    "postgres://db:5432",
    "s3://data-lake/raw"
  ])
  
  # Train & deploy
  model = AutoML.fit(pipe)
  model.deploy(target="api")
  
  >>> Accuracy: 94.2% |
  ```
- Syntax highlight classes: `.tk` purple `#C084FC` (keywords), `.tf` blue `#60A5FA` (module names), `.ts` green `#86EFAC` (strings), `.tr` gold `var(--gold-bright)` (method calls), `.tc` italic zinc (comments)
- Blinking cursor: `7×13px` block, `background: var(--gold-bright)`, `animation: blink 1.1s step-end infinite`
- **Hide terminal on screens ≤ 1100px**

### 3. Ticker Bar

```css
background: var(--zinc-900);
border-top/bottom: 1px solid var(--zinc-800);
padding: 13px 0; overflow: hidden; white-space: nowrap;
```

- Space Mono, `10px`, `letter-spacing: 1.5px`, `color: var(--zinc-300)`, uppercase
- Items: `Machine Learning`, `ETL Pipelines`, `LLM Integration`, `Process Automation`, `BI Dashboards`, `NLP Systems`, `Predictive Modeling`, `RAG Architecture`, `Government Tech`, `A/B Testing`
- Duplicate the full list inside `.ticker-track` (copy twice) so the loop is seamless
- Each item separated by `//` in gold (via `::after` pseudo-element)
- Animation: `transform: translateX(0) → translateX(-50%)`, `30s linear infinite`

### 4. Services Section (`#services`, `.sec-mid`)

**Header:** flex row, space-between
- Left: eyebrow `SERVICES`, title `What we build.`
- Right: subtext `"Full-stack data and AI — from ingestion to inference to insight."`

**Services grid:** `display: grid; grid-template-columns: repeat(3, 1fr); gap: 1px; background: var(--zinc-800); border: 1px solid var(--zinc-800); border-radius: var(--radius-lg); overflow: hidden`

Each `.svc-card`: `background: var(--zinc-900); padding: 32px 28px`
- Number (Space Mono, `10px`, `color: var(--zinc-200)`)
- Title (`16px`, `font-weight: 500`, `color: var(--white)`)
- Description (`13px`, `color: var(--zinc-200)`, `line-height: 1.75`)
- Tech tags (Space Mono, `10px`, `background: var(--zinc-800)`, `border: 1px solid var(--zinc-700)`, `color: var(--zinc-200)`, `border-radius: 4px`)

**First card (`.svc-card.hot`)** — featured/AI card:
- `background: #151300` (very subtle gold tint)
- `::before` pseudo: `top: 0; left: 0; right: 0; height: 2px; background: linear-gradient(90deg, var(--gold), var(--gold-bright))`
- Title color: `var(--gold-bright)`
- Tags: `background: var(--gold-dim); color: var(--gold-bright); border-color: var(--gold-border)`

**Six service cards:**
| # | Title | Tags |
|---|-------|------|
| 01 (hot) | AI & Machine Learning | Deep Learning, NLP, LLM, RAG, Forecasting |
| 02 | ETL & Data Engineering | Python, PySpark, AWS, PostgreSQL |
| 03 | BI & Dashboards | Power BI, Streamlit, Metabase |
| 04 | Process Automation | RPA, FastAPI, n8n, Docker |
| 05 | Government Digital Systems | GIS, Open Data, VPS, Docker |
| 06 | Marketing Analytics | A/B Testing, Attribution, Segmentation |

**Stats row** below the grid — `display: grid; grid-template-columns: repeat(4, 1fr); gap: 1px; background: var(--zinc-800); border-radius: var(--radius-lg); overflow: hidden; margin-top: 24px`

Each `.stat-block`: `background: var(--zinc-950); padding: 32px 28px`
- Number: Space Mono, `38px`, `font-weight: 700`, `color: var(--white)`, `letter-spacing: -2px` — suffix `%` or `+` in `color: var(--gold-bright)`
- Label: `12px`, `color: var(--zinc-300)`

| Number | Label |
|--------|-------|
| 45% | avg. efficiency gain |
| 5+ | years in production AI |
| 20+ | systems deployed |
| 4 | sectors served |

### 5. Approach Section (`#approach`, `.sec-dark`)

Eyebrow: `HOW WE WORK` · Title: `Outcome-first delivery.`

**Two-column grid:** `grid-template-columns: 1fr 1.05fr; gap: 56px; align-items: start; margin-top: 56px`

**Left — step list:**
Five `.appr-step` rows, each `display: flex; gap: 22px; padding: 24px 0; border-bottom: 1px solid var(--zinc-800)`
- Step number: Space Mono `11px`, `color: var(--zinc-200)` (active: `var(--gold-bright)`)
- Title: `15px`, `font-weight: 500`, `color: var(--zinc-300)` (active: `var(--white)`)
- Description: `13px`, `color: var(--zinc-200)`
- Mark steps 01–03 as `.on` (active); 04–05 are inactive

| Step | Title | Description |
|------|-------|-------------|
| 01 | Discovery & data audit | Map your data landscape — sources, quality, gaps — and define success metrics before touching code. |
| 02 | Architecture design | Stack selection, pipeline blueprints, and infrastructure planning. Everything documented before we build. |
| 03 | Agile build | Short sprints, working software at each milestone, stakeholder reviews built in. No surprises at handover. |
| 04 | Deploy & monitor | Production rollout, model monitoring, alerting, and performance dashboards from day one. |
| 05 | Sustain & evolve | Retraining pipelines, system maintenance, and roadmap planning as your data grows. |

**Right — YAML code block** (`position: sticky; top: 80px`):
- Header bar: Space Mono filename `project_lifecycle.yaml` + badge `YAML` (gold dim bg, gold text)
- Body: Space Mono `11.5px`, `line-height: 1.9`, with syntax highlighting:
  - `.ck` purple — YAML keys
  - `.cs` green — string values
  - `.cn` pink — numbers
  - `.cf` blue — variable references
  - `.cr` gold — `COMPLETE` status values
  - `.cc` italic zinc — comments
  - `.ln` zinc — line numbers (non-selectable)
- Content: show phases with statuses `COMPLETE` (gold), `IN_PROGRESS` (blue), `PENDING` (comment color), and metrics block at bottom

### 6. Sectors Section (`#sectors`, `.sec-mid`)

Eyebrow: `INDUSTRIES` · Title: `Where we operate.`

Grid: `grid-template-columns: repeat(2, 1fr); gap: 14px; margin-top: 44px`

Each `.sec-card`: `background: var(--zinc-800); border: 1px solid var(--zinc-700); border-radius: var(--radius-lg); padding: 28px 24px; display: flex; gap: 18px`
- Index: Space Mono `10px`, `color: var(--zinc-200)`, left column
- Name: `15px`, `font-weight: 500`, `color: var(--white)`
- Description: `13px`, `color: var(--zinc-200)`
- Hover: `border-color: var(--gold-border); background: #222226`

| # | Name | Description |
|---|------|-------------|
| 01 | Government & Public Sector | Permitting systems, spatial data platforms, open data infrastructure, and compliance-grade dashboards. |
| 02 | Financial Services | Portfolio analytics, NLP document intelligence, credit scoring, and risk dashboards for banks. |
| 03 | Healthcare & Nonprofits | Patient journey analytics, operational reporting automation, and research data pipelines. |
| 04 | Marketing & E-commerce | Conversion attribution, CLV modeling, and campaign personalization connecting data to revenue. |

### 7. Work / Case Studies (`#work`, `.sec-dark`)

Eyebrow: `SELECTED WORK` · Title: `Shipped. Measured. Real.`

Grid: `grid-template-columns: repeat(2, 1fr); gap: 14px; margin-top: 44px`

Each `.case-card`: `background: var(--zinc-900); border: 1px solid var(--zinc-800); border-radius: var(--radius-lg); padding: 32px 28px; position: relative; overflow: hidden`
- `::before` pseudo — left border reveal on hover: `position: absolute; top: 0; left: 0; width: 2px; height: 0; background: var(--gold); transition: height 0.3s ease` → hover: `height: 100%`
- Hover: `border-color: var(--zinc-300)`
- Sector label: Space Mono `10px`, `color: var(--zinc-300)`, uppercase
- Title: `17px`, `font-weight: 500`, `color: var(--white)`, `letter-spacing: -0.3px`
- Description: `13px`, `color: var(--zinc-200)`, `line-height: 1.7`
- Metrics row: `padding-top: 18px; border-top: 1px solid var(--zinc-800); display: flex; gap: 24px`
  - Value: Space Mono `20px`, `font-weight: 700`, `color: var(--gold-bright)`, `letter-spacing: -1px`
  - Label: `11px`, `color: var(--zinc-300)`

**Four case studies:**

| Sector | Title | Metrics |
|--------|-------|---------|
| Government · Bandung Regency | Building Permit Management System (SIBEDAS) | 45% faster delivery / 20% processing efficiency / 100% real-time access |
| Finance · Bank Muamalat Indonesia | NLP Portfolio Optimisation & Segmentation | 5.3% investment returns / 80% manual effort cut |
| Marketing · Syncwell UK | Predictive Campaign Optimisation Engine | 40% CTR lift / 22% follower growth |
| Healthcare · PCOS Challenge NPO | Patient Attribution & Automated Reporting | 80% workload reduced / 1 unified dashboard |

**Case study descriptions:**
- **SIBEDAS:** Replaced manual permitting with an automated digital platform — data pipelines, real-time compliance dashboards, and stakeholder portals on production VPS infrastructure.
- **Bank Muamalat:** NLP models for investment portfolio analysis, ML-powered market segmentation, and A/B testing infrastructure to iterate on marketing strategies at scale.
- **Syncwell:** ML-powered segmentation and automated campaign triggers with personalised journey orchestration driving measurable lifts in engagement and growth.
- **PCOS Challenge:** Attribution model and automated report extraction pipeline — replacing 80% of manual workload with an interactive dashboard surfacing patient financial insights.

### 8. Founder Section (`#team`, `.sec-mid`)

Eyebrow: `FOUNDER` · Title: `The person behind the work.`

**Layout:** `display: grid; grid-template-columns: 280px 1fr; gap: 48px; align-items: start; margin-top: 48px`

**Left column (`.founder-left`):**

Avatar (`.founder-avatar`):
```css
width: 120px; height: 120px; border-radius: 16px;
background: var(--zinc-800); border: 2px solid var(--gold-border);
font-family: 'Space Mono'; font-size: 32px; font-weight: 700; color: var(--gold);
/* ::after pseudo ring: position: absolute; inset: -4px; border-radius: 20px;
   border: 1px solid var(--gold-dim) */
```
- Initials text: `NZ`

Name: `20px`, `font-weight: 600`, `color: var(--white)`, `letter-spacing: -0.5px`, centered
Role: Space Mono, `11px`, `color: var(--gold)`, `letter-spacing: 1px`, uppercase, centered

Two education blocks (`.founder-edu`): `background: var(--zinc-800); border: 1px solid var(--zinc-700); border-radius: var(--radius); padding: 12px 16px; width: 100%`
- Label: Space Mono `9px`, `color: var(--zinc-600)`, uppercase, `letter-spacing: 1px`
- Value: `13px`, `color: var(--zinc-300)`
- Sub: `11px`, `color: var(--zinc-600)`

| Label | Value | Sub |
|-------|-------|-----|
| Education | MSc Business Analytics | Aston University, UK · 2024 · Cum Laude |
| Also | BBA Innovation & Economics | Ritsumeikan APU, Japan · 2019 |

LinkedIn button (`.founder-li`): `display: flex; align-items: center; gap: 8px; background: var(--gold-dim); border: 1px solid var(--gold-border); border-radius: var(--radius); padding: 10px 14px; text-decoration: none`
- Icon: Space Mono `11px`, `color: var(--gold)`, text `in`
- Text: Space Mono `12px`, `color: var(--gold)`, text `linkedin.com/in/nauval-zulfikar`
- href: `https://linkedin.com/in/nauval-zulfikar/`
- Hover: `background: rgba(245,197,66,0.2)`

**Right column (`.founder-right`):**

Stats row (`.founder-stats`): `display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-bottom: 32px`

Each `.f-stat`: `background: var(--zinc-800); border: 1px solid var(--zinc-700); border-radius: var(--radius); padding: 16px`
- Number: Space Mono `24px`, `font-weight: 700`, `color: var(--gold)`, `letter-spacing: -1px`
- Suffix (+ or empty): `color: var(--zinc-600)`, `font-size: 16px`
- Label: `11px`, `color: var(--zinc-600)`

| Number | Label |
|--------|-------|
| 5+ | years in production AI |
| 20+ | systems deployed |
| 4 | sectors served |

Bio paragraphs (`.founder-bio`): `font-size: 16px; line-height: 1.75; color: var(--zinc-400); font-weight: 300; margin-bottom: 32px`
- Use `<strong>` for key phrases → `color: var(--zinc-200); font-weight: 500`

**Paragraph 1:**
> I'm a Senior Data Scientist and automation engineer with **5+ years building end-to-end AI systems** across government, finance, healthcare, and marketing. I started Aureon to deliver the kind of work I'd always wanted to do — **production-grade, outcome-focused, and built to last**.

**Paragraph 2:**
> My edge is combining deep technical capability — ML, NLP, LLMs, ETL pipelines — with **real domain experience inside organizations**. I've sat inside a government department, a bank, and NPOs. I know what actually gets used versus what gets shelved.

Skills label: Space Mono `10px`, `color: var(--zinc-600)`, uppercase, `letter-spacing: 1.5px`, text `CORE STACK`

Skills grid (`.founder-skills`): `display: flex; flex-wrap: wrap; gap: 8px`

Each `.skill-tag`: Space Mono `11px`, `padding: 5px 12px`, `border-radius: 6px`, `border: 1px solid var(--zinc-700)`, `background: var(--zinc-800)`, `color: var(--zinc-400)`
- Hover: `border-color: var(--gold-border); color: var(--gold)`

Hot tags (`.skill-tag.hot`): `background: var(--gold-dim); border-color: var(--gold-border); color: var(--gold)`

| Hot | Normal |
|-----|--------|
| Python | SQL |
| Machine Learning | PySpark |
| NLP / LLM | FastAPI |
| RAG | Docker |
| | AWS |
| | PostgreSQL |
| | Power BI |
| | Streamlit |
| | Causal Inference |
| | A/B Testing |
| | Time Series |
| | Bayesian Stats |

### 9. CTA Section (`#contact`, `.cta-sec`)

```css
background: var(--zinc-950);
padding: 120px 5%;
text-align: center;
position: relative;
overflow: hidden;
border-top: 1px solid var(--zinc-800);
```

`::before` pseudo: `background: radial-gradient(ellipse 55% 55% at 50% 100%, rgba(245,197,66,0.1) 0%, transparent 70%)`

**Canvas:** `<canvas class="rain-canvas" id="cta-rain">` as first child (same rain animation as hero)

**Content (all `position: relative` to sit above canvas):**
- Mono label: `// ready to build?` (Space Mono, gold, uppercase, `letter-spacing: 2px`)
- Title: `Tell us the problem.<br><span>We'll ship the solution.</span>` — span in `var(--gold-bright)`
  - `font-size: clamp(34px, 5vw, 62px)`, `font-weight: 600`, `letter-spacing: -2px`
- Sub: `"No pitch decks. No discovery theater. A direct conversation about what needs to be built and how fast."`
- Two buttons: `Send an email` (primary, `href="mailto:zulfikar.nauval1998@gmail.com"`) and `LinkedIn →` (outline, `href="https://linkedin.com/in/nauval-zulfikar/"`, `target="_blank"`)
- Contact row (`display: flex; gap: 56px; justify-content: center; margin-top: 64px; padding-top: 44px; border-top: 1px solid var(--zinc-800); flex-wrap: wrap`):

| Label | Value |
|-------|-------|
| Email | zulfikar.nauval1998@gmail.com |
| Based in | Bandung, Indonesia |
| Work auth | ID · UK · NL · UAE |
| LinkedIn | linkedin.com/in/nauval-zulfikar |

### 10. Footer

```css
background: var(--zinc-950);
border-top: 1px solid var(--zinc-800);
padding: 24px 5%;
display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 10px;
```

- Logo: small gold dot (same as nav) + `AUREON` in Space Mono, `color: var(--zinc-200)`
- Copyright: Space Mono `10px`, `color: var(--zinc-200)`, text: `© 2025 AUREON — DATA, AI & AUTOMATION`

---

## Binary Rain Animation

This is the most critical interactive element. Implement as a `<script>` tag at the **bottom of `<body>`**, using vanilla Canvas 2D API.

### Canvas Setup

```html
<canvas class="rain-canvas" id="hero-rain"></canvas>  <!-- inside .hero -->
<canvas class="rain-canvas" id="cta-rain"></canvas>    <!-- inside .cta-sec -->
```

Canvas CSS:
```css
.rain-canvas {
  position: absolute; inset: 0;
  width: 100%; height: 100%;
  pointer-events: none;
  z-index: 0;
  opacity: 0.18;
}
```

### JavaScript Implementation

```javascript
function initRain(canvasId) {
  const canvas = document.getElementById(canvasId);
  if (!canvas) return;
  const ctx = canvas.getContext('2d');
  const section = canvas.parentElement;

  function resize() {
    canvas.width = section.offsetWidth;
    canvas.height = section.offsetHeight;
  }
  resize();
  window.addEventListener('resize', resize);

  // Character pool: binary + Japanese katakana + brand name
  const chars = '01アウレオン10データAI自動化0110100110機械学習パイプライン01AUREON10';
  const fontSize = 13;
  let cols = Math.floor(canvas.width / fontSize);
  let drops = Array(cols).fill(0).map(() => Math.random() * -80);

  function draw() {
    // Fade trail effect
    ctx.fillStyle = 'rgba(13,13,15,0.055)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    cols = Math.floor(canvas.width / fontSize);
    if (drops.length !== cols) {
      drops = Array(cols).fill(0).map((_, i) => drops[i] || Math.random() * -80);
    }

    for (let i = 0; i < cols; i++) {
      const char = chars[Math.floor(Math.random() * chars.length)];
      const y = drops[i] * fontSize;
      const progress = y / canvas.height;

      // Color gradient: dim at top → bright gold midway → fades at bottom
      if (progress > 0.7) {
        ctx.fillStyle = `rgba(245,197,66,${0.6 - (progress - 0.7) * 2})`;
      } else if (progress > 0.5) {
        ctx.fillStyle = `rgba(255,217,102,0.7)`;
      } else {
        ctx.fillStyle = `rgba(245,197,66,${0.25 + progress * 0.4})`;
      }

      ctx.font = `${fontSize}px "Space Mono", monospace`;
      ctx.fillText(char, i * fontSize, y);

      // Reset column when it reaches bottom
      if (y > canvas.height && Math.random() > 0.975) {
        drops[i] = 0;
      }
      drops[i] += 0.4 + Math.random() * 0.3;
    }
  }

  setInterval(draw, 45);  // ~22fps — intentionally not 60fps, feels more "digital"
}

initRain('hero-rain');
initRain('cta-rain');
```

**Key design decisions:**
- `opacity: 0.18` on canvas — subtle, doesn't compete with foreground content
- `rgba(13,13,15,0.055)` fade fill — long trails, slow decay
- Characters include Japanese katakana intentionally — references Nauval's Japan background (Ritsumeikan APU) and adds visual texture beyond generic Matrix binary
- Drop speed: `0.4 + Math.random() * 0.3` — varied, not uniform
- Column reset probability `0.975` — occasional short columns create depth

---

## Entrance Animations

Apply to hero elements only:

```css
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(18px); }
  to   { opacity: 1; transform: translateY(0); }
}

.fu { opacity: 0; animation: fadeUp 0.45s ease forwards; }
.d1 { animation-delay: 0.05s; }
.d2 { animation-delay: 0.15s; }
.d3 { animation-delay: 0.27s; }
.d4 { animation-delay: 0.4s; }
```

Apply to hero elements: eyebrow `.fu .d1`, H1 `.fu .d2`, subtext `.fu .d3`, buttons + terminal `.fu .d4`

---

## Responsive Breakpoints

```css
@media (max-width: 1100px) {
  .hero-terminal { display: none; }          /* hide terminal on tablet */
  .appr-grid { grid-template-columns: 1fr; } /* stack approach section */
}

@media (max-width: 800px) {
  .nav-links { display: none; }              /* hide nav links on mobile */
  .svc-grid { grid-template-columns: 1fr; }
  .stats-row { grid-template-columns: repeat(2, 1fr); }
  .sectors-grid, .cases-grid { grid-template-columns: 1fr; }
  .founder-wrap { grid-template-columns: 1fr; }
}
```

---

## Contact Information

Replace any placeholder contact info with:

- **Email:** `zulfikar.nauval1998@gmail.com`
- **LinkedIn:** `https://linkedin.com/in/nauval-zulfikar/`
- **Location:** Bandung, Indonesia
- **Work authorization:** ID · UK · NL · UAE

---

## Quality Checklist

Before considering the build complete, verify:

- [ ] Single `aureon.html` file — no external CSS or JS files
- [ ] Google Fonts loads (Space Grotesk + Space Mono)
- [ ] Binary rain animates on hero AND CTA sections
- [ ] Rain canvas does not block clicks on buttons
- [ ] Nav is fixed and stays above all content (`z-index: 100`)
- [ ] Hero content has `z-index: 1` so it sits above canvas layers
- [ ] Featured service card (01) has gold top border gradient
- [ ] Founder avatar has the double-ring CSS effect
- [ ] LinkedIn button in founder section opens in new tab
- [ ] Hover on case cards reveals the gold left-border slide animation
- [ ] Skill tags with `.hot` class render in gold
- [ ] All buttons have correct text color: gold buttons use `var(--gold-deep)` not white
- [ ] CTA title `<span>` is gold
- [ ] Ticker animation loops seamlessly (items duplicated)
- [ ] No horizontal scroll on any viewport width
- [ ] `<canvas>` elements have `pointer-events: none` so they don't intercept clicks
