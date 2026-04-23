# Handoff: Slider Panel — Instrument Panel Design

## Overview
This is a redesigned slider input panel for the Borgarlínu reiknivél (Reykjavík BRT calculator) web app. It replaces the existing plain slider layout in the left sidebar (260px sticky panel) with a more structured, information-dense design that groups related parameters into labelled cards, each with icons and a colour-coded value badge.

## About the Design Files
The `reference.html` file in this folder is a **design reference built in HTML** — a working prototype showing the intended look and interaction behaviour. It is **not** production code to copy directly. Your task is to **recreate this design within the existing app's codebase**, matching its framework, component patterns, and state management conventions.

## Fidelity
**High-fidelity.** This is a pixel-precise mockup with final colours, typography, spacing, and hover interactions. Recreate it exactly, substituting the prototype's plain React/CSS for the app's existing component library or styling approach (e.g. Tailwind, CSS modules, styled-components).

---

## Component: `SliderPanel`

A 260px-wide sticky sidebar panel. Contains three **group cards**, each grouping related sliders.

### Panel shell

| Property | Value |
|---|---|
| Width | `260px` |
| Background | `#0b1120` |
| Border | `1px solid rgba(255,255,255,0.06)` |
| Border-radius | `12px` |
| Padding | `24px 20px` |
| Gap between group cards | `8px` |
| Layout | `flex, column` |

---

### Group card (`.d3-group`)

Three cards: **Demographics**, **Service design**, **Financials**.

| Property | Value |
|---|---|
| Background | `rgba(255,255,255,0.025)` |
| Border | `1px solid rgba(255,255,255,0.055)` |
| Border-radius | `8px` |
| Padding | `12px 14px` |
| Gap between rows | `14px` |
| Layout | `flex, column` |

**Group title**
- Font: Inter, 9px, weight 600
- Letter-spacing: `0.14em`
- Text-transform: uppercase
- Colour: `#334155`

---

### Slider row (`.d3-row`)

Each row contains: **header** (icon + label + value badge) → **range input** → **scale labels**.

Layout: `flex, column, gap: 7px`

#### Icon chip (`.d3-icon`)
| Property | Value |
|---|---|
| Size | `20×20px` |
| Border-radius | `5px` |
| Background | `rgba(99,102,241,0.12)` |
| Icon SVG size | `10×10px` |
| Icon stroke colour | `#818cf8` |
| Icon stroke-width | `1.2` |
| Icon stroke-linecap | `round` |

Icons per parameter (inline SVG paths — see `reference.html` for exact `d` values):

| Param | Icon concept |
|---|---|
| Population served | Two people silhouette |
| Urban density | Three ascending bars |
| Peak frequency | Waveform / ECG line |
| Daily span | Clock face |
| Fare level | Card/money rectangle |
| Public subsidy | Percent with diagonal |

#### Label (`.d3-label`)
- Font: Inter, 11px, weight 500
- Colour: `#94a3b8`

#### Value badge (`.d3-badge`)
- Font: JetBrains Mono, 11px, weight 500
- Padding: `2px 7px`
- Border-radius: `4px`
- Min-width: `42px`
- Text-align: centre
- Letter-spacing: `-0.02em`
- Transition: `background 0.1s`

Three states based on how far the slider is along its range:

| State | Condition | Background | Text colour | Border |
|---|---|---|---|---|
| Default (mid) | 25%–75% of range | `rgba(99,102,241,0.15)` | `#a5b4fc` | `1px solid rgba(99,102,241,0.2)` |
| Low | < 25% of range | `rgba(100,116,139,0.15)` | `#64748b` | `1px solid rgba(100,116,139,0.2)` |
| High | > 75% of range | `rgba(99,102,241,0.25)` | `#c7d2fe` | `1px solid rgba(99,102,241,0.4)` |

Unit label inside badge: 9px, `opacity: 0.6`, `margin-left: 2px`.

**Value formatting:**
- If value ≥ 10,000: display as `Xk` or `X.Xk` (e.g. `120k`, `4.2k`)
- Otherwise: `toLocaleString()` (e.g. `8`, `60`)

---

### Range input (`.d3-range`)

Custom-styled `<input type="range">`. The track fill is applied via an **inline `background` style** that updates on every change:

```
background: linear-gradient(
  90deg,
  rgba(99,102,241,0.35) {pct*100}%,
  rgba(255,255,255,0.07) 0%
)
```

where `pct = (value - min) / (max - min)`.

**Track**
| Property | Value |
|---|---|
| Height | `2px` |
| Base background | `rgba(255,255,255,0.07)` |
| Border-radius | `1px` |

**Thumb** (webkit + moz)
| Property | Value |
|---|---|
| Size | `14×14px` |
| Border-radius | `50%` |
| Background | `#0b1120` (same as panel bg — creates "hollow" effect) |
| Border | `2px solid #6366f1` |
| Box-shadow | `0 0 6px rgba(99,102,241,0.4)` |

**Thumb hover state**
| Property | Value |
|---|---|
| Border colour | `#a5b4fc` |
| Box-shadow | `0 0 10px rgba(99,102,241,0.6)` |
| Transition | `border-color 0.1s, box-shadow 0.1s` |

---

### Scale labels (`.d3-scale`)
- Layout: `flex, space-between`, padding `0 1px`
- Font: JetBrains Mono, 9px
- Colour: `#1e293b`
- Show `min` and `max` values; if ≥ 1000 display as `Xk`

---

## Slider parameters

| ID | Label | Min | Max | Step | Default | Unit |
|---|---|---|---|---|---|---|
| `pop` | Population served | 5,000 | 500,000 | 1,000 | 120,000 | inh. |
| `dens` | Urban density | 500 | 20,000 | 100 | 4,200 | /km² |
| `freq` | Peak frequency | 2 | 30 | 1 | 8 | min |
| `span` | Daily span | 12 | 24 | 1 | 18 | hrs |
| `fare` | Fare level | 200 | 1,000 | 50 | 450 | ISK |
| `subsidy` | Public subsidy | 0 | 100 | 5 | 60 | % |

Groups:
- **Demographics**: pop, dens
- **Service design**: freq, span
- **Financials**: fare, subsidy

---

## Design Tokens

### Colours
| Token | Value | Usage |
|---|---|---|
| `--bg-app` | `#080e1a` | Page background |
| `--bg-panel` | `#0b1120` | Panel background |
| `--bg-card` | `rgba(255,255,255,0.025)` | Group card background |
| `--border-subtle` | `rgba(255,255,255,0.055)` | Group card border |
| `--border-panel` | `rgba(255,255,255,0.06)` | Panel border |
| `--accent` | `#6366f1` | Primary accent (thumb border, fills) |
| `--accent-light` | `#818cf8` | Icon strokes |
| `--accent-lighter` | `#a5b4fc` | Badge text (default), thumb hover |
| `--accent-lightest` | `#c7d2fe` | Badge text (high state) |
| `--text-label` | `#94a3b8` | Slider label text |
| `--text-muted` | `#64748b` | Badge text (low state) |
| `--text-dim` | `#334155` | Group title, scale labels `#1e293b` |

### Typography
| Role | Font | Size | Weight | Notes |
|---|---|---|---|---|
| Group title | Inter | 9px | 600 | Uppercase, tracking 0.14em |
| Slider label | Inter | 11px | 500 | — |
| Value badge | JetBrains Mono | 11px | 500 | Tracking -0.02em |
| Scale labels | JetBrains Mono | 9px | 400 | — |

### Spacing
- Panel padding: `24px 20px`
- Group card padding: `12px 14px`
- Gap between groups: `8px`
- Gap between rows in a group: `14px`
- Gap within a row: `7px`
- Icon-to-label gap: `7px`

---

## State Management

The panel manages a single state object mapping slider IDs to their current numeric values. On every slider `onChange`, update the relevant key and re-render.

The badge class (`low` / `''` / `high`) and track fill gradient are both derived directly from the current value — no additional state needed.

Connect the values to the existing calculator's computation logic using whatever state-sharing pattern the app already uses (context, prop callbacks, store, etc.).

---

## Assets

No external images or icons. All icons are inline SVGs (10×10 viewBox, stroke-only). Exact paths are in `reference.html`.

## Files

| File | Purpose |
|---|---|
| `reference.html` | **Interactive design reference** — open in a browser to see and interact with the finished design |
| `README.md` | This document |
