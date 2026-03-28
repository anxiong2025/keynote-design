# Keynote Design

A Claude Code skill for generating professional, insight-driven HTML presentations — from a topic, reference material, or both.

## What This Does

**Keynote Design** generates Apple Keynote-quality HTML presentations with zero dependencies. Single HTML files with inline CSS/JS that open in any browser.

Focuses on **content quality** — every slide delivers a clear insight with evidence, not generic summaries.

### Key Features

- **Zero Dependencies** — Single HTML files. No npm, no build tools, no frameworks.
- **Content Quality First** — Every title is an opinion, every point has evidence, every slide earns its place.
- **11 Slide Types** — Title, Content, Quote, Two-Column, Diagram (5 styles), Stats, Hero Image, Code, Section, Closing
- **8 Visual Presets** — Midnight, Neon Signal, Deep Indigo, Dark Botanical, Clean White, Paper Minimal, Electric Gradient, Ocean Wave
- **Build Animations** — Step-by-step reveals, staggered entrances, Ken Burns, Magic Move (FLIP transitions)
- **Responsive** — clamp()-based typography, dvh viewport units, breakpoints from 500px to 1440px+

## Installation

```bash
npx skills add anxiong2025/keynote-design
```

When prompted:
1. Select **Claude Code** (or any other agent you use)
2. Choose **symlink** (recommended — updates automatically with `git pull`)

Done. Open Claude Code and start using it.

> Also works with Cursor, Windsurf, Copilot, and [40+ other agents](https://github.com/vercel-labs/skills).

<details>
<summary>Alternative: manual git clone</summary>

```bash
git clone https://github.com/anxiong2025/keynote-design.git ~/.claude/skills/keynote-design
```

Update anytime: `cd ~/.claude/skills/keynote-design && git pull`

</details>

## Usage

Open Claude Code in any project directory, then type:

```
/keynote-design "AI Agent Architecture"
```

Or generate from reference material:

```
/keynote-design from notes.md
```

Claude will:
1. Analyze your content and plan a narrative arc
2. Ask which visual preset matches your audience
3. Generate a complete self-contained HTML file
4. Open it in your browser

### Keyboard Shortcuts (in the generated presentation)

| Key | Action |
|-----|--------|
| `→` / `Space` | Next slide or build step |
| `←` | Previous slide |
| `F` | Fullscreen |
| `G` | Slide overview grid |
| `L` | Laser pointer |
| `P` | Speaker notes |
| `Esc` | Exit fullscreen |

## Visual Presets

| Preset | Vibe | Best For |
|--------|------|----------|
| Midnight | Dramatic, Apple-style, pure black | Keynotes, vision talks |
| Neon Signal | Bold, futuristic, high-energy | Startup pitches, demos |
| Deep Indigo | Thoughtful, technical, intellectual | Tech shares, deep dives |
| Dark Botanical | Elegant, warm, premium | Design reviews, brand talks |
| Clean White | Professional, clear, trustworthy | Business reports, formal |
| Paper Minimal | Refined, editorial, quiet | Academic, design critique |
| Electric Gradient | Energetic, creative, vivid | Product launches, creative |
| Ocean Wave | Technical but warm, modern | Engineering talks, dev conf |

## How It Works

```
keynote-design/
├── SKILL.md               # Core workflow, slide types, HTML template
├── style-presets.md        # 8 visual presets (fonts, colors, signatures)
├── viewport-base.css       # Responsive CSS (clamp, dvh, breakpoints)
├── animation-patterns.md   # Entrance/transition/background animations
├── .claude-plugin/
│   └── plugin.json         # Plugin metadata
└── README.md
```

**Progressive disclosure** — `SKILL.md` is a concise map; supporting files are loaded on-demand only when needed.

## Content Quality Philosophy

Great presentations are not summaries — they are arguments.

- **Titles are opinions, not labels.** "Skills Cut Deploy Time 60%" not "About Deployment"
- **Points need evidence.** Data, examples, comparisons — never "improves efficiency"
- **Narrative arc.** Problem → Context → Solution → Proof → Action
- **So What test.** Every point must answer why the audience should care

## Related

- [keynote-design-web](https://github.com/anxiong2025/keynote-design-web) — Web app version with visual editor and live presentation mode

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI

## License

MIT
