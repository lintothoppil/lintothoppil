<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>lintothoppil · GitHub Portfolio</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg: #050b14;
    --surface: #0d1b2a;
    --surface2: #112036;
    --accent: #00f5c4;
    --accent2: #0077ff;
    --accent3: #ff4d6d;
    --text: #e8f0fe;
    --muted: #607d9a;
    --border: rgba(0,245,196,0.15);
    --glow: 0 0 30px rgba(0,245,196,0.25);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    overflow-x: hidden;
    cursor: none;
  }

  /* Custom cursor */
  #cursor {
    position: fixed; width: 12px; height: 12px;
    background: var(--accent); border-radius: 50%;
    pointer-events: none; z-index: 9999;
    transform: translate(-50%, -50%);
    transition: transform 0.1s, opacity 0.2s;
    mix-blend-mode: screen;
  }
  #cursor-ring {
    position: fixed; width: 36px; height: 36px;
    border: 1.5px solid var(--accent);
    border-radius: 50%; pointer-events: none; z-index: 9998;
    transform: translate(-50%, -50%);
    transition: transform 0.18s ease, width 0.2s, height 0.2s, opacity 0.2s;
    opacity: 0.5;
  }

  /* Starfield canvas */
  #starfield {
    position: fixed; top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0; pointer-events: none;
  }

  /* Grid overlay */
  .grid-overlay {
    position: fixed; top: 0; left: 0;
    width: 100%; height: 100%;
    background-image:
      linear-gradient(rgba(0,245,196,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,245,196,0.03) 1px, transparent 1px);
    background-size: 60px 60px;
    z-index: 0; pointer-events: none;
  }

  main { position: relative; z-index: 1; max-width: 1000px; margin: 0 auto; padding: 0 24px 80px; }

  /* ── HERO ── */
  .hero {
    min-height: 100vh; display: flex;
    flex-direction: column; justify-content: center;
    align-items: flex-start; padding: 60px 0 40px;
    position: relative;
  }
  .hero-tag {
    font-family: 'Space Mono', monospace;
    font-size: 11px; letter-spacing: 3px;
    color: var(--accent); text-transform: uppercase;
    border: 1px solid var(--border);
    padding: 6px 14px; border-radius: 2px;
    margin-bottom: 28px;
    opacity: 0; animation: fadeSlideUp 0.6s 0.2s forwards;
    display: flex; align-items: center; gap: 8px;
  }
  .hero-tag::before {
    content: ''; width: 6px; height: 6px;
    background: var(--accent); border-radius: 50%;
    animation: pulse 1.5s infinite;
  }
  @keyframes pulse {
    0%,100%{opacity:1;transform:scale(1)}
    50%{opacity:0.4;transform:scale(0.7)}
  }

  .hero-name {
    font-size: clamp(56px, 10vw, 110px);
    font-weight: 800; line-height: 0.9;
    letter-spacing: -3px; margin-bottom: 20px;
    opacity: 0; animation: fadeSlideUp 0.7s 0.4s forwards;
  }
  .hero-name .line1 { display: block; color: var(--text); }
  .hero-name .line2 {
    display: block;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero-sub {
    font-family: 'Space Mono', monospace;
    font-size: 13px; color: var(--muted);
    line-height: 1.7; max-width: 460px;
    margin-bottom: 36px;
    opacity: 0; animation: fadeSlideUp 0.7s 0.6s forwards;
  }

  .hero-links {
    display: flex; gap: 14px; flex-wrap: wrap;
    opacity: 0; animation: fadeSlideUp 0.7s 0.8s forwards;
  }
  .btn {
    font-family: 'Space Mono', monospace;
    font-size: 11px; letter-spacing: 2px;
    text-transform: uppercase; text-decoration: none;
    padding: 12px 24px; border-radius: 2px;
    cursor: pointer; transition: all 0.25s;
    display: inline-flex; align-items: center; gap: 8px;
  }
  .btn-primary {
    background: var(--accent); color: var(--bg);
    font-weight: 700;
  }
  .btn-primary:hover {
    background: #fff; transform: translateY(-2px);
    box-shadow: 0 8px 30px rgba(0,245,196,0.4);
  }
  .btn-ghost {
    border: 1px solid var(--border); color: var(--text);
  }
  .btn-ghost:hover {
    border-color: var(--accent); color: var(--accent);
    transform: translateY(-2px); box-shadow: var(--glow);
  }

  /* Scroll indicator */
  .scroll-hint {
    position: absolute; bottom: 40px; left: 0;
    font-family: 'Space Mono', monospace;
    font-size: 10px; color: var(--muted); letter-spacing: 3px;
    display: flex; align-items: center; gap: 12px;
    opacity: 0; animation: fadeSlideUp 0.7s 1.2s forwards;
  }
  .scroll-line {
    width: 40px; height: 1px; background: var(--muted);
    position: relative; overflow: hidden;
  }
  .scroll-line::after {
    content: ''; position: absolute; top: 0; left: -100%;
    width: 100%; height: 1px; background: var(--accent);
    animation: scanLine 2s 1.5s infinite;
  }
  @keyframes scanLine { to { left: 100%; } }

  /* ── STATS ── */
  .stats-row {
    display: grid; grid-template-columns: repeat(3, 1fr);
    gap: 1px; background: var(--border);
    border: 1px solid var(--border);
    border-radius: 4px; margin-bottom: 80px;
    opacity: 0; animation: fadeSlideUp 0.7s 1s forwards;
  }
  .stat {
    background: var(--surface); padding: 28px 24px;
    text-align: center; position: relative; overflow: hidden;
    transition: background 0.3s;
  }
  .stat:hover { background: var(--surface2); }
  .stat::after {
    content: ''; position: absolute; bottom: 0; left: 0;
    width: 0; height: 2px; background: var(--accent);
    transition: width 0.4s;
  }
  .stat:hover::after { width: 100%; }
  .stat-num {
    font-size: 40px; font-weight: 800; color: var(--accent);
    line-height: 1; display: block;
  }
  .stat-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px; color: var(--muted);
    letter-spacing: 2px; text-transform: uppercase;
    margin-top: 6px; display: block;
  }

  /* ── SECTIONS ── */
  .section-header {
    display: flex; align-items: center; gap: 16px;
    margin-bottom: 32px;
  }
  .section-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px; color: var(--accent);
    letter-spacing: 4px; text-transform: uppercase;
  }
  .section-line {
    flex: 1; height: 1px;
    background: linear-gradient(90deg, var(--border), transparent);
  }

  /* ── REPOS ── */
  .repos { margin-bottom: 80px; }
  .repos-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 16px;
  }
  .repo-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 4px; padding: 24px;
    text-decoration: none; color: inherit;
    display: flex; flex-direction: column; gap: 12px;
    position: relative; overflow: hidden;
    transition: transform 0.3s, border-color 0.3s, box-shadow 0.3s;
    opacity: 0; transform: translateY(20px);
  }
  .repo-card.visible {
    animation: fadeSlideUp 0.5s forwards;
  }
  .repo-card::before {
    content: ''; position: absolute;
    top: 0; left: 0; right: 0; height: 2px;
    background: linear-gradient(90deg, transparent, var(--accent), transparent);
    transform: scaleX(0); transition: transform 0.4s;
  }
  .repo-card:hover::before { transform: scaleX(1); }
  .repo-card:hover {
    transform: translateY(-4px);
    border-color: rgba(0,245,196,0.4);
    box-shadow: 0 12px 40px rgba(0,245,196,0.1);
  }
  .repo-icon {
    width: 36px; height: 36px; border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px;
  }
  .repo-name {
    font-size: 15px; font-weight: 700; color: var(--text);
  }
  .repo-desc {
    font-family: 'Space Mono', monospace;
    font-size: 11px; color: var(--muted); line-height: 1.6;
    flex: 1;
  }
  .repo-footer {
    display: flex; align-items: center; justify-content: space-between;
  }
  .repo-lang {
    font-family: 'Space Mono', monospace;
    font-size: 10px; letter-spacing: 1px;
    display: flex; align-items: center; gap: 6px;
  }
  .lang-dot {
    width: 8px; height: 8px; border-radius: 50%;
  }
  .repo-stars {
    font-family: 'Space Mono', monospace;
    font-size: 10px; color: var(--muted);
    display: flex; align-items: center; gap: 4px;
  }

  /* Language colors */
  .lang-c { color: #555599; }
  .dot-c { background: #555599; }
  .lang-python { color: #3572A5; }
  .dot-python { background: #3572A5; }
  .lang-html { color: #e34c26; }
  .dot-html { background: #e34c26; }
  .lang-java { color: #b07219; }
  .dot-java { background: #b07219; }
  .lang-shell { color: #89e051; }
  .dot-shell { background: #89e051; }

  /* ── SKILLS ── */
  .skills { margin-bottom: 80px; }
  .skills-grid {
    display: grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
    gap: 12px;
  }
  .skill-chip {
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 3px; padding: 16px;
    text-align: center; transition: all 0.3s;
    opacity: 0; animation: none;
  }
  .skill-chip.visible { animation: fadeSlideUp 0.4s forwards; }
  .skill-chip:hover {
    border-color: var(--accent2);
    box-shadow: 0 0 20px rgba(0,119,255,0.2);
    transform: translateY(-3px);
  }
  .skill-icon { font-size: 26px; margin-bottom: 8px; display: block; }
  .skill-name {
    font-family: 'Space Mono', monospace;
    font-size: 10px; letter-spacing: 2px;
    text-transform: uppercase; color: var(--muted);
  }

  /* ── TERMINAL ── */
  .terminal {
    background: #070d16; border: 1px solid var(--border);
    border-radius: 6px; overflow: hidden; margin-bottom: 80px;
    opacity: 0; animation: fadeSlideUp 0.6s 0.2s both;
  }
  .terminal-bar {
    background: var(--surface2); padding: 10px 16px;
    display: flex; align-items: center; gap: 8px;
  }
  .dot-red { width:10px;height:10px;border-radius:50%;background:#ff5f57; }
  .dot-yellow { width:10px;height:10px;border-radius:50%;background:#febc2e; }
  .dot-green { width:10px;height:10px;border-radius:50%;background:#28c840; }
  .terminal-title {
    font-family: 'Space Mono', monospace;
    font-size: 11px; color: var(--muted); margin-left: 8px;
  }
  .terminal-body { padding: 24px; font-family: 'Space Mono', monospace; font-size: 12px; line-height: 2; }
  .t-prompt { color: var(--accent); }
  .t-cmd { color: var(--text); }
  .t-out { color: var(--muted); }
  .t-hi { color: var(--accent2); }
  .t-err { color: var(--accent3); }
  .cursor-blink {
    display: inline-block; width: 8px; height: 14px;
    background: var(--accent); vertical-align: middle;
    animation: blink 1s step-end infinite;
  }
  @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }

  /* ── FOOTER ── */
  footer {
    border-top: 1px solid var(--border);
    padding: 40px 0; text-align: center;
    font-family: 'Space Mono', monospace;
    font-size: 11px; color: var(--muted);
    position: relative; z-index: 1;
  }
  footer a { color: var(--accent); text-decoration: none; }
  footer a:hover { text-decoration: underline; }

  /* ── ANIMATIONS ── */
  @keyframes fadeSlideUp {
    from { opacity:0; transform:translateY(20px); }
    to { opacity:1; transform:translateY(0); }
  }

  /* Floating orbs */
  .orb {
    position: fixed; border-radius: 50%;
    filter: blur(80px); pointer-events: none; z-index: 0;
    animation: floatOrb 12s ease-in-out infinite;
  }
  .orb1 {
    width: 400px; height: 400px;
    background: rgba(0,245,196,0.06);
    top: -100px; right: -100px;
    animation-duration: 14s;
  }
  .orb2 {
    width: 300px; height: 300px;
    background: rgba(0,119,255,0.07);
    bottom: 200px; left: -100px;
    animation-duration: 18s; animation-delay: -6s;
  }
  @keyframes floatOrb {
    0%,100%{transform:translate(0,0)}
    33%{transform:translate(30px,-40px)}
    66%{transform:translate(-20px,30px)}
  }

  /* Section reveal */
  .reveal { opacity:0; transform:translateY(30px); transition: opacity 0.6s, transform 0.6s; }
  .reveal.in-view { opacity:1; transform:translateY(0); }
</style>
</head>
<body>

<div id="cursor"></div>
<div id="cursor-ring"></div>
<canvas id="starfield"></canvas>
<div class="grid-overlay"></div>
<div class="orb orb1"></div>
<div class="orb orb2"></div>

<main>
  <!-- HERO -->
  <section class="hero">
    <div class="hero-tag">GitHub Profile · Open Source</div>
    <h1 class="hero-name">
      <span class="line1">LINTO</span>
      <span class="line2">THOPPIL</span>
    </h1>
    <p class="hero-sub">
      Developer · Explorer · Builder<br/>
      Writing code in C, Python, Java, Shell & Web.<br/>
      12 repositories · Pro member
    </p>
    <div class="hero-links">
      <a class="btn btn-primary" href="https://github.com/lintothoppil" target="_blank">
        <svg width="14" height="14" fill="currentColor" viewBox="0 0 16 16"><path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.012 8.012 0 0 0 16 8c0-4.42-3.58-8-8-8z"/></svg>
        View GitHub
      </a>
      <a class="btn btn-ghost" href="https://github.com/lintothoppil?tab=repositories" target="_blank">
        All Repos →
      </a>
    </div>
    <div class="scroll-hint">
      <div class="scroll-line"></div>
      SCROLL TO EXPLORE
    </div>
  </section>

  <!-- STATS -->
  <div class="stats-row reveal">
    <div class="stat">
      <span class="stat-num" data-target="12">0</span>
      <span class="stat-label">Repositories</span>
    </div>
    <div class="stat">
      <span class="stat-num" data-target="9">0</span>
      <span class="stat-label">Stars Earned</span>
    </div>
    <div class="stat">
      <span class="stat-num" data-target="5">0</span>
      <span class="stat-label">Languages</span>
    </div>
  </div>

  <!-- REPOS -->
  <section class="repos reveal">
    <div class="section-header">
      <span class="section-label">// popular repositories</span>
      <div class="section-line"></div>
    </div>
    <div class="repos-grid">

      <a class="repo-card" href="https://github.com/lintothoppil/DS" target="_blank">
        <div class="repo-icon" style="background:rgba(85,85,153,0.15)">🗂️</div>
        <div class="repo-name">DS</div>
        <div class="repo-desc">Data Structures implementations — arrays, linked lists, trees & more, written in C.</div>
        <div class="repo-footer">
          <div class="repo-lang lang-c"><div class="lang-dot dot-c"></div>C</div>
          <div class="repo-stars">⭐ 1</div>
        </div>
      </a>

      <a class="repo-card" href="https://github.com/lintothoppil/python" target="_blank">
        <div class="repo-icon" style="background:rgba(53,114,165,0.15)">🐍</div>
        <div class="repo-name">python</div>
        <div class="repo-desc">Python scripts and programs covering fundamentals and practical utilities.</div>
        <div class="repo-footer">
          <div class="repo-lang lang-python"><div class="lang-dot dot-python"></div>Python</div>
          <div class="repo-stars">⭐ 1</div>
        </div>
      </a>

      <a class="repo-card" href="https://github.com/lintothoppil/web" target="_blank">
        <div class="repo-icon" style="background:rgba(227,76,38,0.15)">🌐</div>
        <div class="repo-name">web</div>
        <div class="repo-desc">Web development projects with HTML, CSS and JavaScript experiments.</div>
        <div class="repo-footer">
          <div class="repo-lang lang-html"><div class="lang-dot dot-html"></div>HTML</div>
          <div class="repo-stars">⭐ 1</div>
        </div>
      </a>

      <a class="repo-card" href="https://github.com/lintothoppil/Java" target="_blank">
        <div class="repo-icon" style="background:rgba(176,114,25,0.15)">☕</div>
        <div class="repo-name">Java</div>
        <div class="repo-desc">Java programs — OOP concepts, algorithms, and practice problems.</div>
        <div class="repo-footer">
          <div class="repo-lang lang-java"><div class="lang-dot dot-java"></div>Java</div>
          <div class="repo-stars">⭐ 1</div>
        </div>
      </a>

      <a class="repo-card" href="https://github.com/lintothoppil/Shell" target="_blank">
        <div class="repo-icon" style="background:rgba(137,224,81,0.12)">⚙️</div>
        <div class="repo-name">Shell</div>
        <div class="repo-desc">Shell scripting collection for automation and system management tasks.</div>
        <div class="repo-footer">
          <div class="repo-lang lang-shell"><div class="lang-dot dot-shell"></div>Shell</div>
          <div class="repo-stars">⭐ 1</div>
        </div>
      </a>

      <a class="repo-card" href="https://github.com/lintothoppil/rezum_ai" target="_blank">
        <div class="repo-icon" style="background:rgba(0,245,196,0.12)">🤖</div>
        <div class="repo-name">rezum_ai</div>
        <div class="repo-desc">AI-powered resume tool — smart generation and intelligent formatting.</div>
        <div class="repo-footer">
          <div class="repo-lang lang-html"><div class="lang-dot dot-html"></div>HTML</div>
          <div class="repo-stars">⭐ 1</div>
        </div>
      </a>

    </div>
  </section>

  <!-- SKILLS -->
  <section class="skills reveal">
    <div class="section-header">
      <span class="section-label">// tech stack</span>
      <div class="section-line"></div>
    </div>
    <div class="skills-grid">
      <div class="skill-chip"><span class="skill-icon">⚡</span><div class="skill-name">C</div></div>
      <div class="skill-chip"><span class="skill-icon">🐍</span><div class="skill-name">Python</div></div>
      <div class="skill-chip"><span class="skill-icon">☕</span><div class="skill-name">Java</div></div>
      <div class="skill-chip"><span class="skill-icon">🌐</span><div class="skill-name">HTML/CSS</div></div>
      <div class="skill-chip"><span class="skill-icon">🔧</span><div class="skill-name">Shell</div></div>
      <div class="skill-chip"><span class="skill-icon">🗂️</span><div class="skill-name">Data Structs</div></div>
      <div class="skill-chip"><span class="skill-icon">🤖</span><div class="skill-name">AI / ML</div></div>
      <div class="skill-chip"><span class="skill-icon">🐙</span><div class="skill-name">Git</div></div>
    </div>
  </section>

  <!-- TERMINAL -->
  <section class="reveal">
    <div class="section-header">
      <span class="section-label">// quick access</span>
      <div class="section-line"></div>
    </div>
    <div class="terminal">
      <div class="terminal-bar">
        <div class="dot-red"></div>
        <div class="dot-yellow"></div>
        <div class="dot-green"></div>
        <span class="terminal-title">bash — lintothoppil@github</span>
      </div>
      <div class="terminal-body" id="terminal-body">
        <div><span class="t-prompt">~ $ </span><span class="t-cmd">gh profile lintothoppil</span></div>
        <div class="t-out">Fetching profile data...</div>
        <br/>
        <div><span class="t-hi">name</span><span class="t-out">      : lintothoppil</span></div>
        <div><span class="t-hi">repos</span><span class="t-out">     : 12 public repositories</span></div>
        <div><span class="t-hi">stars</span><span class="t-out">     : ⭐ 9</span></div>
        <div><span class="t-hi">followers</span><span class="t-out"> : 1</span></div>
        <div><span class="t-hi">plan</span><span class="t-out">      : GitHub Pro ✓</span></div>
        <div><span class="t-hi">stack</span><span class="t-out">     : C · Python · Java · HTML · Shell</span></div>
        <br/>
        <div><span class="t-prompt">~ $ </span><span class="t-cmd">git clone https://github.com/lintothoppil/rezum_ai.git</span></div>
        <div class="t-out">Cloning into 'rezum_ai'...</div>
        <div class="t-out">remote: Enumerating objects: done.</div>
        <div><span class="t-err">✓</span><span class="t-out"> Cloning complete.</span></div>
        <br/>
        <div><span class="t-prompt">~ $ </span><span class="cursor-blink"></span></div>
      </div>
    </div>
  </section>

</main>

<footer>
  <p>Built with ♥ for <a href="https://github.com/lintothoppil" target="_blank">lintothoppil</a> · GitHub Profile · 2026</p>
</footer>

<script>
// ── Cursor ──
const cursor = document.getElementById('cursor');
const ring   = document.getElementById('cursor-ring');
document.addEventListener('mousemove', e => {
  cursor.style.left = e.clientX + 'px';
  cursor.style.top  = e.clientY + 'px';
  setTimeout(() => {
    ring.style.left = e.clientX + 'px';
    ring.style.top  = e.clientY + 'px';
  }, 80);
});
document.querySelectorAll('a,.btn,.repo-card,.skill-chip').forEach(el => {
  el.addEventListener('mouseenter', () => {
    ring.style.width = '60px'; ring.style.height = '60px';
    ring.style.opacity = '0.8';
  });
  el.addEventListener('mouseleave', () => {
    ring.style.width = '36px'; ring.style.height = '36px';
    ring.style.opacity = '0.5';
  });
});

// ── Starfield ──
const canvas = document.getElementById('starfield');
const ctx = canvas.getContext('2d');
let stars = [];
function resize() {
  canvas.width  = window.innerWidth;
  canvas.height = window.innerHeight;
}
function initStars() {
  stars = [];
  for (let i = 0; i < 180; i++) {
    stars.push({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      r: Math.random() * 1.2 + 0.2,
      a: Math.random(),
      da: (Math.random() - 0.5) * 0.004,
      dy: Math.random() * 0.15 + 0.05,
    });
  }
}
function drawStars() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  stars.forEach(s => {
    s.a += s.da;
    if (s.a <= 0 || s.a >= 1) s.da *= -1;
    s.y -= s.dy;
    if (s.y < 0) { s.y = canvas.height; s.x = Math.random() * canvas.width; }
    ctx.beginPath();
    ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
    ctx.fillStyle = `rgba(200,230,255,${s.a})`;
    ctx.fill();
  });
  requestAnimationFrame(drawStars);
}
resize(); initStars(); drawStars();
window.addEventListener('resize', () => { resize(); initStars(); });

// ── Counter animation ──
function animateCounters() {
  document.querySelectorAll('.stat-num[data-target]').forEach(el => {
    const target = +el.getAttribute('data-target');
    let curr = 0;
    const step = Math.ceil(target / 30);
    const t = setInterval(() => {
      curr = Math.min(curr + step, target);
      el.textContent = curr;
      if (curr >= target) clearInterval(t);
    }, 40);
  });
}

// ── Intersection observer ──
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.classList.add('in-view');
      if (e.target.querySelector('.stat-num[data-target]')) animateCounters();
    }
  });
}, { threshold: 0.15 });
document.querySelectorAll('.reveal').forEach(el => io.observe(el));

// ── Staggered repo & skill cards ──
const cardIO = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      const cards = e.target.querySelectorAll('.repo-card, .skill-chip');
      cards.forEach((c, i) => {
        setTimeout(() => c.classList.add('visible'), i * 100);
      });
      cardIO.unobserve(e.target);
    }
  });
}, { threshold: 0.1 });
document.querySelectorAll('.repos-grid, .skills-grid').forEach(el => cardIO.observe(el));
</script>
</body>
</html>
