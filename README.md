<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Neuro ICU · ICP Flashcards</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;600;700&display=swap" rel="stylesheet">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
font-family: ‘DM Sans’, sans-serif;
background: #06111c;
min-height: 100vh;
display: flex;
flex-direction: column;
align-items: center;
padding: 2rem 1rem 5rem;
color: #fff;
}

/* subtle grid background */
body::after {
content: ‘’;
position: fixed; inset: 0; pointer-events: none;
background-image:
linear-gradient(rgba(16,201,154,.03) 1px, transparent 1px),
linear-gradient(90deg, rgba(16,201,154,.03) 1px, transparent 1px);
background-size: 48px 48px;
}

.page {
position: relative; z-index: 1;
width: 100%; max-width: 620px;
display: flex; flex-direction: column; align-items: center; gap: 1.25rem;
}

/* ── HEADER ── */
header { text-align: center; }
.brand {
display: inline-block;
font-size: .65rem; font-weight: 700; letter-spacing: .2em; text-transform: uppercase;
color: #10c99a; background: rgba(16,201,154,.1);
border: 1px solid rgba(16,201,154,.25); border-radius: 999px;
padding: .28rem .85rem; margin-bottom: .65rem;
}
h1 { font-size: clamp(1.4rem, 4vw, 1.9rem); font-weight: 700; color: #fff; }
.sub { font-size: .8rem; color: #3d6580; margin-top: .25rem; }

/* ── DOTS ── */
.dots { display: flex; gap: .4rem; }
.dot {
width: 34px; height: 5px; border-radius: 99px;
background: rgba(255,255,255,.1);
cursor: pointer; transition: all .3s;
}
.dot.active { background: #10c99a; width: 50px; }
.dot.done   { background: rgba(16,201,154,.3); }

/* ══════════════════════════════
THE CARD
══════════════════════════════ */
.scene { width: 100%; perspective: 1200px; }

.card-inner {
width: 100%; height: 420px;
position: relative;
transform-style: preserve-3d;
transition: transform .55s cubic-bezier(.4, 0, .2, 1);
}
.card-inner.flipped { transform: rotateY(180deg); }

.face {
position: absolute; inset: 0;
border-radius: 20px;
backface-visibility: hidden;
-webkit-backface-visibility: hidden;
display: flex; flex-direction: column;
}

/* ━━━ FRONT — question only ━━━ */
.face-front {
background: #0e2035;
border: 1px solid rgba(255,255,255,.08);
box-shadow: 0 20px 56px rgba(0,0,0,.55);
}

.front-header {
display: flex; justify-content: space-between; align-items: center;
padding: 1rem 1.3rem .85rem;
border-bottom: 1px solid rgba(255,255,255,.06);
flex-shrink: 0;
}
.cat-pill {
font-size: .62rem; font-weight: 700; letter-spacing: .1em; text-transform: uppercase;
padding: .24rem .7rem; border-radius: 999px;
background: rgba(16,201,154,.12); color: #10c99a;
border: 1px solid rgba(16,201,154,.22);
}
.card-num { font-size: .7rem; color: #3d6580; }

/* Question — centred, big, bold, nothing else */
.front-body {
flex: 1;
display: flex; align-items: center; justify-content: center;
padding: 2rem 2rem;
}
.question {
font-size: clamp(1.15rem, 3vw, 1.5rem);
font-weight: 700;
line-height: 1.5;
color: #ffffff;
text-align: center;
}

/* ━━━ BACK — bold bullets + optional illustration ━━━ */
.face-back {
transform: rotateY(180deg);
background: #ffffff;
box-shadow: 0 20px 56px rgba(0,0,0,.55);
overflow: hidden;
}

.back-header {
flex-shrink: 0;
display: flex; justify-content: space-between; align-items: center;
padding: .85rem 1.3rem;
background: #0a8f6d;
}
.back-label { font-size: .62rem; font-weight: 700; letter-spacing: .15em; text-transform: uppercase; color: rgba(255,255,255,.85); }
.back-cat   { font-size: .62rem; font-weight: 600; letter-spacing: .06em; text-transform: uppercase; color: rgba(255,255,255,.5); }

/* illustration strip */
.back-art {
flex-shrink: 0;
display: flex; align-items: center; justify-content: center;
padding: .6rem 1.3rem .3rem;
background: #f0f8ff;
border-bottom: 1px solid #d8ecf8;
}
.back-art svg { max-height: 90px; width: 100%; }

/* answers */
.back-answers {
flex: 1; min-height: 0;
overflow-y: auto;
padding: 1.1rem 1.3rem 1rem;
scrollbar-width: thin; scrollbar-color: #c2dded transparent;
}
.back-answers::-webkit-scrollbar { width: 4px; }
.back-answers::-webkit-scrollbar-thumb { background: #c2dded; border-radius: 99px; }

/* each bullet item */
.bullet {
display: flex; gap: .75rem; align-items: flex-start;
padding: .65rem 0;
border-bottom: 1px solid #e8f3fb;
opacity: 0;
transform: translateX(-18px);
/* animation set by JS */
}
.bullet:last-child { border-bottom: none; }

.bullet-dot {
flex-shrink: 0;
width: 9px; height: 9px; border-radius: 50%;
background: #0a8f6d;
margin-top: .45rem;
}
.bullet-text {
font-size: clamp(.92rem, 2.5vw, 1.05rem);
font-weight: 700;
line-height: 1.5;
color: #061828;
}

/* animated state */
.bullet.show {
animation: bullet-in .4s cubic-bezier(.23,1,.32,1) forwards;
}
@keyframes bullet-in {
to { opacity: 1; transform: translateX(0); }
}

.back-footer {
flex-shrink: 0;
padding: .45rem 1.3rem;
background: #e8f4fb; border-top: 1px solid #d0e8f5;
font-size: .66rem; color: #7aa0bc; text-align: right;
}

/* ── FLIP BUTTON ── */
.flip-btn {
width: 100%; padding: .9rem 1rem; border-radius: 14px; border: none;
font-family: ‘DM Sans’, sans-serif; font-size: .95rem; font-weight: 700;
cursor: pointer; transition: all .18s;
display: flex; align-items: center; justify-content: center; gap: .5rem;
}
.flip-btn.q-side {
background: #10c99a; color: #06111c;
}
.flip-btn.q-side:hover { background: #13e2a8; transform: translateY(-2px); box-shadow: 0 8px 24px rgba(16,201,154,.35); }
.flip-btn.a-side {
background: rgba(255,255,255,.08); color: #fff;
border: 1.5px solid rgba(255,255,255,.14);
}
.flip-btn.a-side:hover { background: rgba(255,255,255,.14); transform: translateY(-2px); }
.flip-btn:active { transform: translateY(0) !important; }

/* ── MARK BUTTONS ── */
.mark-row { display: none; gap: .6rem; width: 100%; }
.mark-row.show { display: flex; }
.mark-btn {
flex: 1; padding: .65rem; border-radius: 12px;
font-family: ‘DM Sans’, sans-serif; font-size: .82rem; font-weight: 700;
cursor: pointer; transition: all .18s;
display: flex; align-items: center; justify-content: center; gap: .35rem;
}
.btn-got { background: rgba(16,201,154,.12); border: 1.5px solid rgba(16,201,154,.35); color: #10c99a; }
.btn-got:hover { background: rgba(16,201,154,.25); }
.btn-rev { background: rgba(248,113,113,.1); border: 1.5px solid rgba(248,113,113,.3); color: #f87171; }
.btn-rev:hover { background: rgba(248,113,113,.2); }

/* ── NAV ── */
.nav-row { display: flex; gap: .5rem; width: 100%; }
.nav-btn {
flex: 1; padding: .6rem; border-radius: 12px;
border: 1.5px solid rgba(255,255,255,.1); background: rgba(255,255,255,.05); color: #fff;
font-family: ‘DM Sans’, sans-serif; font-size: .82rem; font-weight: 700;
cursor: pointer; transition: all .18s;
}
.nav-btn:hover { background: rgba(255,255,255,.12); transform: translateY(-1px); }
.nav-btn:active { transform: translateY(0); }
.nav-btn:disabled { opacity: .25; cursor: default; transform: none; }

.kb {
font-size: .68rem; color: rgba(255,255,255,.2);
display: flex; gap: 1rem; flex-wrap: wrap; justify-content: center;
}
.kb kbd {
border: 1px solid rgba(255,255,255,.12); border-radius: 4px;
padding: .1rem .3rem; font-size: .63rem; color: rgba(255,255,255,.28);
font-family: ‘DM Sans’, sans-serif;
}

@media (max-width: 480px) {
.card-inner { height: 450px; }
.front-body { padding: 1.5rem 1.25rem; }
}
</style>

</head>
<body>
<div class="page">

  <header>
    <div class="brand">Neuro ICU · ICP Management</div>
    <h1>Pilot Flashcards</h1>
    <p class="sub">5 cards · Tap "Reveal Answer" to flip</p>
  </header>

  <div class="dots" id="dots"></div>

  <!-- Card -->

  <div class="scene">
    <div class="card-inner" id="card-inner">

```
  <!-- FRONT: question only -->
  <div class="face face-front">
    <div class="front-header">
      <span class="cat-pill" id="f-cat"></span>
      <span class="card-num"  id="f-num"></span>
    </div>
    <div class="front-body">
      <div class="question" id="f-question"></div>
    </div>
  </div>

  <!-- BACK: illustration + bold bullets -->
  <div class="face face-back">
    <div class="back-header">
      <span class="back-label">Answer</span>
      <span class="back-cat" id="b-cat"></span>
    </div>
    <div class="back-art" id="b-art"></div>
    <div class="back-answers" id="b-answers"></div>
    <div class="back-footer" id="b-footer"></div>
  </div>

</div>
```

  </div>

  <!-- Flip button -->

  <button class="flip-btn q-side" id="flip-btn" onclick="doFlip()">
    ↩ <span id="flip-lbl">Reveal Answer</span>
  </button>

  <!-- Mark buttons (after flip) -->

  <div class="mark-row" id="mark-row">
    <button class="mark-btn btn-got" onclick="markCard(true)">✓ Got it</button>
    <button class="mark-btn btn-rev" onclick="markCard(false)">✗ Needs review</button>
  </div>

  <!-- Navigation -->

  <div class="nav-row">
    <button class="nav-btn" id="btn-prev" onclick="go(-1)">← Previous</button>
    <button class="nav-btn" id="btn-next" onclick="go(1)">Next →</button>
  </div>

  <div class="kb">
    <span><kbd>Space</kbd> flip</span>
    <span><kbd>←</kbd> <kbd>→</kbd> navigate</span>
    <span><kbd>1</kbd> got it &nbsp; <kbd>2</kbd> needs review</span>
  </div>

</div>

<script>
/* ═══════ SVG ILLUSTRATIONS (answer side only) ═══════ */
const ART = {

  gauge: `<svg viewBox="0 0 360 88" xmlns="http://www.w3.org/2000/svg">
    <!-- colour scale bar -->
    <rect x="20" y="30" width="80" height="22" rx="6" fill="#10c99a"/>
    <rect x="110" y="30" width="60" height="22" rx="6" fill="#fbbf24"/>
    <rect x="180" y="30" width="80" height="22" rx="6" fill="#f97316"/>
    <rect x="270" y="30" width="70" height="22" rx="6" fill="#ef4444"/>
    <text x="60"  y="45" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="11" fill="white" font-weight="700">7–15</text>
    <text x="140" y="45" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="11" fill="white" font-weight="700">16–20</text>
    <text x="220" y="45" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="11" fill="white" font-weight="700">21–40</text>
    <text x="305" y="45" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="11" fill="white" font-weight="700">&gt; 40</text>
    <text x="60"  y="22" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9.5" fill="#0a8f6d" font-weight="600">Normal</text>
    <text x="140" y="22" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9.5" fill="#d97706" font-weight="600">Mild</text>
    <text x="220" y="22" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9.5" fill="#c2410c" font-weight="600">Moderate</text>
    <text x="305" y="22" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9.5" fill="#b91c1c" font-weight="600">Severe</text>
    <text x="60"  y="66" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8.5" fill="#065f46">mmHg</text>
    <text x="140" y="66" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8.5" fill="#78350f">mmHg</text>
    <text x="220" y="66" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8.5" fill="#7c2d12">mmHg</text>
    <text x="305" y="66" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8.5" fill="#7f1d1d">mmHg</text>
    <!-- treat marker -->
    <polygon points="180,58 174,72 186,72" fill="#1d4ed8"/>
    <text x="180" y="83" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8.5" fill="#1d4ed8" font-weight="700">TREAT ↑ 20 mmHg</text>
  </svg>`,

  cpp: `<svg viewBox="0 0 340 86" xmlns="http://www.w3.org/2000/svg">
    <!-- CPP -->
    <rect x="4"   y="8" width="82" height="70" rx="12" fill="#0a8f6d"/>
    <text x="45"  y="38" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="17" fill="white" font-weight="700">CPP</text>
    <text x="45"  y="54" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="11" fill="rgba(255,255,255,.75)">60–70 mmHg</text>
    <text x="45"  y="70" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8.5" fill="rgba(255,255,255,.55)">target</text>
    <!-- = -->
    <text x="102" y="53" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="26" fill="#334155" font-weight="300">=</text>
    <!-- MAP -->
    <rect x="118" y="8" width="82" height="70" rx="12" fill="#1d4ed8"/>
    <text x="159" y="38" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="17" fill="white" font-weight="700">MAP</text>
    <text x="159" y="54" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="11" fill="rgba(255,255,255,.75)">Mean Arterial</text>
    <text x="159" y="70" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8.5" fill="rgba(255,255,255,.55)">Pressure</text>
    <!-- - -->
    <text x="216" y="53" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="26" fill="#334155" font-weight="300">−</text>
    <!-- ICP -->
    <rect x="228" y="8" width="82" height="70" rx="12" fill="#dc2626"/>
    <text x="269" y="38" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="17" fill="white" font-weight="700">ICP</text>
    <text x="269" y="54" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="11" fill="rgba(255,255,255,.75)">&lt; 20 mmHg</text>
    <text x="269" y="70" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8.5" fill="rgba(255,255,255,.55)">aim</text>
  </svg>`,

  steps: `<svg viewBox="0 0 340 86" xmlns="http://www.w3.org/2000/svg">
    <!-- 6 steps in a row -->
    <rect x="0"   y="18" width="50" height="50" rx="10" fill="#0a8f6d"/>
    <text x="25"  y="40" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="14" fill="white" font-weight="700">1</text>
    <text x="25"  y="54" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">HOB 30°</text>
    <text x="25"  y="63" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">Midline</text>
    <rect x="58"  y="18" width="50" height="50" rx="10" fill="#0d6eaf"/>
    <text x="83"  y="40" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="14" fill="white" font-weight="700">2</text>
    <text x="83"  y="54" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">Analgesia</text>
    <text x="83"  y="63" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">Sedation</text>
    <rect x="116" y="18" width="50" height="50" rx="10" fill="#0d6eaf"/>
    <text x="141" y="40" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="14" fill="white" font-weight="700">3</text>
    <text x="141" y="54" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">O₂ &amp; CO₂</text>
    <text x="141" y="63" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">targets</text>
    <rect x="174" y="18" width="50" height="50" rx="10" fill="#b45309"/>
    <text x="199" y="40" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="14" fill="white" font-weight="700">4</text>
    <text x="199" y="54" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">Normo-</text>
    <text x="199" y="63" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">thermia</text>
    <rect x="232" y="18" width="50" height="50" rx="10" fill="#b45309"/>
    <text x="257" y="40" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="14" fill="white" font-weight="700">5</text>
    <text x="257" y="54" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">EVD</text>
    <text x="257" y="63" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">Drainage</text>
    <rect x="290" y="18" width="50" height="50" rx="10" fill="#b91c1c"/>
    <text x="315" y="40" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="14" fill="white" font-weight="700">6</text>
    <text x="315" y="54" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">Osmo-</text>
    <text x="315" y="63" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7.5" fill="rgba(255,255,255,.85)">therapy</text>
    <text x="170" y="12" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8.5" fill="#64748b" letter-spacing="1">FIRST-TIER STEPS</text>
  </svg>`,

  skull: `<svg viewBox="0 0 300 86" xmlns="http://www.w3.org/2000/svg">
    <defs><clipPath id="sc"><ellipse cx="150" cy="42" rx="80" ry="38"/></clipPath></defs>
    <!-- skull border -->
    <ellipse cx="150" cy="42" rx="88" ry="44" fill="none" stroke="#94a3b8" stroke-width="7"/>
    <ellipse cx="150" cy="42" rx="80" ry="38" fill="#f0f8ff"/>
    <!-- brain 80 -->
    <ellipse cx="150" cy="40" rx="74" ry="34" fill="#d1fae5" stroke="#0a8f6d" stroke-width="1" clip-path="url(#sc)"/>
    <text x="150" y="36" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="14" fill="#065f46" font-weight="700">80%</text>
    <text x="150" y="50" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="9"  fill="#065f46" font-weight="600">BRAIN</text>
    <!-- blood right -->
    <path d="M224,20 A80,38 0 0,1 224,64 L150,42 Z" fill="#fee2e2" stroke="#dc2626" stroke-width="1" clip-path="url(#sc)"/>
    <text x="210" y="40" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="10" fill="#b91c1c" font-weight="700">10%</text>
    <text x="210" y="53" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8"  fill="#b91c1c">BLOOD</text>
    <!-- csf left -->
    <path d="M76,20  A80,38 0 0,0 76,64  L150,42 Z" fill="#dbeafe" stroke="#1d4ed8" stroke-width="1" clip-path="url(#sc)"/>
    <text x="90"  y="40" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="10" fill="#1e40af" font-weight="700">10%</text>
    <text x="90"  y="53" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8"  fill="#1e40af">CSF</text>
    <!-- label -->
    <text x="150" y="80" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8.5" fill="#475569" letter-spacing="1.5">RIGID FIXED VOLUME</text>
  </svg>`,

  monitor: `<svg viewBox="0 0 320 86" xmlns="http://www.w3.org/2000/svg">
    <!-- monitor -->
    <rect x="50"  y="4"  width="140" height="68" rx="8" fill="#0f172a" stroke="#0a8f6d" stroke-width="1.5"/>
    <polyline points="58,42 68,42 72,26 77,58 82,20 87,58 92,42 100,42 104,36 108,50 112,30 116,50 120,42 130,42 134,36 138,46 140,42 148,42" fill="none" stroke="#10c99a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
    <text x="170" y="30" font-family="DM Sans,sans-serif" font-size="8" fill="#10c99a">ICP</text>
    <text x="163" y="48" font-family="DM Sans,sans-serif" font-size="22" fill="#10c99a" font-weight="700">14</text>
    <text x="165" y="60" font-family="DM Sans,sans-serif" font-size="8" fill="rgba(16,201,154,.6)">mmHg</text>
    <!-- indications -->
    <rect x="198" y="4"  width="56" height="30" rx="6" fill="#fee2e2" stroke="#fca5a5" stroke-width="1"/>
    <text x="226" y="17" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8" fill="#b91c1c" font-weight="700">GCS ≤ 8</text>
    <text x="226" y="28" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7" fill="#dc2626">Severe TBI</text>
    <rect x="260" y="4"  width="56" height="30" rx="6" fill="#fef3c7" stroke="#fcd34d" stroke-width="1"/>
    <text x="288" y="17" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8" fill="#92400e" font-weight="700">SAH III–V</text>
    <text x="288" y="28" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7" fill="#b45309">Hunt &amp; Hess</text>
    <rect x="198" y="40" width="56" height="30" rx="6" fill="#dbeafe" stroke="#93c5fd" stroke-width="1"/>
    <text x="226" y="53" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8" fill="#1e40af" font-weight="700">Hydro-</text>
    <text x="226" y="64" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7" fill="#1d4ed8">cephalus</text>
    <rect x="260" y="40" width="56" height="30" rx="6" fill="#d1fae5" stroke="#6ee7b7" stroke-width="1"/>
    <text x="288" y="53" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="8" fill="#065f46" font-weight="700">Large</text>
    <text x="288" y="64" text-anchor="middle" font-family="DM Sans,sans-serif" font-size="7" fill="#047857">Stroke</text>
  </svg>`,
};

/* ═══════ CARD DATA ═══════ */
const CARDS = [
  {
    cat: "ICP Management",
    q:   "What is the normal range for Intracranial Pressure (ICP)?",
    art: "gauge",
    answers: [
      "Normal ICP in adults is 7 – 15 mmHg at rest.",
      "Mild elevation: 16 – 20 mmHg.",
      "Moderate elevation: 21 – 40 mmHg.",
      "Severe elevation: greater than 40 mmHg.",
      "Treatment is started when ICP is above 20 – 22 mmHg for more than 5 minutes.",
    ],
  },
  {
    cat: "ICP Management",
    q:   "What is Cerebral Perfusion Pressure (CPP) and what is the target?",
    art: "cpp",
    answers: [
      "CPP = MAP minus ICP.",
      "Target CPP is 60 – 70 mmHg in most neuro / TBI patients.",
      "Below 60 mmHg = risk of cerebral ischaemia.",
      "Above 70 mmHg = risk of worsening vasogenic oedema.",
    ],
  },
  {
    cat: "ICP Management",
    q:   "What are the first-tier interventions for raised ICP?",
    art: "steps",
    answers: [
      "Head of bed at 30° with head in midline.",
      "Analgesia and sedation to reduce agitation and Valsalva.",
      "Maintain SpO₂ above 94% and PaCO₂ at 4.5 – 5.0 kPa.",
      "Treat pyrexia — aim for normothermia.",
      "CSF drainage via EVD if one is in situ.",
      "Osmotherapy: Mannitol or Hypertonic Saline.",
    ],
  },
  {
    cat: "ICP Management",
    q:   "What is the Monro-Kellie Doctrine?",
    art: "skull",
    answers: [
      "The skull is a rigid box with a fixed total volume.",
      "It contains: Brain (80%), Blood (10%), and CSF (10%).",
      "If one component increases, another must decrease to keep ICP stable.",
      "Once compensatory mechanisms are exhausted, small volume changes cause exponential ICP rises.",
    ],
  },
  {
    cat: "ICP Management",
    q:   "What are the indications for ICP monitoring?",
    art: "monitor",
    answers: [
      "Severe TBI with GCS ≤ 8 after resuscitation, plus an abnormal CT scan.",
      "Severe TBI with normal CT plus two or more of: age over 40, SBP below 90, motor posturing.",
      "Post-operative craniotomy at the surgeon's discretion.",
      "Large territory stroke with significant cerebral oedema.",
      "Hydrocephalus.",
      "SAH grade III – V on the Hunt and Hess scale.",
    ],
  },
];

/* ═══════ STATE ═══════ */
let idx = 0, flipped = false;
const known = new Set();
let animTimer = null;

/* ═══════ RENDER ═══════ */
function render() {
  const c = CARDS[idx];

  // dots
  const dotsEl = document.getElementById('dots');
  dotsEl.innerHTML = '';
  CARDS.forEach((card, i) => {
    const d = document.createElement('div');
    d.className = 'dot' + (i === idx ? ' active' : '') + (known.has(card.q) ? ' done' : '');
    d.onclick = e => { e.stopPropagation(); go_to(i); };
    dotsEl.appendChild(d);
  });

  // card flip state
  document.getElementById('card-inner').classList.toggle('flipped', flipped);

  // FRONT
  document.getElementById('f-cat').textContent      = c.cat;
  document.getElementById('f-num').textContent      = (idx + 1) + ' / ' + CARDS.length;
  document.getElementById('f-question').textContent = c.q;

  // BACK — populate and reset bullets
  document.getElementById('b-cat').textContent    = c.cat;
  document.getElementById('b-art').innerHTML      = ART[c.art] || '';
  document.getElementById('b-footer').textContent = 'Card ' + (idx + 1) + ' of ' + CARDS.length + '  ·  Neuro ICU  ·  ICP Management';

  const answersEl = document.getElementById('b-answers');
  answersEl.innerHTML = '';
  c.answers.forEach(txt => {
    const row = document.createElement('div');
    row.className = 'bullet';                  // starts invisible
    row.innerHTML =
      '<div class="bullet-dot"></div>' +
      '<div class="bullet-text">' + txt + '</div>';
    answersEl.appendChild(row);
  });

  // trigger bullet animation if already flipped
  clearTimeout(animTimer);
  if (flipped) scheduleBullets();

  // flip button
  updateFlipBtn();

  // mark row
  document.getElementById('mark-row').classList.toggle('show', flipped);

  // nav
  document.getElementById('btn-prev').disabled = idx === 0;
  document.getElementById('btn-next').disabled = idx === CARDS.length - 1;
}

function updateFlipBtn() {
  const btn = document.getElementById('flip-btn');
  const lbl = document.getElementById('flip-lbl');
  btn.className = flipped ? 'flip-btn a-side' : 'flip-btn q-side';
  lbl.textContent = flipped ? 'Back to Question' : 'Reveal Answer';
}

/* stagger bullets in one by one after flip completes */
function scheduleBullets() {
  animTimer = setTimeout(() => {
    const bullets = document.querySelectorAll('.bullet');
    bullets.forEach((b, i) => {
      setTimeout(() => {
        b.style.animationDelay = '0ms';
        b.classList.add('show');
      }, i * 120);
    });
  }, 580);   // wait for card flip to finish
}

/* ═══════ ACTIONS ═══════ */
function doFlip() {
  flipped = !flipped;
  document.getElementById('card-inner').classList.toggle('flipped', flipped);
  updateFlipBtn();
  document.getElementById('mark-row').classList.toggle('show', flipped);

  // reset bullets then re-animate if showing answer
  clearTimeout(animTimer);
  document.querySelectorAll('.bullet').forEach(b => b.classList.remove('show'));
  if (flipped) scheduleBullets();
}

function go(dir) {
  const n = idx + dir;
  if (n < 0 || n >= CARDS.length) return;
  idx = n; flipped = false; render();
}

function go_to(i) {
  idx = i; flipped = false; render();
}

function markCard(isKnown) {
  isKnown ? known.add(CARDS[idx].q) : known.delete(CARDS[idx].q);
  if (idx < CARDS.length - 1) go(1);
  else render();
}

document.addEventListener('keydown', e => {
  if (['INPUT', 'TEXTAREA'].includes(document.activeElement.tagName)) return;
  if (e.key === ' ' || e.key === 'Enter') { e.preventDefault(); doFlip(); }
  if (e.key === 'ArrowRight') go(1);
  if (e.key === 'ArrowLeft')  go(-1);
  if (e.key === '1' && flipped) markCard(true);
  if (e.key === '2' && flipped) markCard(false);
});

render();
</script>

</body>
</html>
