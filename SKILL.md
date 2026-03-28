---
name: keynote-design
description: Generate a self-contained HTML presentation with Apple Keynote-quality design, animations, and typography. Use when the user wants to create a presentation, slide deck, or pitch deck. Outputs a single .html file that opens directly in a browser.
argument-hint: [topic or "from file.md"]
allowed-tools: Read, Write
---

# Keynote Design — AI Presentation Generator

Generate a **self-contained HTML file** — zero dependencies, opens in any browser, full Keynote-quality experience.

## Architecture

This skill uses progressive disclosure. The main file (you're reading it) is a concise map. Supporting files are loaded on-demand:

| File | Purpose | Load When |
|------|---------|-----------|
| `SKILL.md` | Core workflow, slide types, HTML template | Always |
| [style-presets.md](style-presets.md) | 8 curated visual presets with fonts/colors/signatures | Choosing theme |
| [viewport-base.css](viewport-base.css) | Mandatory responsive CSS (clamp, dvh, breakpoints) | Generating HTML |
| [animation-patterns.md](animation-patterns.md) | Entrance/transition/background animation reference | Generating HTML |

---

## Step 1: Plan the Slides

Design slide content as a JSON array. Each slide is one of these types:

| Type | Fields | Use for |
|------|--------|---------|
| `title` | `eyebrow, title, subtitle, notes?` | Opening slide |
| `content` | `eyebrow, title, body?, points[]?, notes?` | Body slides |
| `quote` | `quote, author?, notes?` | Impact quotes |
| `two-col` | `leftLabel, leftTitle, leftPoints[], rightLabel, rightTitle, rightPoints[], notes?` | Comparisons, before/after |
| `section` | `eyebrow, title, notes?` | Section dividers |
| `stats` | `eyebrow, title, stats:[{number, label}], notes?` | Key metrics with counter animation |
| `hero-image` | `eyebrow, title, subtitle?, imageUrl, kenBurns?, notes?` | Full-bleed image with Ken Burns |
| `hero-video` | `eyebrow, title, subtitle?, videoUrl, poster?, notes?` | Full-bleed video background |
| `code` | `eyebrow, title, language, code, notes?` | Code showcase |
| `diagram` | `eyebrow, title, diagramStyle, ...data, notes?` | Architecture, flows, timelines |
| `closing` | `eyebrow, title, subtitle?, notes?` | Final slide |

### Universal optional fields (any slide type):
- `bgGlow: "r,g,b"` — per-slide glow color (overrides theme default). Use to create atmosphere shifts.
- `bgGlow2: "r,g,b"` — secondary glow color for dual-tone gradients.
- `morphId: "name"` — Magic Move: consecutive slides with same morphId get FLIP transition on shared elements (eyebrow, title, subtitle morph smoothly between positions/sizes).

### Ken Burns variants (hero-image only):
- `kenBurns: "default"` — slow zoom-in + slight pan (14s)
- `kenBurns: "zoom-out"` — starts zoomed in, slowly pulls back
- `kenBurns: "pan-right"` — horizontal pan across the image

### Diagram sub-types:
- `diagramStyle:"layered"` + `layers:[{label, desc?, color, items[], arrow?}]`
- `diagramStyle:"flow"` + `nodes:[{label, desc?}]`
- `diagramStyle:"timeline"` + `steps:[{label, desc?}]`
- `diagramStyle:"comparison"` + `left:{title, items[]}, right:{title, items[]}`
- `diagramStyle:"radial"` + `center:"Hub", nodes:["A","B"]`

### Content Rules:
- First slide = `title`, last = `closing`. Mix types between.
- Max 5 points per slide, max 18 words per point. Titles under 12 words.
- Use `stats` for numbers that should animate. Use `code` for technical slides — provide **plain code** in the `code` field (no HTML spans); the built-in `highlight()` function auto-highlights based on `language`.
- Eyebrows: short uppercase context labels (e.g., "ARCHITECTURE", "KEY METRICS").
- **Content exceeds limits? Split into multiple slides. Never cram.**
- **Visual rhythm:** Alternate dense slides (content, stats) with breathing slides (hero-image, section, quote). Never stack 3+ dense slides in a row.
- **bgGlow strategy:** Assign unique `bgGlow` colors to every slide. Shift gradually through a color arc — e.g., blue→purple→warm→cool→blue. This creates Apple-style atmosphere progression.
- **Magic Move:** Use `morphId` on consecutive slides that share a thematic thread — the eyebrow/title will smoothly morph between positions. Great for section→detail transitions.
- **hero-image/hero-video:** Use 1-2 per presentation for maximum visual impact. Place at narrative turning points.

---

## Step 2: Choose Theme

Read [style-presets.md](style-presets.md) for full preset specifications.

Quick reference — pick from user request or default to `midnight`:

| Preset | Vibe | Best for |
|--------|------|----------|
| `midnight` | Dramatic, Apple-style, pure black | Keynotes, vision talks |
| `neon-signal` | Bold, futuristic, high-energy | Startup pitches, demos |
| `deep-indigo` | Thoughtful, technical, intellectual | Tech shares, deep dives |
| `dark-botanical` | Elegant, warm, premium | Design reviews, brand |
| `clean-white` | Professional, clear, trustworthy | Business reports, formal |
| `paper-minimal` | Refined, editorial, quiet | Academic, design critique |
| `electric-gradient` | Energetic, creative, vivid | Product launches, creative |
| `ocean-wave` | Technical but warm, modern | Engineering talks, dev conf |

---

## Step 3: Generate the HTML File

**Before generating, read these files:**
- [viewport-base.css](viewport-base.css) — include its FULL contents in `<style>`
- [animation-patterns.md](animation-patterns.md) — pick animations matching the preset's feeling
- [style-presets.md](style-presets.md) — get exact fonts, colors, signature elements for chosen preset

### HTML Template

```html
<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PRESENTATION_TITLE</title>

<!-- Fonts: from chosen preset (Google Fonts or Fontshare) -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="FONT_URL_FROM_PRESET" rel="stylesheet">

<style>
/* ── Theme Variables (from chosen preset) ── */
:root {
    --bg: PRESET_BG;
    --text: PRESET_TEXT;
    --text-dim: PRESET_TEXT_DIM;
    --text-body: PRESET_TEXT_BODY;
    --accent: PRESET_ACCENT;
    --accent2: PRESET_ACCENT2;
    --accent-rgb: PRESET_ACCENT_RGB;
    --accent2-rgb: PRESET_ACCENT2_RGB;
    --border: PRESET_BORDER;
    --dot-bg: PRESET_DOT_BG;
    --font-display: PRESET_FONT_DISPLAY;
    --font-body: PRESET_FONT_BODY;
}

/* ── PASTE viewport-base.css FULL CONTENTS HERE ── */

/* ── PASTE animation keyframes from animation-patterns.md ── */
/* Include: buildFadeUp, buildScale, buildBlur, buildSlideLeft, buildSlideRight, buildSpring, buildDrop */
/* Include: transFade, transPush, transPushBack, transZoom, transDissolve, transMorph */
/* Include: exitFade, exitFadeBack (exit animations for outgoing slides) */
/* Include: per-type entrance mapping */
/* Include: gradient-text, background effects matching preset */

/* ── PASTE preset-specific signature CSS ── */
/* e.g., bg-mesh, bg-noise, bg-grid, neon glow, etc. from the preset */
</style>
</head>
<body>

<div class="progress" id="progress"></div>
<div class="build-dots" id="buildDots"></div>
<div class="help" id="help">Space / → Next &nbsp;&nbsp; ← Back &nbsp;&nbsp; F Fullscreen &nbsp;&nbsp; P Speaker &nbsp;&nbsp; G Overview &nbsp;&nbsp; L Laser</div>

<!-- Floating orbs (ambient background) -->
<div class="orb orb-1"></div>
<div class="orb orb-2"></div>
<div class="orb orb-3"></div>

<!-- Laser pointer -->
<div class="laser-pointer" id="laserPointer"></div>

<div class="deck" id="deck">
  <!-- Slides rendered by JS -->
</div>

<!-- Slide Overview (Press G) -->
<div class="slide-overview" id="slideOverview">
    <div class="slide-overview-title">Slide Overview</div>
    <div class="slide-overview-grid" id="overviewGrid"></div>
</div>

<script>
const slides = SLIDES_DATA;

let current = 0, buildStep = 0, buildTotal = 0;

/* Build step counts per slide type */
function getBuildSteps(s) {
    switch(s.type) {
        case 'title': case 'closing': return 3;
        case 'content': return s.points?.length ? 3 : 2;
        case 'quote': return 2;
        case 'two-col': return 2;
        case 'diagram': return 2;
        case 'stats': return 2;
        case 'hero-image': return 2;
        case 'hero-video': return 2;
        case 'code': return 2;
        default: return 1;
    }
}

/* ── Syntax Highlighter (lightweight, zero-dependency) ── */
function highlight(code, lang) {
    if (!code) return '';
    const esc = code.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');

    const LANGS = {
        javascript: { kw: /\b(const|let|var|function|return|if|else|for|while|class|import|export|from|default|async|await|new|this|typeof|instanceof|try|catch|throw|switch|case|break|continue|yield|of|in|null|undefined|true|false)\b/g, str: /(["'`])(?:(?!\1|\\).|\\.)*?\1/g, cm: /\/\/.*$|\/\*[\s\S]*?\*\//gm, fn: /\b([a-zA-Z_$]\w*)\s*(?=\()/g },
        typescript: null, /* alias → javascript */
        python: { kw: /\b(def|class|return|if|elif|else|for|while|import|from|as|with|try|except|raise|finally|lambda|yield|pass|break|continue|and|or|not|in|is|None|True|False|self|async|await|print)\b/g, str: /("""[\s\S]*?"""|'''[\s\S]*?'''|"(?:[^"\\]|\\.)*"|'(?:[^'\\]|\\.)*')/g, cm: /#.*$/gm, fn: /\b([a-zA-Z_]\w*)\s*(?=\()/g },
        go: { kw: /\b(func|return|if|else|for|range|switch|case|default|var|const|type|struct|interface|package|import|defer|go|chan|select|map|make|new|nil|true|false|string|int|bool|error|fmt)\b/g, str: /("(?:[^"\\]|\\.)*"|`[^`]*`)/g, cm: /\/\/.*$|\/\*[\s\S]*?\*\//gm, fn: /\b([a-zA-Z_]\w*)\s*(?=\()/g },
        rust: { kw: /\b(fn|let|mut|const|if|else|for|while|loop|match|return|use|mod|pub|struct|enum|impl|trait|where|self|Self|true|false|Some|None|Ok|Err|async|await|move|unsafe|extern|crate|super|type|as|in|ref|dyn|Box|Vec|String|Option|Result)\b/g, str: /("(?:[^"\\]|\\.)*")/g, cm: /\/\/.*$|\/\*[\s\S]*?\*\//gm, fn: /\b([a-zA-Z_]\w*)\s*(?=\()/g },
        html: { kw: /(&lt;\/?[a-zA-Z][a-zA-Z0-9-]*)/g, str: /("(?:[^"\\]|\\.)*"|'(?:[^'\\]|\\.)*')/g, cm: /&lt;!--[\s\S]*?--&gt;/g },
        css: { kw: /(@[a-z-]+|:[a-z-]+)\b/g, str: /("(?:[^"\\]|\\.)*"|'(?:[^'\\]|\\.)*')/g, cm: /\/\*[\s\S]*?\*\//gm, fn: /([.#][a-zA-Z_-][\w-]*)/g },
        sql: { kw: /\b(SELECT|FROM|WHERE|INSERT|UPDATE|DELETE|CREATE|DROP|ALTER|TABLE|INTO|VALUES|SET|JOIN|LEFT|RIGHT|INNER|OUTER|ON|AND|OR|NOT|NULL|AS|ORDER|BY|GROUP|HAVING|LIMIT|OFFSET|UNION|INDEX|PRIMARY|KEY|FOREIGN|REFERENCES|CASCADE|DISTINCT|COUNT|SUM|AVG|MAX|MIN|EXISTS|IN|BETWEEN|LIKE|IS)\b/gi, str: /('(?:[^'\\]|\\.)*')/g, cm: /--.*$/gm },
        shell: { kw: /\b(if|then|else|fi|for|while|do|done|case|esac|function|return|exit|echo|export|source|cd|ls|grep|sed|awk|curl|sudo|apt|npm|pip|docker|git)\b/g, str: /(["'])(?:(?!\1|\\).|\\.)*?\1/g, cm: /#.*$/gm },
        bash: null, sh: null, zsh: null, /* aliases → shell */
        jsx: null, tsx: null, js: null, ts: null, /* aliases → javascript */
        py: null /* alias → python */
    };

    /* Resolve aliases */
    const aliases = { typescript:'javascript', jsx:'javascript', tsx:'javascript', js:'javascript', ts:'javascript', py:'python', bash:'shell', sh:'shell', zsh:'shell' };
    const key = (lang||'').toLowerCase();
    const rules = LANGS[key] || LANGS[aliases[key]] || LANGS.javascript;

    /* Tokenize: protect strings/comments first, then highlight keywords/functions */
    const tokens = [];
    let result = esc;

    /* Pass 1: comments */
    if (rules.cm) result = result.replace(rules.cm, m => { const id = tokens.length; tokens.push(`<span class="cm">${m}</span>`); return `\x00${id}\x00`; });
    /* Pass 2: strings */
    if (rules.str) result = result.replace(rules.str, m => { const id = tokens.length; tokens.push(`<span class="str">${m}</span>`); return `\x00${id}\x00`; });
    /* Pass 3: functions (before keywords so fn names aren't eaten) */
    if (rules.fn) result = result.replace(rules.fn, (m, name) => m.startsWith('\x00') ? m : `<span class="fn">${name}</span>` + m.slice(name.length));
    /* Pass 4: keywords */
    if (rules.kw) result = result.replace(rules.kw, m => m.startsWith('\x00') ? m : `<span class="kw">${m}</span>`);

    /* Restore protected tokens */
    result = result.replace(/\x00(\d+)\x00/g, (_, id) => tokens[id]);
    return result;
}

/* Render slide HTML from data */
function renderSlide(s, i) {
    const n = `<div class="slide-num">${i+1} / ${slides.length}</div>`;
    const b = (step) => `data-build="${step}"`;
    const morph = (id) => s.morphId ? ` data-morph-id="${s.morphId}-${id}"` : '';

    switch(s.type) {
        case 'title':
            return `${n}<div ${b(1)} class="eyebrow"${morph('ey')}>${s.eyebrow||''}</div><div ${b(2)} class="title gradient-text"${morph('t')}>${s.title}</div>${s.subtitle?`<div ${b(3)} class="subtitle"${morph('sub')}>${s.subtitle}</div>`:''}`;

        case 'content':
            return `${n}<div ${b(1)} class="eyebrow"${morph('ey')}>${s.eyebrow||''}</div><div ${b(1)} class="title"${morph('t')}>${s.title}</div>${s.body?`<div ${b(2)} class="body">${s.body}</div>`:''}${s.points?`<ul ${b(s.body?3:2)} class="points stagger">${s.points.map(p=>`<li>${p}</li>`).join('')}</ul>`:''}`;

        case 'quote':
            return `${n}<div ${b(1)} class="quote-text"${morph('t')}>${s.quote}</div>${s.author?`<div ${b(2)} class="quote-author"${morph('sub')}>— ${s.author}</div>`:''}`;

        case 'two-col':
            return `${n}<div class="col" ${b(1)}><div class="col-label">${s.leftLabel||''}</div><div class="title">${s.leftTitle||''}</div>${s.leftPoints?`<ul class="points stagger">${s.leftPoints.map(p=>`<li>${p}</li>`).join('')}</ul>`:''}</div><div class="col" ${b(2)}><div class="col-label">${s.rightLabel||''}</div><div class="title">${s.rightTitle||''}</div>${s.rightPoints?`<ul class="points stagger">${s.rightPoints.map(p=>`<li>${p}</li>`).join('')}</ul>`:''}</div>`;

        case 'stats':
            return `${n}<div ${b(1)} class="eyebrow"${morph('ey')}>${s.eyebrow||''}</div><div ${b(1)} class="title"${morph('t')}>${s.title}</div><div ${b(2)} class="stats-grid stagger">${(s.stats||[]).map(st=>`<div class="stat-item"><div class="stat-number" data-target="${st.number}">${st.number}</div><div class="stat-label">${st.label}</div></div>`).join('')}</div>`;

        case 'hero-image':
            const kbClass = s.kenBurns === 'zoom-out' ? ' kb-zoom-out' : s.kenBurns === 'pan-right' ? ' kb-pan-right' : '';
            return `${n}<div class="hero-bg${kbClass}" style="background-image:url('${s.imageUrl}')"></div><div class="hero-overlay"></div><div class="hero-content"><div ${b(1)} class="eyebrow"${morph('ey')}>${s.eyebrow||''}</div><div ${b(1)} class="title"${morph('t')}>${s.title}</div>${s.subtitle?`<div ${b(2)} class="subtitle"${morph('sub')}>${s.subtitle}</div>`:''}</div>`;

        case 'hero-video':
            return `${n}<video class="hero-video-bg" autoplay muted loop playsinline${s.poster ? ` poster="${s.poster}"` : ''} src="${s.videoUrl}"></video><div class="hero-video-overlay"></div><div class="hero-video-content"><div ${b(1)} class="eyebrow"${morph('ey')}>${s.eyebrow||''}</div><div ${b(1)} class="title"${morph('t')}>${s.title}</div>${s.subtitle?`<div ${b(2)} class="subtitle"${morph('sub')}>${s.subtitle}</div>`:''}</div>`;

        case 'code':
            return `${n}<div ${b(1)} class="eyebrow"${morph('ey')}>${s.eyebrow||''}</div><div ${b(1)} class="title"${morph('t')}>${s.title}</div><pre ${b(2)} class="code-block"><code>${highlight(s.code||'', s.language)}</code></pre>`;

        case 'diagram':
            return `${n}<div ${b(1)} class="eyebrow"${morph('ey')}>${s.eyebrow||''}</div><div ${b(1)} class="title"${morph('t')}>${s.title}</div><div ${b(2)} class="html-diagram">${renderHtmlDiagram(s)}</div>`;

        case 'section':
            return `${n}<div ${b(1)} class="eyebrow"${morph('ey')}>${s.eyebrow||''}</div><div ${b(1)} class="title gradient-text"${morph('t')}>${s.title}</div>`;

        case 'closing':
            return `${n}<div ${b(1)} class="eyebrow"${morph('ey')}>${s.eyebrow||'Thank You'}</div><div ${b(2)} class="title gradient-text"${morph('t')}>${s.title}</div>${s.subtitle?`<div ${b(3)} class="subtitle"${morph('sub')}>${s.subtitle}</div>`:''}`;

        default:
            return `<div class="title">${s.title||''}</div>`;
    }
}

/* ── HTML Diagram Renderer ── */
/* (Include full diagram rendering code for layered/flow/timeline/comparison/radial) */
const LAYER_COLORS=['purple','yellow','orange','blue','red','green','teal'];

function renderHtmlDiagram(s) {
    const style = s.diagramStyle || (s.layers?'layered':s.nodes?'flow':s.steps?'timeline':'comparison');

    if(style==='layered'&&s.layers) return `<div class="diagram-layered">${s.layers.map((l,i)=>{const c=l.color||LAYER_COLORS[i%LAYER_COLORS.length];return `${i>0?'<div class="layer-arrow">↓ '+(l.arrow||'')+'</div>':''}
<div class="diagram-layer layer-${c}"><div class="layer-info"><div class="layer-label">${l.label}</div>${l.desc?`<div class="layer-desc">${l.desc}</div>`:''}</div><div class="layer-items">${(l.items||[]).map(it=>`<div class="layer-item">${it}</div>`).join('')}</div></div>`;}).join('')}</div>`;

    if(style==='flow'&&s.nodes) return `<div class="diagram-flow">${s.nodes.map((n,i)=>{const nd=typeof n==='string'?{label:n}:n;return `${i>0?`<div class="flow-arrow">${nd.arrow||'→'}</div>`:''}
<div class="flow-node"><div class="flow-node-label">${nd.label}</div>${nd.desc?`<div class="flow-node-desc">${nd.desc}</div>`:''}</div>`;}).join('')}</div>`;

    if(style==='timeline'&&s.steps) return `<div class="diagram-timeline">${s.steps.map((st,i)=>{const t=typeof st==='string'?{label:st}:st;return `<div class="timeline-node"><div class="timeline-dot"></div><div class="timeline-content"><div class="timeline-label">${t.label}</div>${t.desc?`<div class="timeline-desc">${t.desc}</div>`:''}</div></div>`;}).join('')}</div>`;

    if(style==='comparison'&&s.left&&s.right) return `<div class="diagram-comparison"><div class="comparison-side"><div class="comparison-header">${s.left.title}</div>${(s.left.items||[]).map(it=>`<div class="comparison-item">${it}</div>`).join('')}</div><div class="comparison-side"><div class="comparison-header">${s.right.title}</div>${(s.right.items||[]).map(it=>`<div class="comparison-item">${it}</div>`).join('')}</div></div>`;

    if(style==='radial'&&s.nodes){const center=s.center||s.nodes[0],outer=typeof center==='string'?s.nodes.slice(1):s.nodes,cl=typeof center==='string'?center:center.label,r=120;return `<div class="diagram-radial"><div class="radial-center">${cl}</div>${outer.map((n,i)=>{const l=typeof n==='string'?n:n.label,a=(i/outer.length)*2*Math.PI-Math.PI/2,x=Math.cos(a)*r+140-35,y=Math.sin(a)*r+140-14;return `<div class="radial-node" style="left:${x}px;top:${y}px;animation-delay:${0.2+i*0.1}s">${l}</div>`;}).join('')}</div>`;}

    return '';
}

let transDir = 1;

/* ── Build Deck ── */
const deck = document.getElementById('deck');
slides.forEach((s, i) => {
    const div = document.createElement('div');
    div.className = 'slide' + (i === 0 ? ' active' : '');
    div.setAttribute('data-type', s.type);
    /* Per-slide background glow */
    if (s.bgGlow) div.style.setProperty('--glow-color', s.bgGlow);
    if (s.bgGlow2) div.style.setProperty('--glow-color2', s.bgGlow2);
    div.innerHTML = renderSlide(s, i);
    deck.appendChild(div);
});

/* ── Build System ── */
function applyBuild(step) {
    const slide = deck.children[current];
    slide.querySelectorAll('[data-build]').forEach(el => {
        const s = parseInt(el.dataset.build);
        if (s <= step) {
            if (!el.classList.contains('revealed')) {
                el.classList.add('revealed');
                /* Stagger children */
                if (el.classList.contains('stagger')) {
                    el.querySelectorAll(':scope > *').forEach((child, idx) => {
                        child.style.opacity = '0';
                        child.style.animationDelay = `${idx * parseFloat(getComputedStyle(document.documentElement).getPropertyValue('--stagger') || 0.08)}s`;
                    });
                }
            }
        } else {
            el.classList.remove('revealed');
        }
    });

    /* Update build dots */
    const dots = document.getElementById('buildDots');
    dots.innerHTML = buildTotal > 1 ? Array.from({length:buildTotal},(_,i)=>`<div class="build-dot${i<step?' active':''}"></div>`).join('') : '';
}

let isTransitioning = false;

/* ── Magic Move: FLIP animation for shared morph-id elements ── */
function performMagicMove(prevSlide, nextSlide, callback) {
    const prevMorphs = prevSlide.querySelectorAll('[data-morph-id]');
    const nextMorphs = nextSlide.querySelectorAll('[data-morph-id]');
    if (!prevMorphs.length || !nextMorphs.length) { callback(); return false; }

    const firstMap = {};
    prevMorphs.forEach(el => { firstMap[el.dataset.morphId] = { rect: el.getBoundingClientRect(), el }; });

    nextSlide.classList.add('active');
    nextSlide.style.opacity = '0';
    nextSlide.querySelectorAll('[data-build]').forEach(el => el.classList.add('revealed'));
    void nextSlide.offsetWidth;

    let hasPairs = false;
    const animations = [];

    nextMorphs.forEach(el => {
        const id = el.dataset.morphId;
        if (!firstMap[id]) return;
        hasPairs = true;
        const first = firstMap[id].rect;
        const last = el.getBoundingClientRect();
        const dx = first.left - last.left, dy = first.top - last.top;
        const sw = first.width / (last.width || 1), sh = first.height / (last.height || 1);
        el.style.transformOrigin = 'top left';
        el.style.transform = `translate(${dx}px, ${dy}px) scale(${sw}, ${sh})`;
        el.style.transition = 'none';
        void el.offsetWidth;
        el.style.transition = 'transform 0.65s cubic-bezier(0.22, 1, 0.36, 1)';
        el.style.transform = 'translate(0, 0) scale(1, 1)';
        animations.push(el);
    });

    if (!hasPairs) {
        nextSlide.classList.remove('active'); nextSlide.style.opacity = '';
        nextSlide.querySelectorAll('[data-build]').forEach(el => el.classList.remove('revealed'));
        callback(); return false;
    }

    prevSlide.style.transition = 'opacity 0.35s ease'; prevSlide.style.opacity = '0';
    nextSlide.style.transition = 'opacity 0.3s ease 0.05s'; nextSlide.style.opacity = '1';

    setTimeout(() => {
        prevSlide.classList.remove('active'); prevSlide.style.transition = ''; prevSlide.style.opacity = '';
        prevSlide.querySelectorAll('[data-build]').forEach(el => el.classList.remove('revealed'));
        animations.forEach(el => { el.style.transform = ''; el.style.transition = ''; el.style.transformOrigin = ''; });
        nextSlide.style.transition = ''; nextSlide.style.opacity = '';
    }, 700);
    return true;
}

function showSlide(idx, revealAll) {
    if (idx < 0 || idx >= slides.length || isTransitioning) return;
    if (idx === current) return;
    isTransitioning = true;

    const prev = deck.children[current];
    const next = deck.children[idx];

    const didMagicMove = performMagicMove(prev, next, () => {
        const exitClass = transDir >= 0 ? 'exit-forward' : 'exit-backward';
        prev.classList.remove('trans-morph', 'trans-morph-back');
        prev.classList.add(exitClass);
        setTimeout(() => {
            prev.classList.remove('active', exitClass, 'exit-forward', 'exit-backward');
            prev.querySelectorAll('[data-build]').forEach(el => el.classList.remove('revealed'));
            next.classList.remove('trans-morph', 'trans-morph-back');
            next.classList.add('active');
            void next.offsetWidth;
            next.classList.add(transDir < 0 ? 'trans-morph-back' : 'trans-morph');
            finishTransition(idx, next, revealAll);
        }, 400);
    });

    if (didMagicMove) {
        current = idx;
        buildTotal = getBuildSteps(slides[current]);
        buildStep = revealAll ? buildTotal : 1;
        applyBuild(buildStep);
        if (slides[current].type === 'stats') {
            next.querySelectorAll('.stat-number[data-target]').forEach(el => animateCounter(el, el.dataset.target));
        }
        document.getElementById('progress').style.width = ((current+1)/slides.length*100)+'%';
        setTimeout(() => { isTransitioning = false; }, 700);
    }
}

function finishTransition(idx, cur, revealAll) {
    current = idx;
    buildTotal = getBuildSteps(slides[current]);
    buildStep = revealAll ? buildTotal : 1;
    applyBuild(buildStep);
    if (slides[current].type === 'stats') {
        cur.querySelectorAll('.stat-number[data-target]').forEach(el => animateCounter(el, el.dataset.target));
    }
    document.getElementById('progress').style.width = ((current+1)/slides.length*100)+'%';
    isTransitioning = false;
}

function advance() {
    if (buildStep < buildTotal) { buildStep++; applyBuild(buildStep); }
    else if (current < slides.length - 1) { transDir = 1; showSlide(current + 1); }
}

function retreat() {
    if (buildStep > 1) { buildStep--; applyBuild(buildStep); }
    else if (current > 0) { transDir = -1; showSlide(current - 1, true); }
}

/* ── Counter Animation (with elastic overshoot) ── */
function animateCounter(el, target, duration) {
    duration = duration || 1400;
    const raw = String(target);
    const isFloat = raw.includes('.');
    const decimals = isFloat ? (raw.split('.')[1]?.length || 1) : 0;
    const num = parseFloat(raw.replace(/[^\d.\-]/g, ''));
    const suffix = raw.replace(/^[\d.\-,]+/, '');
    const prefix = raw.match(/^[^\d\-]*/)?.[0] || '';
    if (isNaN(num)) return;

    function easeOutBack(t) {
        const c1 = 1.70158, c3 = c1 + 1;
        return 1 + c3 * Math.pow(t - 1, 3) + c1 * Math.pow(t - 1, 2);
    }

    const start = performance.now();
    function step(now) {
        const t = Math.min((now - start) / duration, 1);
        const eased = easeOutBack(t);
        const val = Math.min(num * eased, num * 1.06);
        const display = t >= 1 ? num : val;
        el.textContent = prefix + (isFloat ? display.toFixed(decimals) : Math.round(display).toLocaleString()) + suffix;
        if (t < 1) requestAnimationFrame(step);
    }
    requestAnimationFrame(step);
}

/* ── Init (don't use showSlide — it skips idx===current) ── */
(function initFirstSlide() {
    current = 0;
    buildTotal = getBuildSteps(slides[0]);
    buildStep = 1;
    applyBuild(buildStep);
    document.getElementById('progress').style.width = (1/slides.length*100)+'%';
})();
setTimeout(() => document.getElementById('help').classList.add('hidden'), 4000);

/* ── Controls ── */
document.addEventListener('keydown', e => {
    if (e.key === 'ArrowRight' || e.key === ' ' || e.key === 'Enter') { e.preventDefault(); advance(); }
    if (e.key === 'ArrowLeft' || e.key === 'Backspace') { e.preventDefault(); retreat(); }
    if (e.key === 'f' || e.key === 'F') { document.documentElement.requestFullscreen?.() || document.documentElement.webkitRequestFullscreen?.(); }
    if (e.key === 'Escape' && (document.fullscreenElement || document.webkitFullscreenElement)) { document.exitFullscreen?.() || document.webkitExitFullscreen?.(); }
    if (e.key === 'Home') showSlide(0);
    if (e.key === 'End') showSlide(slides.length - 1, true);
});
document.addEventListener('click', e => { if (e.clientX > window.innerWidth / 3) advance(); else retreat(); });

/* ── Speaker Mode (Press P) ── */
const channel = new BroadcastChannel('keynote-sync');
let speakerWin = null;

function syncSpeaker() {
    channel.postMessage({ type: 'sync', current, buildStep, buildTotal });
}
const _origApplyBuild = applyBuild;
applyBuild = function(step) { _origApplyBuild(step); syncSpeaker(); };

channel.onmessage = (e) => {
    if (e.data.type === 'nav') {
        if (e.data.action === 'advance') advance();
        else if (e.data.action === 'retreat') retreat();
        else if (e.data.action === 'goto') { transDir = 1; showSlide(e.data.idx); }
    }
};

function openSpeakerMode() {
    if (speakerWin && !speakerWin.closed) { speakerWin.focus(); return; }
    speakerWin = window.open('', 'keynote-speaker', 'width=960,height=700');
    if (!speakerWin) { alert('Please allow popups for speaker mode'); return; }

    const doc = speakerWin.document;
    doc.open();
    doc.write(`<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<title>Speaker Mode</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{background:#1a1a1a;color:#e0e0e0;font-family:'Inter',-apple-system,sans-serif;padding:20px;height:100vh;display:grid;grid-template-rows:auto 1fr auto;gap:16px;overflow:hidden}
.top-bar{display:flex;justify-content:space-between;align-items:center;padding:8px 0;border-bottom:1px solid #333}
.timer{font-size:2.5rem;font-weight:700;font-variant-numeric:tabular-nums;color:#2997ff;letter-spacing:-0.02em}
.slide-counter{font-size:1rem;color:#888}
.main{display:grid;grid-template-columns:1fr 1fr;gap:16px;min-height:0}
.preview-panel{display:flex;flex-direction:column;gap:8px}
.preview-label{font-size:0.7rem;text-transform:uppercase;letter-spacing:0.15em;color:#666;font-weight:600}
.preview-box{flex:1;background:#0a0a0a;border-radius:8px;border:1px solid #333;display:flex;align-items:center;justify-content:center;padding:16px;overflow:hidden;position:relative}
.preview-box.current{border-color:#2997ff;box-shadow:0 0 20px rgba(41,151,255,0.15)}
.preview-title{font-size:clamp(0.8rem,1.8vw,1.2rem);font-weight:700;text-align:center;line-height:1.2}
.preview-eyebrow{font-size:0.6rem;color:#2997ff;text-transform:uppercase;letter-spacing:0.15em;margin-bottom:4px;text-align:center}
.preview-type{position:absolute;top:6px;right:8px;font-size:0.55rem;color:#555;text-transform:uppercase;letter-spacing:0.1em}
.bottom{display:flex;flex-direction:column;gap:8px}
.notes-label{font-size:0.7rem;text-transform:uppercase;letter-spacing:0.15em;color:#666;font-weight:600}
.notes{background:#0f0f0f;border-radius:8px;border:1px solid #333;padding:16px;font-size:0.95rem;line-height:1.65;color:#ccc;max-height:30vh;overflow-y:auto}
.notes:empty::before{content:'No speaker notes for this slide';color:#555;font-style:italic}
.controls{display:flex;gap:8px;margin-top:8px}
.controls button{flex:1;padding:10px;border:1px solid #333;border-radius:6px;background:#222;color:#ccc;font-size:0.85rem;cursor:pointer;transition:all 0.15s}
.controls button:hover{background:#333;border-color:#555}
.controls button:active{background:#444}
.build-indicator{display:flex;gap:4px;align-items:center;margin-left:12px}
.build-pip{width:6px;height:6px;border-radius:50%;background:#333}
.build-pip.active{background:#2997ff}
</style>
</head>
<body>
<div class="top-bar">
    <div class="timer" id="timer">00:00</div>
    <div style="display:flex;align-items:center">
        <div class="slide-counter" id="counter">1 / ${slides.length}</div>
        <div class="build-indicator" id="buildPips"></div>
    </div>
</div>
<div class="main">
    <div class="preview-panel">
        <div class="preview-label">Current</div>
        <div class="preview-box current" id="previewCurrent">
            <div><div class="preview-eyebrow" id="curEyebrow"></div><div class="preview-title" id="curTitle"></div></div>
            <div class="preview-type" id="curType"></div>
        </div>
    </div>
    <div class="preview-panel">
        <div class="preview-label">Next</div>
        <div class="preview-box" id="previewNext">
            <div><div class="preview-eyebrow" id="nextEyebrow"></div><div class="preview-title" id="nextTitle"></div></div>
            <div class="preview-type" id="nextType"></div>
        </div>
    </div>
</div>
<div class="bottom">
    <div class="notes-label">Speaker Notes</div>
    <div class="notes" id="notes"></div>
    <div class="controls">
        <button id="btnPrev">\u2190 Back</button>
        <button id="btnNext">Next \u2192</button>
    </div>
</div>
<script>
const slides = ${JSON.stringify(slides.map(s => ({
    type: s.type, title: s.title || s.quote || '', eyebrow: s.eyebrow || '', notes: s.notes || ''
})))};
const channel = new BroadcastChannel('keynote-sync');
const timerEl = document.getElementById('timer');
const counterEl = document.getElementById('counter');
const notesEl = document.getElementById('notes');
const buildPipsEl = document.getElementById('buildPips');
const curTitle = document.getElementById('curTitle');
const curEyebrow = document.getElementById('curEyebrow');
const curType = document.getElementById('curType');
const nextTitle = document.getElementById('nextTitle');
const nextEyebrow = document.getElementById('nextEyebrow');
const nextType = document.getElementById('nextType');

let startTime = Date.now();
setInterval(() => {
    const elapsed = Math.floor((Date.now() - startTime) / 1000);
    timerEl.textContent = String(Math.floor(elapsed / 60)).padStart(2, '0') + ':' + String(elapsed % 60).padStart(2, '0');
}, 1000);

function updateUI(cur, buildStep, buildTotal) {
    const slide = slides[cur], next = slides[cur + 1];
    counterEl.textContent = (cur + 1) + ' / ' + slides.length;
    notesEl.textContent = slide.notes || '';
    curEyebrow.textContent = slide.eyebrow;
    curTitle.textContent = slide.title;
    curType.textContent = slide.type;
    if (next) { nextEyebrow.textContent = next.eyebrow; nextTitle.textContent = next.title; nextType.textContent = next.type; }
    else { nextEyebrow.textContent = ''; nextTitle.textContent = 'End'; nextType.textContent = ''; }
    buildPipsEl.innerHTML = buildTotal > 1 ? Array.from({length: buildTotal}, (_, i) =>
        '<div class="build-pip' + (i < buildStep ? ' active' : '') + '"><\/div>'
    ).join('') : '';
}

channel.onmessage = (e) => { if (e.data.type === 'sync') updateUI(e.data.current, e.data.buildStep, e.data.buildTotal); };

document.getElementById('btnNext').addEventListener('click', () => channel.postMessage({ type: 'nav', action: 'advance' }));
document.getElementById('btnPrev').addEventListener('click', () => channel.postMessage({ type: 'nav', action: 'retreat' }));
document.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowRight' || e.key === ' ') { e.preventDefault(); channel.postMessage({ type: 'nav', action: 'advance' }); }
    if (e.key === 'ArrowLeft') { e.preventDefault(); channel.postMessage({ type: 'nav', action: 'retreat' }); }
});
updateUI(0, 1, 2);
<\/script>
</body>
</html>`);
    doc.close();
    setTimeout(syncSpeaker, 200);
}

document.addEventListener('keydown', e => {
    if (e.key === 'p' || e.key === 'P') { e.preventDefault(); openSpeakerMode(); }
});

/* ── Slide Overview (Press G) ── */
let overviewOpen = false;
const overviewEl = document.getElementById('slideOverview');
const overviewGrid = document.getElementById('overviewGrid');

function buildOverviewGrid() {
    overviewGrid.innerHTML = '';
    slides.forEach((s, i) => {
        const thumb = document.createElement('div');
        thumb.className = 'slide-thumb' + (i === current ? ' current' : '');
        if (s.bgGlow) thumb.style.setProperty('--glow-color', s.bgGlow);
        thumb.innerHTML = `<div class="slide-thumb-num">${i+1}</div><div class="slide-thumb-type">${s.type}</div><div class="slide-thumb-eyebrow">${s.eyebrow||''}</div><div class="slide-thumb-title">${s.title||s.quote||''}</div>`;
        thumb.addEventListener('click', () => { closeOverview(); transDir = i > current ? 1 : -1; showSlide(i, true); });
        overviewGrid.appendChild(thumb);
    });
}
function openOverview() { if (overviewOpen) return; overviewOpen = true; buildOverviewGrid(); overviewEl.style.display = 'block'; requestAnimationFrame(() => overviewEl.classList.add('visible')); }
function closeOverview() { if (!overviewOpen) return; overviewOpen = false; overviewEl.classList.remove('visible'); setTimeout(() => { overviewEl.style.display = 'none'; }, 300); }
document.addEventListener('keydown', e => {
    if (e.key === 'g' || e.key === 'G') { e.preventDefault(); overviewOpen ? closeOverview() : openOverview(); }
    if (e.key === 'Escape' && overviewOpen) { e.preventDefault(); closeOverview(); }
});

/* ── Touch / Swipe Gestures ── */
let touchStartX = 0, touchStartY = 0, touchStartTime = 0;
document.addEventListener('touchstart', e => {
    if (overviewOpen) return;
    touchStartX = e.touches[0].clientX;
    touchStartY = e.touches[0].clientY;
    touchStartTime = Date.now();
}, { passive: true });
document.addEventListener('touchend', e => {
    if (overviewOpen) return;
    const dx = e.changedTouches[0].clientX - touchStartX;
    const dy = e.changedTouches[0].clientY - touchStartY;
    const dt = Date.now() - touchStartTime;
    const absDx = Math.abs(dx), absDy = Math.abs(dy);
    if (absDx > 50 && absDx > absDy * 1.5 && dt < 500) {
        if (dx < 0) advance();
        else retreat();
    }
}, { passive: true });

/* ── Laser Pointer (Press L) ── */
let laserActive = false;
const laserEl = document.getElementById('laserPointer');
const laserTrails = [];
const MAX_TRAILS = 8;
for (let i = 0; i < MAX_TRAILS; i++) {
    const trail = document.createElement('div');
    trail.className = 'laser-trail';
    document.body.appendChild(trail);
    laserTrails.push(trail);
}
document.addEventListener('mousemove', e => {
    if (!laserActive) return;
    laserEl.style.left = e.clientX + 'px';
    laserEl.style.top = e.clientY + 'px';
    for (let i = MAX_TRAILS - 1; i > 0; i--) {
        laserTrails[i].style.left = laserTrails[i - 1].style.left;
        laserTrails[i].style.top = laserTrails[i - 1].style.top;
        laserTrails[i].style.opacity = (1 - i / MAX_TRAILS) * 0.3;
        laserTrails[i].style.width = laserTrails[i].style.height = (8 - i * 0.6) + 'px';
    }
    laserTrails[0].style.left = e.clientX + 'px';
    laserTrails[0].style.top = e.clientY + 'px';
    laserTrails[0].style.opacity = '0.4';
});
function toggleLaser() {
    laserActive = !laserActive;
    laserEl.classList.toggle('active', laserActive);
    document.body.style.cursor = laserActive ? 'none' : '';
    if (!laserActive) laserTrails.forEach(t => t.style.opacity = '0');
}
document.addEventListener('keydown', e => {
    if (e.key === 'l' || e.key === 'L') { e.preventDefault(); toggleLaser(); }
});

document.getElementById('help').innerHTML = 'Space/\u2192 Next &nbsp; \u2190Back &nbsp; F Full &nbsp; P Speaker &nbsp; G Grid &nbsp; L Laser';
</script>
</body>
</html>
```

**CRITICAL**: When generating the actual file:
1. Replace `SLIDES_DATA` with the actual JSON array as inline JavaScript
2. Replace all `PRESET_*` placeholders with the chosen preset's actual values
3. Replace `FONT_URL_FROM_PRESET` with the actual Google Fonts / Fontshare URL
4. **Paste the FULL contents** of `viewport-base.css` into `<style>` (includes hero-image, hero-video, Ken Burns, slide-overview, floating orbs, laser pointer, and print/PDF CSS)
5. **Paste the relevant animation keyframes** from `animation-patterns.md` (including exit animations, non-uniform stagger)
6. Include the preset's signature CSS (bg-mesh, bg-noise, etc.) — use `--glow-color` / `--glow-color2` CSS vars in gradient mesh
7. Include diagram CSS (layered/flow/timeline/comparison/radial styles from the diagram section below)
8. Add `notes` field to each slide's JSON data for speaker mode
9. Assign unique `bgGlow` to every slide — create a color arc matching the theme's accent palette
10. Use `morphId` on 2-3 pairs of consecutive slides for Magic Move transitions

### Diagram CSS

Include these styles when any `diagram` slide type is used:

```css
/* ── Diagram: Shared ── */
.html-diagram{flex:1;display:flex;align-items:center;justify-content:center;min-height:0;padding:0 clamp(1rem,3vw,3rem);overflow:hidden;width:100%}

/* ── Layered ── */
.diagram-layered{display:flex;flex-direction:column;gap:clamp(8px,1.2vh,14px);width:100%;max-width:85%}
.diagram-layer{display:flex;align-items:center;gap:clamp(12px,2vw,24px);padding:clamp(12px,2vh,20px) clamp(14px,2.5vw,24px);border-radius:10px;border:1.5px solid;animation:buildFadeUp 0.5s var(--ease-smooth) both}
.diagram-layer:nth-child(1){animation-delay:.1s}.diagram-layer:nth-child(2){animation-delay:.2s}.diagram-layer:nth-child(3){animation-delay:.3s}.diagram-layer:nth-child(4){animation-delay:.4s}.diagram-layer:nth-child(5){animation-delay:.5s}
.layer-info{min-width:28%}.layer-label{font-weight:600;font-size:clamp(13px,1.6vw,18px);margin-bottom:4px}.layer-desc{font-size:clamp(9px,1.1vw,13px);opacity:.6}
.layer-items{display:flex;gap:clamp(6px,1vw,12px);flex-wrap:wrap;flex:1;justify-content:flex-end}
.layer-item{padding:clamp(6px,0.8vh,10px) clamp(12px,1.5vw,18px);border-radius:6px;font-size:clamp(10px,1.1vw,14px);font-weight:500;border:1px solid;white-space:nowrap}
.layer-arrow{text-align:center;font-size:clamp(10px,1vw,13px);opacity:.35;line-height:1;margin:clamp(2px,0.5vh,6px) 0}

/* Layer color variants */
.layer-purple{background:rgba(147,112,219,.08);border-color:rgba(147,112,219,.25);color:#9370db}
.layer-purple .layer-item{background:rgba(147,112,219,.06);border-color:rgba(147,112,219,.2);color:#9370db}
.layer-yellow{background:rgba(218,165,32,.08);border-color:rgba(218,165,32,.25);color:#b8860b}
.layer-yellow .layer-item{background:rgba(218,165,32,.06);border-color:rgba(218,165,32,.2);color:#b8860b}
.layer-orange{background:rgba(210,105,30,.08);border-color:rgba(210,105,30,.25);color:#d2691e}
.layer-orange .layer-item{background:rgba(210,105,30,.06);border-color:rgba(210,105,30,.2);color:#d2691e}
.layer-blue{background:rgba(70,130,180,.08);border-color:rgba(70,130,180,.25);color:#4682b4}
.layer-blue .layer-item{background:rgba(70,130,180,.06);border-color:rgba(70,130,180,.2);color:#4682b4}
.layer-red{background:rgba(205,92,92,.08);border-color:rgba(205,92,92,.25);color:#cd5c5c}
.layer-red .layer-item{background:rgba(205,92,92,.06);border-color:rgba(205,92,92,.2);color:#cd5c5c}
.layer-green{background:rgba(60,179,113,.08);border-color:rgba(60,179,113,.25);color:#3cb371}
.layer-green .layer-item{background:rgba(60,179,113,.06);border-color:rgba(60,179,113,.2);color:#3cb371}
.layer-teal{background:rgba(0,128,128,.08);border-color:rgba(0,128,128,.25);color:#008080}
.layer-teal .layer-item{background:rgba(0,128,128,.06);border-color:rgba(0,128,128,.2);color:#008080}

/* ── Flow ── */
.diagram-flow{display:flex;align-items:center;gap:0;width:100%;max-width:90%;justify-content:center;flex-wrap:wrap}
.flow-node{display:flex;flex-direction:column;align-items:center;gap:8px;padding:clamp(14px,2vh,24px) clamp(18px,2.5vw,32px);border-radius:10px;border:1px solid rgba(var(--accent-rgb),.15);background:rgba(var(--accent-rgb),.04);backdrop-filter:blur(4px);text-align:center;min-width:120px;animation:staggerItem .5s var(--ease-smooth) both;transition:border-color 0.3s,box-shadow 0.3s}
.flow-node:hover{border-color:rgba(var(--accent-rgb),.3);box-shadow:0 4px 20px rgba(var(--accent-rgb),.1)}
.flow-node-label{font-weight:700;font-size:clamp(12px,1.4vw,16px)}.flow-node-desc{font-size:clamp(10px,1.1vw,13px);opacity:.55;line-height:1.4}
.flow-arrow{font-size:18px;opacity:.25;padding:0 clamp(6px,1vw,14px);flex-shrink:0;color:var(--accent)}

/* ── Timeline (with color progression accent→accent2) ── */
.diagram-timeline{display:flex;align-items:flex-start;gap:clamp(8px,1.5vw,16px);width:100%;max-width:90%;position:relative;padding-top:24px}
.diagram-timeline::before{content:'';position:absolute;top:30px;left:8%;right:8%;height:2px;background:linear-gradient(90deg,rgba(var(--accent-rgb),.15),rgba(var(--accent-rgb),.4),rgba(var(--accent2-rgb),.4),rgba(var(--accent2-rgb),.15));border-radius:1px}
.timeline-node{flex:1;display:flex;flex-direction:column;align-items:center;gap:10px;position:relative;animation:staggerItem .6s var(--ease-smooth) both}
.timeline-node:nth-child(1){animation-delay:.15s}.timeline-node:nth-child(2){animation-delay:.3s}.timeline-node:nth-child(3){animation-delay:.45s}.timeline-node:nth-child(4){animation-delay:.6s}
.timeline-dot{width:12px;height:12px;border-radius:50%;background:var(--accent);box-shadow:0 0 12px rgba(var(--accent-rgb),.4);z-index:1;flex-shrink:0}
.timeline-node:nth-child(2) .timeline-dot{background:linear-gradient(135deg,var(--accent),var(--accent2))}
.timeline-node:nth-child(3) .timeline-dot{background:var(--accent2);box-shadow:0 0 12px rgba(var(--accent2-rgb),.4)}
.timeline-content{text-align:center;padding:clamp(12px,2vh,20px) clamp(10px,1.5vw,16px);border-radius:10px;border:1px solid rgba(var(--accent-rgb),.12);background:rgba(var(--accent-rgb),.04);width:100%;backdrop-filter:blur(4px);transition:border-color 0.3s,box-shadow 0.3s}
.timeline-content:hover{border-color:rgba(var(--accent-rgb),.25);box-shadow:0 4px 24px rgba(var(--accent-rgb),.08)}
.timeline-label{font-weight:700;font-size:clamp(10px,1.2vw,15px);margin-bottom:4px;color:var(--text)}.timeline-desc{font-size:clamp(8px,0.95vw,12px);opacity:.6;line-height:1.5;color:var(--text-body)}

/* ── Comparison ── */
.diagram-comparison{display:grid;grid-template-columns:1fr 1fr;gap:12px;width:100%;max-width:90%}
.comparison-side{padding:12px 16px;border-radius:8px;border:1.5px solid;animation:buildFadeUp .5s var(--ease-smooth) both}
.comparison-side:first-child{animation-delay:.1s;border-color:rgba(var(--accent-rgb),.25);background:rgba(var(--accent-rgb),.04)}
.comparison-side:last-child{animation-delay:.3s;border-color:rgba(var(--accent2-rgb),.25);background:rgba(var(--accent2-rgb),.04)}
.comparison-header{font-weight:600;font-size:clamp(10px,1.3vw,15px);margin-bottom:8px;padding-bottom:6px;border-bottom:1px solid var(--border)}
.comparison-item{font-size:clamp(8px,.9vw,11px);padding:4px 0;opacity:.75}

/* ── Radial ── */
.diagram-radial{position:relative;width:280px;height:280px}
.radial-center{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);padding:14px 20px;border-radius:50%;width:90px;height:90px;display:flex;align-items:center;justify-content:center;text-align:center;font-weight:700;font-size:clamp(9px,1.1vw,13px);background:rgba(var(--accent-rgb),.12);border:2px solid rgba(var(--accent-rgb),.35);z-index:2;animation:buildScale .5s var(--ease-smooth) both}
.radial-node{position:absolute;padding:6px 12px;border-radius:6px;font-size:clamp(7px,.9vw,11px);font-weight:500;background:rgba(var(--accent-rgb),.06);border:1px solid rgba(var(--accent-rgb),.2);text-align:center;white-space:nowrap;animation:buildScale .4s var(--ease-smooth) both}
```

---

## Skill Presets (Narrative Flow)

Apply these content constraints based on user request:

| Preset | Voice | Flow |
|--------|-------|------|
| **Apple Keynote** `--style apple` | Dramatic, minimal, visionary. Theme: `midnight` | vision → problem → breakthrough → demo-points → impact → one-more-thing → close |
| **Product Launch** `--style launch` | Exciting, benefit-focused. Theme: `electric-gradient` | product → pain → solution → features → comparison → pricing → close |
| **Tech Share** `--style tech` | Technical, clear, practical. Theme: `deep-indigo` | problem → why-it-matters → approaches → solution → architecture → benchmarks → summary |
| **Product Demo** `--style demo` | Clear, practical, benefit-driven. Theme: `clean-white` | what → pain → how-it-works → features → walkthrough → before-after → summary |
| **Business Report** `--style business` | Professional, data-driven. Theme: `paper-minimal` | context → problem → solution → comparison → results → next-steps → close |
| **Product Review** `--style review` | Structured, user-centric. Theme: `clean-white` | background → problem → goals → solution → user-flow → risks → timeline → discussion |

---

## Working with Reference Material

If the user says "from file.md" or provides reference content:
1. Read the file with the Read tool
2. Extract key points — do NOT copy verbatim
3. Derive topic if not specified
4. Prioritize data, metrics, and concrete examples

---

## Output

Write to user's working directory. Default: `presentation.html`.

After writing the file, **automatically open it in the browser**:

```bash
open presentation.html    # macOS
# xdg-open presentation.html  # Linux
# start presentation.html     # Windows
```

Then tell the user:

```
Presentation ready! Press F for fullscreen.
Controls: Space/→ next, ← back, P speaker mode, G slide overview, Esc exit.
```

## Defaults

- Slides: 7–10 unless specified
- Language: match user's language
- Theme: `midnight` unless specified or implied by style preset
- Filename: `presentation.html` unless specified
