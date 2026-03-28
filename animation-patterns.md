# Animation Patterns Reference

Load this file when generating presentations. Match animations to the intended feeling.

## Easing Curves

```css
/* Standard curves — use these via CSS variables */
--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);     /* Smooth deceleration, default for most */
--ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);    /* Slight overshoot, playful/energetic */
--ease-smooth: cubic-bezier(0.22, 1, 0.36, 1);       /* Apple-style smooth ease */
--ease-dramatic: cubic-bezier(0.0, 0.0, 0.2, 1);     /* Slow start, dramatic entrance */
```

## Feeling-to-Animation Guide

| Feeling | Entrance | Duration | Easing | Background | Signature |
|---------|----------|----------|--------|------------|-----------|
| **Dramatic / Apple** | Fade + scale (0.92→1) | 0.8–1.2s | ease-smooth | Gradient mesh + glow orbs | Slow, confident, large scale |
| **Techy / Futuristic** | Blur-in + slide-up | 0.5–0.7s | ease-out-expo | Grid pattern + scan lines | Neon glow, glitch text |
| **Playful / Friendly** | Bounce-in (spring) | 0.5–0.8s | ease-spring | Soft gradient | Overshoot, wobble |
| **Professional** | Clean fade + slide-up | 0.3–0.5s | ease-out-expo | Subtle noise | Fast, precise |
| **Calm / Minimal** | Gentle fade only | 0.8–1.2s | ease-smooth | Solid or soft gradient | Very slow, understated |
| **Editorial** | Staggered text reveal | 0.6–1s | ease-dramatic | Paper texture | Strong hierarchy |

---

## Entrance Animations

### Core: Staggered Build System (Non-Uniform)

The build system reveals elements step-by-step. Each `[data-build]` element gets a build step number. Within each step, child elements stagger with **ease-out timing** — fast start, gradual deceleration — NOT mechanical equal intervals.

```css
/* ── Base build state (hidden) ── */
.slide [data-build] {
    opacity: 0;
    transform: translateY(24px);
    transition: none;
}

/* ── Revealed state ── */
.slide [data-build].revealed {
    animation: buildScale var(--duration-slow) var(--ease-smooth) both;
}

/* ── Stagger: parent becomes visible instantly, children animate individually ── */
.slide [data-build].revealed.stagger {
    animation: none;
    opacity: 1;
    transform: none;
}
.slide [data-build].revealed.stagger > * {
    opacity: 0;
    transform: translateY(18px);
    animation: staggerItem 0.5s var(--ease-smooth) both;
}
@keyframes staggerItem {
    from { opacity: 0; transform: translateY(18px); }
    to   { opacity: 1; transform: translateY(0); }
}
/* Non-uniform stagger: ease-out curve — fast start, gentle deceleration */
.slide [data-build].revealed.stagger > *:nth-child(1) { animation-delay: 0.04s; }
.slide [data-build].revealed.stagger > *:nth-child(2) { animation-delay: 0.10s; }
.slide [data-build].revealed.stagger > *:nth-child(3) { animation-delay: 0.18s; }
.slide [data-build].revealed.stagger > *:nth-child(4) { animation-delay: 0.28s; }
.slide [data-build].revealed.stagger > *:nth-child(5) { animation-delay: 0.39s; }
.slide [data-build].revealed.stagger > *:nth-child(6) { animation-delay: 0.50s; }
.slide [data-build].revealed.stagger > *:nth-child(7) { animation-delay: 0.62s; }
.slide [data-build].revealed.stagger > *:nth-child(8) { animation-delay: 0.74s; }

/* ── Nested non-uniform stagger ── */
.slide [data-build].revealed .stagger > * {
    opacity: 0;
    transform: translateY(18px);
    animation: staggerItem 0.5s var(--ease-smooth) both;
}
.slide [data-build].revealed .stagger > *:nth-child(1) { animation-delay: 0.04s; }
.slide [data-build].revealed .stagger > *:nth-child(2) { animation-delay: 0.10s; }
.slide [data-build].revealed .stagger > *:nth-child(3) { animation-delay: 0.18s; }
.slide [data-build].revealed .stagger > *:nth-child(4) { animation-delay: 0.28s; }
.slide [data-build].revealed .stagger > *:nth-child(5) { animation-delay: 0.39s; }
.slide [data-build].revealed .stagger > *:nth-child(6) { animation-delay: 0.50s; }
```

### Entrance Keyframes

```css
/* Default: Fade + slide up */
@keyframes buildFadeUp {
    from { opacity: 0; transform: translateY(24px); }
    to   { opacity: 1; transform: translateY(0); }
}

/* Scale in — for titles, hero elements */
@keyframes buildScale {
    from { opacity: 0; transform: scale(0.92); }
    to   { opacity: 1; transform: scale(1); }
}

/* Blur in — techy, futuristic feel */
@keyframes buildBlur {
    from { opacity: 0; filter: blur(12px); transform: translateY(8px); }
    to   { opacity: 1; filter: blur(0);    transform: translateY(0); }
}

/* Slide from left — for columns, asymmetric layouts */
@keyframes buildSlideLeft {
    from { opacity: 0; transform: translateX(-40px); }
    to   { opacity: 1; transform: translateX(0); }
}

/* Slide from right */
@keyframes buildSlideRight {
    from { opacity: 0; transform: translateX(40px); }
    to   { opacity: 1; transform: translateX(0); }
}

/* Spring pop — playful */
@keyframes buildSpring {
    0%   { opacity: 0; transform: scale(0.6); }
    60%  { opacity: 1; transform: scale(1.08); }
    80%  { transform: scale(0.97); }
    100% { opacity: 1; transform: scale(1); }
}

/* Drop in — dramatic, Apple-style */
@keyframes buildDrop {
    0%   { opacity: 0; transform: translateY(-60px) scale(0.95); }
    60%  { opacity: 1; transform: translateY(4px) scale(1.01); }
    100% { opacity: 1; transform: translateY(0) scale(1); }
}

/* Counter / number roll — for stats slides */
@keyframes countUp {
    from { opacity: 0; transform: translateY(100%); }
    to   { opacity: 1; transform: translateY(0); }
}
```

### Per-Type Entrance Mapping

Apply these CSS classes based on slide type for best effect:

```css
/* Titles get scale-in */
.slide [data-build].revealed .title        { animation-name: buildScale; }
.slide [data-build].revealed .gradient-text { animation-name: buildScale; animation-duration: var(--duration-slow); }

/* Columns slide in from sides */
.slide .col[data-build].revealed             { animation-name: buildSlideLeft; }
.slide .col:last-child[data-build].revealed  { animation-name: buildSlideRight; }

/* Quotes scale in dramatically */
.slide .quote-text[data-build].revealed { animation-name: buildScale; animation-duration: 0.8s; }

/* Stats counter pop */
.slide .stat-number[data-build].revealed { animation-name: buildDrop; animation-duration: 0.8s; }

/* Code block blurs in */
.slide .code-block[data-build].revealed { animation-name: buildBlur; animation-duration: 0.7s; }
```

---

## Slide Transitions

Apply to the incoming `.slide.active` element:

```css
/* Fade (default) */
@keyframes transFade {
    from { opacity: 0; }
    to   { opacity: 1; }
}

/* Push — slide in from right */
@keyframes transPush {
    from { opacity: 0; transform: translateX(100%); }
    to   { opacity: 1; transform: translateX(0); }
}

/* Push back — slide in from left (going backwards) */
@keyframes transPushBack {
    from { opacity: 0; transform: translateX(-100%); }
    to   { opacity: 1; transform: translateX(0); }
}

/* Zoom — scale down into view */
@keyframes transZoom {
    from { opacity: 0; transform: scale(1.15); }
    to   { opacity: 1; transform: scale(1); }
}

/* Dissolve — blur transition */
@keyframes transDissolve {
    from { opacity: 0; filter: blur(16px); }
    to   { opacity: 1; filter: blur(0); }
}

/* Morph — subtle scale + fade, Apple-like */
@keyframes transMorph {
    from { opacity: 0; transform: scale(0.97) translateY(10px); }
    to   { opacity: 1; transform: scale(1) translateY(0); }
}
```

```css
.slide.trans-fade     { animation: transFade 0.6s var(--ease-smooth) forwards; }
.slide.trans-push     { animation: transPush 0.5s var(--ease-smooth) forwards; }
.slide.trans-push-back{ animation: transPushBack 0.5s var(--ease-smooth) forwards; }
.slide.trans-zoom     { animation: transZoom 0.5s var(--ease-smooth) forwards; }
.slide.trans-dissolve { animation: transDissolve 0.6s var(--ease-smooth) forwards; }
.slide.trans-morph    { animation: transMorph 0.6s var(--ease-smooth) forwards; }
```

### Exit Animations

Applied to the outgoing slide before the incoming slide appears. Creates a seamless crossfade effect.

```css
/* Exit forward — when advancing (scale up slightly + fade out) */
@keyframes exitFade {
    from { opacity: 1; transform: scale(1) translateY(0); }
    to   { opacity: 0; transform: scale(1.03) translateY(-8px); }
}
/* Exit backward — when retreating (scale down slightly + fade out) */
@keyframes exitFadeBack {
    from { opacity: 1; transform: scale(1) translateY(0); }
    to   { opacity: 0; transform: scale(0.97) translateY(8px); }
}
.slide.exit-forward  { animation: exitFade 0.4s var(--ease-smooth) forwards; pointer-events: none; }
.slide.exit-backward { animation: exitFadeBack 0.4s var(--ease-smooth) forwards; pointer-events: none; }
```

**JS integration:** In `showSlide()`, apply the exit class to the outgoing slide, wait for the exit duration (400ms), then remove it and show the incoming slide with its entrance animation.

---

## Ken Burns Effect (Hero Image)

Applied automatically to `.hero-bg` elements. Three variants:

```css
/* Default: slow zoom-in + slight pan */
@keyframes kenBurns {
    0%   { transform: scale(1) translate(0, 0); }
    100% { transform: scale(1.08) translate(-1.5%, -1%); }
}
/* Zoom out: starts zoomed, pulls back */
@keyframes kenBurnsZoomOut {
    0%   { transform: scale(1.12) translate(-2%, -1.5%); }
    100% { transform: scale(1) translate(0, 0); }
}
/* Pan right: horizontal drift */
@keyframes kenBurnsPanRight {
    0%   { transform: scale(1.06) translate(2%, 0); }
    100% { transform: scale(1.06) translate(-2%, -0.5%); }
}
```

Apply via CSS class: `.hero-bg` (default), `.hero-bg.kb-zoom-out`, `.hero-bg.kb-pan-right`. Duration: 14s. The `.hero-bg` uses `inset: -5%` + `110%` size to prevent edge exposure during animation.

---

## Magic Move (FLIP Transition)

When two consecutive slides share `data-morph-id` attributes on matching elements, the engine performs a FLIP (First-Last-Invert-Play) animation:

1. **First**: Record positions of morph elements on outgoing slide
2. **Last**: Show incoming slide, measure new positions
3. **Invert**: Apply transform to make elements appear in their old position
4. **Play**: Animate transform to 0 (natural position)

Duration: 0.65s with `ease-smooth`. Crossfade between slides runs in parallel.

**Usage in slide data:**
```javascript
{ type: "section", title: "My Topic", morphId: "topic" },
{ type: "content", title: "My Topic Details", morphId: "topic" }
// The eyebrow and title will morph smoothly between positions
```

Elements get `data-morph-id="${morphId}-ey"`, `data-morph-id="${morphId}-t"`, `data-morph-id="${morphId}-sub"` for eyebrow, title, subtitle respectively.

---

## Per-Slide Background Glow

Each slide can define `--glow-color` and `--glow-color2` CSS custom properties (RGB triplets). The gradient mesh `::before` pseudo-element uses these variables:

```css
.slide::before {
    background:
        radial-gradient(ellipse 60% 50% at 20% 80%, rgba(var(--glow-color), 0.14) 0%, transparent 70%),
        radial-gradient(ellipse 50% 60% at 80% 20%, rgba(var(--glow-color2), 0.10) 0%, transparent 70%),
        radial-gradient(ellipse 80% 80% at 50% 50%, rgba(var(--glow-color), 0.05) 0%, transparent 60%);
}
```

**Strategy**: Assign every slide a unique bgGlow. Create a gradual color arc through the presentation — e.g., blue → purple → warm → green → blue. This creates the atmosphere progression that defines Apple's visual storytelling.

---

## Background Effects

### Gradient Mesh (Ambient Light / Apple-style Glow)

```css
/* Layered radial gradients for organic depth (uses per-slide --glow-color vars) */
.bg-mesh::before {
    content: '';
    position: absolute;
    inset: 0;
    background:
        radial-gradient(ellipse 60% 50% at 20% 80%, rgba(var(--glow-color), 0.14) 0%, transparent 70%),
        radial-gradient(ellipse 50% 60% at 80% 20%, rgba(var(--glow-color2), 0.10) 0%, transparent 70%),
        radial-gradient(ellipse 80% 80% at 50% 50%, rgba(var(--glow-color), 0.05) 0%, transparent 60%);
    pointer-events: none;
    z-index: 0;
}

/* Animated glow orbs — subtle floating lights */
.bg-glow-orb {
    position: absolute;
    border-radius: 50%;
    filter: blur(80px);
    opacity: 0.15;
    animation: orbFloat 12s ease-in-out infinite alternate;
    pointer-events: none;
}
@keyframes orbFloat {
    0%   { transform: translate(0, 0) scale(1); }
    33%  { transform: translate(30px, -20px) scale(1.1); }
    66%  { transform: translate(-20px, 15px) scale(0.95); }
    100% { transform: translate(10px, -10px) scale(1.05); }
}
```

### Floating Orbs (Ambient Background)

Large, blurred, slowly drifting orbs that use the theme's accent colors. They float behind all slide content and create a living, organic feel. Three orbs with different sizes, positions, and drift speeds. CSS is defined in `viewport-base.css` (`.orb`, `.orb-1/2/3`, `@keyframes orbDrift1/2/3`).

```html
<!-- Place before the deck div in the body -->
<div class="orb orb-1"></div>
<div class="orb orb-2"></div>
<div class="orb orb-3"></div>
```

Key properties:
- `filter: blur(80px)` + `opacity: 0.12` — very subtle, atmospheric
- Uses `--accent-rgb` and `--accent2-rgb` via `radial-gradient` for theme-aware coloring
- Three different drift keyframes (18s, 22s, 25s) with `infinite alternate` — never repeats exactly
- `will-change: transform` for GPU compositing
- Hidden automatically under `@media (prefers-reduced-motion: reduce)` and `@media print`

### Noise Texture

```css
/* SVG-based noise overlay for grain/texture */
.bg-noise::after {
    content: '';
    position: absolute;
    inset: 0;
    opacity: 0.03;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    background-size: 256px 256px;
    pointer-events: none;
    z-index: 0;
}
```

### Grid Pattern

```css
/* Subtle structural grid lines */
.bg-grid::after {
    content: '';
    position: absolute;
    inset: 0;
    background-image:
        linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
    background-size: clamp(30px, 4vw, 60px) clamp(30px, 4vw, 60px);
    pointer-events: none;
    z-index: 0;
}
```

### Gradient Text (Shimmer)

```css
.gradient-text {
    background: linear-gradient(
        90deg,
        var(--text) 0%,
        var(--accent) 25%,
        var(--accent2) 50%,
        var(--accent) 75%,
        var(--text) 100%
    );
    background-size: 200% auto;
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: gradientShift 4s ease infinite;
}
@keyframes gradientShift {
    0%   { background-position: 0% center; }
    50%  { background-position: 100% center; }
    100% { background-position: 0% center; }
}
```

---

## Interactive Effects (Optional Enhancements)

### Number Counter Animation (for stats slides)

```javascript
function animateCounter(el, target, duration = 1200) {
    const isFloat = String(target).includes('.');
    const decimals = isFloat ? (String(target).split('.')[1]?.length || 1) : 0;
    const num = parseFloat(target);
    const suffix = String(target).replace(/[\d.,]+/, '');
    const prefix = String(target).match(/^[^\d]*/)?.[0] || '';
    const start = performance.now();

    function step(now) {
        const t = Math.min((now - start) / duration, 1);
        const eased = 1 - Math.pow(1 - t, 4); // ease-out-quart
        const current = num * eased;
        el.textContent = prefix + (isFloat ? current.toFixed(decimals) : Math.round(current)) + suffix;
        if (t < 1) requestAnimationFrame(step);
    }
    requestAnimationFrame(step);
}
```

### Parallax on Mouse Move (for hero/title slides)

```javascript
function setupParallax(slide) {
    const els = slide.querySelectorAll('[data-parallax]');
    slide.addEventListener('mousemove', (e) => {
        const x = (e.clientX / window.innerWidth - 0.5) * 2;
        const y = (e.clientY / window.innerHeight - 0.5) * 2;
        els.forEach(el => {
            const depth = parseFloat(el.dataset.parallax) || 1;
            el.style.transform = `translate(${x * depth * 10}px, ${y * depth * 10}px)`;
        });
    });
}
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Stagger not working | Ensure parent has `.stagger` class AND `.revealed` |
| Animation replays on back | Clear animation classes before re-adding |
| Jank on blur animations | Add `will-change: filter, opacity` to animated elements |
| Mobile performance | Disable blur/glow effects below 768px, reduce particle count |
| `prefers-reduced-motion` | Already handled in viewport-base.css — no extra work needed |
