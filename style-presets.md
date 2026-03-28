# Style Presets Reference

Each preset has a distinct personality: unique font pairing, color system, signature visual elements, and recommended animation feeling. **No two presets should feel alike.**

When generating, pick ONE preset and commit fully. Mix-and-match is allowed only when the user explicitly asks.

---

## Dark Themes

### 1. Midnight (Default — Apple Keynote Style)

**Vibe:** Dramatic, minimal, visionary — pure Apple energy
**Best for:** Product launches, keynotes, vision talks

**Typography:**
- Display: `SF Pro Display` fallback → `General Sans` (Fontshare, weight 600/700) — geometric, confident, Apple-adjacent without being generic
- Body: `SF Pro Text` fallback → `Satoshi` (Fontshare, weight 300/400/500) — clean humanist sans with subtle personality

**Colors:**
```css
:root {
    --bg: #000000;
    --text: #f5f5f7;
    --text-dim: rgba(255,255,255,0.45);
    --text-body: rgba(255,255,255,0.65);
    --accent: #2997ff;
    --accent2: #5e5ce6;
    --accent-rgb: 41,151,255;
    --accent2-rgb: 94,92,230;
    --border: rgba(255,255,255,0.08);
    --dot-bg: rgba(255,255,255,0.15);
}
```

**Signature Elements:**
- Gradient mesh glow behind title (subtle blue/purple orbs)
- Gradient text shimmer on hero titles
- Maximum whitespace — let elements breathe
- Noise texture overlay at 2-3% opacity

**Animation Feeling:** Dramatic — slow scale-in (buildScale), long durations (0.8-1.2s), ease-smooth

**Background:** `bg-mesh` + `bg-noise`

**Signature CSS:**
```css
/* Gradient mesh glow — blue/purple ambient orbs */
.slide::before {
    content: '';
    position: absolute; inset: 0;
    background:
        radial-gradient(ellipse 60% 50% at 20% 80%, rgba(41,151,255,0.12) 0%, transparent 70%),
        radial-gradient(ellipse 50% 60% at 80% 20%, rgba(94,92,230,0.08) 0%, transparent 70%),
        radial-gradient(ellipse 80% 80% at 50% 50%, rgba(41,151,255,0.04) 0%, transparent 60%);
    pointer-events: none; z-index: 0;
}
/* Noise texture */
.slide::after {
    content: '';
    position: absolute; inset: 0; opacity: 0.025;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    background-size: 256px 256px;
    pointer-events: none; z-index: 0;
}
.slide > *:not(.slide-num) { position: relative; z-index: 1; }
.slide-num { z-index: 1; }
```

---

### 2. Neon Signal

**Vibe:** Bold, high-energy, futuristic
**Best for:** Product demos, startup pitches, tech announcements

**Typography:**
- Display: `Clash Display` (Fontshare, weight 600/700) or `Space Grotesk` (Google, 700)
- Body: `Satoshi` (Fontshare, 400/500) or `Space Grotesk` (Google, 400)

**Colors:**
```css
:root {
    --bg: #0a0a0f;
    --text: #ffffff;
    --text-dim: rgba(255,255,255,0.5);
    --text-body: rgba(255,255,255,0.7);
    --accent: #00ffcc;
    --accent2: #ff3366;
    --accent-rgb: 0,255,204;
    --accent2-rgb: 255,51,102;
    --border: rgba(0,255,204,0.1);
    --dot-bg: rgba(255,255,255,0.15);
}
```

**Signature Elements:**
- Neon glow on accent elements: `box-shadow: 0 0 30px rgba(0,255,204,0.2)`
- Grid pattern background
- Monospace accents for labels/numbers
- Accent-colored progress bar glow

**Animation Feeling:** Techy — blur-in (buildBlur), medium speed (0.5-0.7s), ease-out-expo

**Background:** `bg-grid` + neon glow accents

**Signature CSS:**
```css
/* Grid pattern background */
.slide::before {
    content: '';
    position: absolute; inset: 0;
    background-image:
        linear-gradient(rgba(0,255,204,0.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,255,204,0.04) 1px, transparent 1px);
    background-size: clamp(30px, 4vw, 60px) clamp(30px, 4vw, 60px);
    pointer-events: none; z-index: 0;
}
/* Neon glow on accent elements */
.eyebrow { text-shadow: 0 0 20px rgba(0,255,204,0.3); }
.accent-glow { box-shadow: 0 0 30px rgba(0,255,204,0.2), 0 0 60px rgba(0,255,204,0.1); }
.stat-number { text-shadow: 0 0 40px rgba(0,255,204,0.25); }
/* Scanline overlay (optional) */
.slide::after {
    content: '';
    position: absolute; inset: 0;
    background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,255,204,0.008) 2px, rgba(0,255,204,0.008) 4px);
    pointer-events: none; z-index: 0;
}
.slide > *:not(.slide-num) { position: relative; z-index: 1; }
.slide-num { z-index: 1; }
/* Override default build animation to blur-in */
.slide [data-build].revealed { animation-name: buildBlur; animation-duration: 0.6s; animation-timing-function: var(--ease-out-expo); }
```

---

### 3. Deep Indigo

**Vibe:** Thoughtful, technical, intellectual
**Best for:** Tech shares, architecture talks, deep dives

**Typography:**
- Display: `Syne` (Google Fonts, weight 700/800)
- Body: `IBM Plex Sans` (Google Fonts, weight 300/400)

**Colors:**
```css
:root {
    --bg: linear-gradient(155deg, #080820 0%, #0e1040 40%, #1a1a60 70%, #0c0c30 100%);
    --text: #d8d8f0;
    --text-dim: rgba(216,216,240,0.5);
    --text-body: rgba(216,216,240,0.7);
    --accent: #a78bfa;
    --accent2: #c084fc;
    --accent-rgb: 167,139,250;
    --accent2-rgb: 192,132,252;
    --border: rgba(167,139,250,0.1);
    --dot-bg: rgba(255,255,255,0.12);
}
```

**Signature Elements:**
- Soft purple glow orbs floating in background
- Code-style monospace highlights inline
- Layered card borders with gradient

**Animation Feeling:** Professional — clean fade-up (buildFadeUp), snappy (0.4-0.6s), ease-out-expo

**Background:** `bg-mesh` (purple tones) + `bg-noise`

**Signature CSS:**
```css
/* Purple gradient mesh */
.slide::before {
    content: '';
    position: absolute; inset: 0;
    background:
        radial-gradient(ellipse 50% 50% at 25% 75%, rgba(167,139,250,0.12) 0%, transparent 65%),
        radial-gradient(ellipse 60% 40% at 75% 25%, rgba(192,132,252,0.08) 0%, transparent 65%);
    pointer-events: none; z-index: 0;
}
/* Noise */
.slide::after {
    content: '';
    position: absolute; inset: 0; opacity: 0.025;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    background-size: 256px 256px;
    pointer-events: none; z-index: 0;
}
.slide > *:not(.slide-num) { position: relative; z-index: 1; }
.slide-num { z-index: 1; }
/* Code-style monospace accent for labels */
.eyebrow { font-family: 'JetBrains Mono', 'SF Mono', monospace; letter-spacing: 0.2em; }
/* Snappy fade-up override */
.slide [data-build].revealed { animation-name: buildFadeUp; animation-duration: 0.5s; animation-timing-function: var(--ease-out-expo); }
```

---

### 4. Dark Botanical

**Vibe:** Elegant, warm, premium, artistic
**Best for:** Design reviews, brand presentations, creative pitches

**Typography:**
- Display: `Cormorant Garamond` (Google Fonts, weight 500/600) — elegant serif
- Body: `IBM Plex Sans` (Google Fonts, weight 300/400)

**Colors:**
```css
:root {
    --bg: #0f0f0f;
    --text: #e8e4df;
    --text-dim: rgba(232,228,223,0.45);
    --text-body: rgba(232,228,223,0.65);
    --accent: #d4a574;
    --accent2: #e8b4b8;
    --accent-rgb: 212,165,116;
    --accent2-rgb: 232,180,184;
    --border: rgba(212,165,116,0.1);
    --dot-bg: rgba(255,255,255,0.1);
}
```

**Signature Elements:**
- Abstract soft gradient circles (blurred, overlapping warm tones)
- Thin vertical accent line dividers
- Italic serif for quotes and emphasis
- Warm noise overlay

**Animation Feeling:** Calm — gentle fade + scale (buildScale), slow (0.8-1s), ease-smooth

**Background:** Warm gradient orbs + `bg-noise`

**Signature CSS:**
```css
/* Warm gradient orbs — abstract botanical shapes */
.slide::before {
    content: '';
    position: absolute; inset: 0;
    background:
        radial-gradient(ellipse 40% 50% at 80% 70%, rgba(212,165,116,0.12) 0%, transparent 60%),
        radial-gradient(ellipse 35% 45% at 15% 30%, rgba(232,180,184,0.1) 0%, transparent 55%),
        radial-gradient(ellipse 30% 30% at 60% 20%, rgba(201,184,150,0.06) 0%, transparent 50%);
    pointer-events: none; z-index: 0;
}
/* Warm noise */
.slide::after {
    content: '';
    position: absolute; inset: 0; opacity: 0.03;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    background-size: 256px 256px;
    pointer-events: none; z-index: 0;
}
.slide > *:not(.slide-num) { position: relative; z-index: 1; }
.slide-num { z-index: 1; }
/* Serif italic for quotes */
.quote-text { font-style: italic; }
/* Thin vertical accent lines as decorative dividers */
.col:first-child::after {
    content: '';
    position: absolute; right: 0; top: 20%; bottom: 20%;
    width: 1px;
    background: linear-gradient(to bottom, transparent, rgba(212,165,116,0.3), transparent);
}
/* Slow, calm animation */
.slide [data-build].revealed { animation-name: buildScale; animation-duration: 1s; animation-timing-function: var(--ease-smooth); }
```

---

## Light Themes

### 5. Clean White

**Vibe:** Professional, clear, trustworthy
**Best for:** Business reports, investor decks, formal presentations

**Typography:**
- Display: `DM Sans` (Google Fonts, weight 700/800) — geometric precision, more character than system fonts
- Body: `Source Sans 3` (Google Fonts, weight 400/500) — humanist, optimized for long reading

**Colors:**
```css
:root {
    --bg: #ffffff;
    --text: #1d1d1f;
    --text-dim: #86868b;
    --text-body: #424245;
    --accent: #007aff;
    --accent2: #5856d6;
    --accent-rgb: 0,122,255;
    --accent2-rgb: 88,86,214;
    --border: rgba(0,0,0,0.06);
    --dot-bg: rgba(0,0,0,0.1);
}
```

**Signature Elements:**
- Sharp card shadows for content sections
- Blue accent underlines on key terms
- Clean horizontal rules between sections
- Minimal — no background effects

**Animation Feeling:** Professional — fast fade-up (0.3-0.5s), ease-out-expo

**Background:** Solid white, optional subtle gray gradient at edges

**Signature CSS:**
```css
/* Clean white — no bg effects, just subtle edge vignette */
.slide::before {
    content: '';
    position: absolute; inset: 0;
    background: radial-gradient(ellipse 70% 70% at 50% 50%, transparent 60%, rgba(0,0,0,0.02) 100%);
    pointer-events: none; z-index: 0;
}
.slide > *:not(.slide-num) { position: relative; z-index: 1; }
.slide-num { z-index: 1; }
/* Blue accent underlines */
.title { border-bottom: 2px solid rgba(0,122,255,0.15); padding-bottom: clamp(0.5rem, 1vh, 0.75rem); display: inline-block; }
.slide[data-type="title"] .title,
.slide[data-type="closing"] .title,
.slide[data-type="section"] .title { border-bottom: none; display: block; }
/* Card shadows for content */
.code-block, .stats-grid { box-shadow: 0 2px 20px rgba(0,0,0,0.06); border-radius: 12px; padding: clamp(1rem, 2vw, 2rem); }
/* Fast precise animation */
.slide [data-build].revealed { animation-name: buildFadeUp; animation-duration: 0.35s; animation-timing-function: var(--ease-out-expo); }
```

---

### 6. Paper Minimal

**Vibe:** Formal, quiet, refined
**Best for:** Academic talks, design critiques, editorial presentations

**Typography:**
- Display: `Fraunces` (Google Fonts, weight 700/900) — distinctive serif
- Body: `Work Sans` (Google Fonts, weight 400/500)

**Colors:**
```css
:root {
    --bg: #f5f5f7;
    --text: #1d1d1f;
    --text-dim: #86868b;
    --text-body: #515154;
    --accent: #1d1d1f;
    --accent2: #6e6e73;
    --accent-rgb: 29,29,31;
    --accent2-rgb: 110,110,115;
    --border: rgba(0,0,0,0.06);
    --dot-bg: rgba(0,0,0,0.08);
}
```

**Signature Elements:**
- Abstract geometric shapes (circle outline + line + dot)
- Drop caps on first paragraph
- Pull-quote style large text
- Warm cream tint

**Animation Feeling:** Editorial — staggered text reveal, slow (0.6-1s), ease-dramatic

**Background:** Solid off-white + `bg-noise` at 2%

**Signature CSS:**
```css
/* Warm paper noise */
.slide::after {
    content: '';
    position: absolute; inset: 0; opacity: 0.02;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    background-size: 256px 256px;
    pointer-events: none; z-index: 0;
}
.slide > *:not(.slide-num) { position: relative; z-index: 1; }
.slide-num { z-index: 1; }
/* Geometric decorative shapes */
.slide[data-type="title"]::before,
.slide[data-type="section"]::before {
    content: '';
    position: absolute;
    top: 15%; right: 12%;
    width: clamp(60px, 10vw, 120px); height: clamp(60px, 10vw, 120px);
    border: 2px solid rgba(29,29,31,0.08);
    border-radius: 50%;
    pointer-events: none; z-index: 0;
}
/* Drop cap effect on body text */
.body::first-letter {
    font-family: var(--font-display);
    font-size: 2.5em;
    float: left;
    line-height: 0.8;
    margin-right: 0.1em;
    color: var(--text);
}
/* Editorial stagger */
.slide [data-build].revealed { animation-name: buildFadeUp; animation-duration: 0.8s; animation-timing-function: cubic-bezier(0.0, 0.0, 0.2, 1); }
```

---

## Vivid Themes

### 7. Electric Gradient

**Vibe:** Energetic, creative, modern
**Best for:** Product launches, creative pitches, startup demos

**Typography:**
- Display: `Cabinet Grotesk` (Fontshare, weight 700/800) — bold geometric with personality, energetic character
- Body: `Outfit` (Google Fonts, weight 400/500) — rounded, friendly, complements the boldness

**Colors:**
```css
:root {
    --bg: linear-gradient(135deg, #1a1a2e 0%, #3d1552 30%, #6b2fa0 60%, #2d5aa0 100%);
    --text: #f0eef5;
    --text-dim: rgba(255,255,255,0.55);
    --text-body: rgba(255,255,255,0.7);
    --accent: #ffb86c;
    --accent2: #ff79c6;
    --accent-rgb: 255,184,108;
    --accent2-rgb: 255,121,198;
    --border: rgba(255,255,255,0.1);
    --dot-bg: rgba(255,255,255,0.15);
}
```

**Signature Elements:**
- Animated gradient background (slow shift)
- Orange/pink accent highlights against purple
- Rounded pill badges for tags/labels
- Glow behind cards

**Animation Feeling:** Playful — spring pop (buildSpring), medium (0.5-0.8s), ease-spring

**Background:** Animated gradient + `bg-noise`

**Signature CSS:**
```css
/* Animated gradient background */
.slide {
    background: linear-gradient(135deg, #1a1a2e 0%, #3d1552 30%, #6b2fa0 60%, #2d5aa0 100%);
    background-size: 300% 300%;
    animation: gradientBG 15s ease infinite;
}
@keyframes gradientBG {
    0%   { background-position: 0% 50%; }
    50%  { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
}
/* Noise overlay */
.slide::after {
    content: '';
    position: absolute; inset: 0; opacity: 0.03;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    background-size: 256px 256px;
    pointer-events: none; z-index: 0;
}
.slide > *:not(.slide-num) { position: relative; z-index: 1; }
.slide-num { z-index: 1; }
/* Pill badges */
.eyebrow {
    background: rgba(255,184,108,0.15);
    padding: 4px 14px;
    border-radius: 100px;
    display: inline-block;
    border: 1px solid rgba(255,184,108,0.2);
}
/* Card glow */
.code-block, .diagram-layer, .flow-node {
    box-shadow: 0 4px 30px rgba(255,184,108,0.08);
}
/* Spring animation override */
.slide [data-build].revealed { animation-name: buildSpring; animation-duration: 0.7s; animation-timing-function: var(--ease-spring); }
```

---

### 8. Ocean Wave

**Vibe:** Technical but warm, engineering-focused, modern
**Best for:** Engineering talks, platform launches, developer conferences

**Typography:**
- Display: `Manrope` (Google Fonts, weight 700/800)
- Body: `Manrope` (Google Fonts, weight 400/500)

**Colors:**
```css
:root {
    --bg: linear-gradient(155deg, #0d0d1a 0%, #1a1a2e 30%, #2d1b4e 60%, #16213e 100%);
    --text: #e0dff0;
    --text-dim: rgba(224,223,240,0.5);
    --text-body: rgba(224,223,240,0.7);
    --accent: #06b6d4;
    --accent2: #8b5cf6;
    --accent-rgb: 6,182,212;
    --accent2-rgb: 139,92,246;
    --border: rgba(6,182,212,0.1);
    --dot-bg: rgba(255,255,255,0.12);
}
```

**Signature Elements:**
- Cyan/purple dual accent system
- Subtle wave-like gradient shifts
- Terminal-style monospace for code/data
- Thin accent borders on cards

**Animation Feeling:** Techy — blur-in + slide (0.5-0.7s), ease-out-expo

**Background:** `bg-mesh` (cyan/purple) + `bg-grid` (very subtle)

**Signature CSS:**
```css
/* Cyan/purple gradient mesh */
.slide::before {
    content: '';
    position: absolute; inset: 0;
    background:
        radial-gradient(ellipse 55% 50% at 30% 70%, rgba(6,182,212,0.1) 0%, transparent 65%),
        radial-gradient(ellipse 50% 55% at 70% 30%, rgba(139,92,246,0.08) 0%, transparent 65%);
    pointer-events: none; z-index: 0;
}
/* Subtle grid overlay */
.slide::after {
    content: '';
    position: absolute; inset: 0;
    background-image:
        linear-gradient(rgba(6,182,212,0.025) 1px, transparent 1px),
        linear-gradient(90deg, rgba(6,182,212,0.025) 1px, transparent 1px);
    background-size: clamp(40px, 5vw, 70px) clamp(40px, 5vw, 70px);
    pointer-events: none; z-index: 0;
}
.slide > *:not(.slide-num) { position: relative; z-index: 1; }
.slide-num { z-index: 1; }
/* Terminal monospace accent for code-related text */
.code-block { border-color: rgba(6,182,212,0.2); background: rgba(6,182,212,0.04); }
/* Blur-in animation */
.slide [data-build].revealed { animation-name: buildBlur; animation-duration: 0.6s; animation-timing-function: var(--ease-out-expo); }
```

---

## DO NOT USE (Anti-AI-Slop Rules)

**Fonts:**
- NEVER use Inter, Roboto, or Arial as display fonts — they scream "default"
- NEVER use the same font for display and body unless the preset explicitly pairs them (e.g., Outfit)
- Each preset's display font MUST be visually distinctive at 4rem+. If two presets look the same in a title, one needs a different font.

**Colors:**
- No `#6366f1` generic indigo. No purple-gradient-on-white cliche. No rainbow gradients.
- ALWAYS assign unique `bgGlow` to every slide. A presentation where all slides have the same background atmosphere is not Apple-quality.

**Layouts:** Not everything centered. Not every slide the same template. Break the grid occasionally.

**Animations:** No gratuitous spinning or bouncing. No animation for animation's sake. Every motion must serve the narrative.

**Visual rhythm:** Alternate dense and breathing slides. Use hero-image or section slides as "palate cleansers" between content-heavy slides.

**General:** Each generation should feel distinct. If you catch yourself defaulting to the same font/color combo, stop and pick something different.

---

## Font Loading

Always load fonts in `<head>`. Examples:

```html
<!-- Google Fonts example -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@700;800&family=Source+Sans+3:wght@400;500&display=swap" rel="stylesheet">

<!-- Fontshare example (Midnight preset) -->
<link href="https://api.fontshare.com/v2/css?f[]=general-sans@600,700&f[]=satoshi@300,400,500&display=swap" rel="stylesheet">

<!-- Fontshare example (Neon Signal preset) -->
<link href="https://api.fontshare.com/v2/css?f[]=clash-display@600,700&f[]=satoshi@400,500&display=swap" rel="stylesheet">

<!-- Fontshare example (Electric Gradient preset) -->
<link href="https://api.fontshare.com/v2/css?f[]=cabinet-grotesk@700,800&display=swap" rel="stylesheet">
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;500&display=swap" rel="stylesheet">
```

Set `--font-display` and `--font-body` in `:root` to match the chosen preset's fonts.
