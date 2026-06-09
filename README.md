<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>SAURABH GAUR // INTELLIGENCE LAYER</title>
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Space+Mono:wght@400;700&family=Bebas+Neue&display=swap" rel="stylesheet"/>
<style>
/* ══════════════════════════════════════════════
   TOKENS
══════════════════════════════════════════════ */
:root {
  --kiwi:        #CCFF00;
  --kiwi-bright: #DFFF00;
  --kiwi-mid:    #88BB00;
  --kiwi-dim:    #3A5200;
  --kiwi-ghost:  #1A2600;
  --kiwi-glow:   rgba(204,255,0,0.15);
  --black:       #000000;
  --black-tint:  #040800;
  --black-card:  #080D00;
  --white:       #E8FFD0;
  --grey:        #7A9A60;
  --grey-dim:    #3A5030;
  --font-mono:   'Share Tech Mono', monospace;
  --font-head:   'Space Mono', monospace;
}

/* ══════════════════════════════════════════════
   RESET + BASE
══════════════════════════════════════════════ */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  background: var(--black);
  color: var(--white);
  font-family: var(--font-mono);
  font-size: 13px;
  line-height: 1.75;
  overflow-x: hidden;
  cursor: crosshair;
}
::selection { background: var(--kiwi); color: #000; }
::-webkit-scrollbar { width: 3px; }
::-webkit-scrollbar-track { background: #000; }
::-webkit-scrollbar-thumb { background: var(--kiwi); }

/* ══════════════════════════════════════════════
   CANVAS BACKGROUND
══════════════════════════════════════════════ */
#matrix-canvas {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  opacity: 0.06;
  pointer-events: none;
  z-index: 0;
}

/* ══════════════════════════════════════════════
   SCANLINES
══════════════════════════════════════════════ */
body::after {
  content: '';
  position: fixed;
  inset: 0;
  background: repeating-linear-gradient(
    0deg,
    transparent, transparent 2px,
    rgba(204, 255, 0, 0.018) 2px, rgba(204, 255, 0, 0.018) 4px
  );
  pointer-events: none;
  z-index: 9998;
}

/* ══════════════════════════════════════════════
   BOOT SCREEN
══════════════════════════════════════════════ */
#boot {
  position: fixed;
  inset: 0;
  background: #000;
  z-index: 9999;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--kiwi);
  animation: bootFade 0.4s ease 3.2s forwards;
}
#boot-text { max-width: 500px; width: 100%; padding: 0 20px; }
#boot-text .line { opacity: 0; }
#boot-text .line:nth-child(1)  { animation: bootLine 0s 0.1s forwards; }
#boot-text .line:nth-child(2)  { animation: bootLine 0s 0.4s forwards; }
#boot-text .line:nth-child(3)  { animation: bootLine 0s 0.7s forwards; }
#boot-text .line:nth-child(4)  { animation: bootLine 0s 1.0s forwards; }
#boot-text .line:nth-child(5)  { animation: bootLine 0s 1.3s forwards; }
#boot-text .line:nth-child(6)  { animation: bootLine 0s 1.6s forwards; }
#boot-text .line:nth-child(7)  { animation: bootLine 0s 1.9s forwards; }
#boot-text .line:nth-child(8)  { animation: bootLine 0s 2.2s forwards; }
#boot-text .line:nth-child(9)  { animation: bootLine 0s 2.5s forwards; }
#boot-text .line:nth-child(10) { animation: bootLine 0s 2.8s forwards; }
.ok   { color: var(--kiwi); }
.warn { color: #888; }
.dim  { color: var(--kiwi-dim); }
@keyframes bootLine { to { opacity: 1; } }
@keyframes bootFade { to { opacity: 0; pointer-events: none; } }

/* ══════════════════════════════════════════════
   STATUS BAR (top)
══════════════════════════════════════════════ */
#statusbar {
  position: fixed;
  top: 0; left: 0; right: 0;
  height: 30px;
  background: var(--black-tint);
  border-bottom: 1px solid var(--kiwi-ghost);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 24px;
  z-index: 100;
  font-size: 10px;
  letter-spacing: 0.12em;
}
.sb-left  { color: var(--kiwi); }
.sb-right { color: var(--grey); display: flex; gap: 20px; }
.sb-dot   { display: inline-block; width: 6px; height: 6px; background: var(--kiwi); border-radius: 50%; margin-right: 6px; animation: pulse 1.8s ease-in-out infinite; }
@keyframes pulse { 0%,100%{opacity:1;box-shadow:0 0 6px var(--kiwi)} 50%{opacity:0.4;box-shadow:none} }

/* ══════════════════════════════════════════════
   LAYOUT WRAPPER
══════════════════════════════════════════════ */
#app {
  position: relative;
  z-index: 1;
  padding-top: 30px;
}

/* ══════════════════════════════════════════════
   HERO
══════════════════════════════════════════════ */
.hero {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 60px 24px;
  position: relative;
  border-bottom: 1px solid var(--kiwi-ghost);
}
.hero-pre {
  font-size: 10px;
  letter-spacing: 0.25em;
  color: var(--kiwi-dim);
  margin-bottom: 20px;
  text-transform: uppercase;
}
.hero-name {
  font-family: var(--font-head);
  font-weight: 700;
  font-size: clamp(3rem, 10vw, 8rem);
  line-height: 0.9;
  letter-spacing: 0.05em;
  color: var(--kiwi);
  text-shadow: 0 0 30px rgba(204,255,0,0.6), 0 0 80px rgba(204,255,0,0.2);
  position: relative;
  user-select: none;
  animation: neonFlicker 8s ease-in-out infinite;
}
.hero-name .glitch-layer {
  position: absolute;
  top: 0; left: 0; right: 0;
  color: #fff;
  opacity: 0;
  animation: glitchAnim 7s ease-in-out infinite;
}
.hero-name .glitch-layer.two {
  color: #0f0;
  animation: glitchAnim2 7s ease-in-out infinite;
}
@keyframes neonFlicker {
  0%,98%,100%{ text-shadow: 0 0 30px rgba(204,255,0,0.6), 0 0 80px rgba(204,255,0,0.2); }
  99%{ text-shadow: none; }
}
@keyframes glitchAnim {
  0%,94%,100%{ opacity:0; transform:translate(0); clip-path:inset(100% 0 0 0); }
  95%{ opacity:0.7; clip-path:inset(10% 0 70% 0); transform:translate(-6px,2px); }
  96%{ opacity:0.5; clip-path:inset(50% 0 30% 0); transform:translate(4px,-2px); }
  97%{ opacity:0.7; clip-path:inset(80% 0 5%  0); transform:translate(-4px,1px); }
}
@keyframes glitchAnim2 {
  0%,95%,100%{ opacity:0; transform:translate(0); clip-path:inset(100% 0 0 0); }
  96%{ opacity:0.4; clip-path:inset(30% 0 55% 0); transform:translate(5px,-3px); }
  97%{ opacity:0.3; clip-path:inset(65% 0 20% 0); transform:translate(-5px,3px); }
}
.hero-subtitle {
  font-size: 11px;
  color: var(--grey);
  letter-spacing: 0.2em;
  margin-top: 16px;
  text-transform: uppercase;
}
.hero-subtitle span { color: var(--kiwi-mid); }
.hero-ticker {
  margin-top: 28px;
  font-size: 12px;
  color: var(--kiwi-mid);
  height: 20px;
  overflow: hidden;
}
.hero-ticker .tick-line {
  display: block;
  animation: tickScroll 14s linear infinite;
}
@keyframes tickScroll {
  0%   { transform: translateY(0); }
  14%  { transform: translateY(0); }
  20%  { transform: translateY(-20px); }
  34%  { transform: translateY(-20px); }
  40%  { transform: translateY(-40px); }
  54%  { transform: translateY(-40px); }
  60%  { transform: translateY(-60px); }
  74%  { transform: translateY(-60px); }
  80%  { transform: translateY(-80px); }
  94%  { transform: translateY(-80px); }
  100% { transform: translateY(-100px); }
}
.hero-badges {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  margin-top: 36px;
}
.badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 14px;
  border: 1px solid var(--kiwi-ghost);
  font-size: 10px;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--kiwi);
  background: rgba(204,255,0,0.04);
  cursor: default;
  transition: all 180ms;
}
.badge:hover {
  border-color: var(--kiwi);
  background: var(--kiwi-glow);
  box-shadow: 0 0 12px rgba(204,255,0,0.2);
}
.badge-dot { width: 5px; height: 5px; background: var(--kiwi); border-radius: 50%; flex-shrink: 0; }

/* ── visitor badge ── */
.hero-visitor {
  margin-top: 14px;
  opacity: 0.65;
  transition: opacity 200ms;
}
.hero-visitor:hover { opacity: 1; }

.hero-scroll {
  position: absolute;
  bottom: 28px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 9px;
  letter-spacing: 0.2em;
  color: var(--kiwi-dim);
  animation: scrollBounce 2s ease-in-out infinite;
}
@keyframes scrollBounce { 0%,100%{transform:translateX(-50%) translateY(0)} 50%{transform:translateX(-50%) translateY(5px)} }

/* ══════════════════════════════════════════════
   SECTION SHELL
══════════════════════════════════════════════ */
.section {
  max-width: 960px;
  margin: 0 auto;
  padding: 72px 24px;
  border-bottom: 1px solid var(--kiwi-ghost);
}
.section-label {
  font-size: 9px;
  letter-spacing: 0.3em;
  color: var(--kiwi-dim);
  text-transform: uppercase;
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 10px;
}
.section-label::after {
  content: '';
  flex: 1;
  height: 1px;
  background: linear-gradient(to right, var(--kiwi-ghost), transparent);
}
.section-title {
  font-family: var(--font-head);
  font-size: clamp(1.5rem, 4vw, 2.8rem);
  font-weight: 700;
  color: var(--kiwi);
  letter-spacing: 0.08em;
  line-height: 1.1;
  text-shadow: 0 0 16px rgba(204,255,0,0.3);
  margin-bottom: 40px;
}

/* ══════════════════════════════════════════════
   TERMINAL BLOCK
══════════════════════════════════════════════ */
.terminal {
  background: var(--black-card);
  border: 1px solid var(--kiwi-ghost);
  padding: 20px 24px;
  margin-bottom: 20px;
  position: relative;
  overflow: hidden;
}
.terminal::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 1px;
  background: linear-gradient(to right, transparent, var(--kiwi), transparent);
  opacity: 0.4;
}
.terminal-header {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 9px;
  letter-spacing: 0.2em;
  color: var(--grey-dim);
  margin-bottom: 16px;
  padding-bottom: 10px;
  border-bottom: 1px solid var(--kiwi-ghost);
  text-transform: uppercase;
}
.term-btn { width: 8px; height: 8px; border-radius: 50%; }
.term-btn.r { background: #ff4c4c55; }
.term-btn.y { background: #ffcc0055; }
.term-btn.g { background: var(--kiwi); opacity: 0.5; }
.prompt { color: var(--kiwi); }
.cmd    { color: var(--white); }
.term-row { margin: 4px 0; display: flex; gap: 10px; }
.term-key {
  color: var(--grey);
  min-width: 80px;
  flex-shrink: 0;
}
.term-val { color: var(--white); }
.term-val.kiwi { color: var(--kiwi); }
.term-comment { color: var(--kiwi-dim); font-style: normal; }
.blink { animation: blink 1s step-end infinite; }
@keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }

/* ══════════════════════════════════════════════
   DOMAIN ITEMS
══════════════════════════════════════════════ */
.domain-grid {
  display: grid;
  gap: 2px;
  grid-template-columns: 1fr 1fr;
}
@media(max-width:640px){ .domain-grid{ grid-template-columns: 1fr; } }
.domain-item {
  padding: 20px;
  border: 1px solid var(--kiwi-ghost);
  background: var(--black-card);
  transition: all 200ms ease;
  cursor: default;
}
.domain-item:hover {
  border-color: rgba(204,255,0,0.35);
  background: rgba(204,255,0,0.03);
  transform: translateY(-1px);
  box-shadow: 0 4px 20px rgba(204,255,0,0.06);
}
.domain-num {
  font-size: 9px;
  letter-spacing: 0.25em;
  color: var(--kiwi-dim);
  margin-bottom: 8px;
}
.domain-title {
  font-family: var(--font-head);
  font-size: 0.75rem;
  font-weight: 700;
  color: var(--kiwi);
  letter-spacing: 0.1em;
  margin-bottom: 10px;
  text-transform: uppercase;
}
.domain-body { color: var(--grey); font-size: 12px; line-height: 1.7; }

/* ══════════════════════════════════════════════
   TRANSMISSION (PROJECTS)
══════════════════════════════════════════════ */
.transmission {
  margin-bottom: 2px;
  border: 1px solid var(--kiwi-ghost);
  background: var(--black-card);
  overflow: hidden;
  transition: border-color 200ms;
}
.transmission:hover { border-color: rgba(204,255,0,0.3); }
.trans-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  border-bottom: 1px solid var(--kiwi-ghost);
  cursor: pointer;
  gap: 12px;
}
.trans-num {
  font-size: 9px;
  letter-spacing: 0.2em;
  color: var(--kiwi-dim);
  flex-shrink: 0;
}
.trans-title {
  font-family: var(--font-head);
  font-size: 0.8rem;
  font-weight: 700;
  color: var(--kiwi);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  flex: 1;
}
.trans-tag {
  font-size: 9px;
  color: var(--grey);
  letter-spacing: 0.1em;
  flex-shrink: 0;
}
.trans-body { padding: 20px; }
.trans-label {
  font-size: 9px;
  letter-spacing: 0.25em;
  color: var(--kiwi-dim);
  text-transform: uppercase;
  margin-bottom: 6px;
  margin-top: 14px;
}
.trans-label:first-child { margin-top: 0; }
.trans-text { color: var(--grey); font-size: 12px; line-height: 1.8; }
.tree-item {
  display: flex;
  gap: 12px;
  margin: 4px 0;
  color: var(--grey);
  font-size: 12px;
}
.tree-icon { color: var(--kiwi-mid); flex-shrink: 0; }
.status-row {
  display: flex;
  gap: 24px;
  flex-wrap: wrap;
  margin-top: 14px;
  padding-top: 14px;
  border-top: 1px solid var(--kiwi-ghost);
}
.status-item { display: flex; gap: 8px; font-size: 11px; }
.status-key { color: var(--kiwi-dim); letter-spacing: 0.1em; }
.status-val { color: var(--kiwi); }

/* ══════════════════════════════════════════════
   STACK
══════════════════════════════════════════════ */
.stack-grid {
  display: grid;
  gap: 2px;
  grid-template-columns: repeat(3, 1fr);
}
@media(max-width:640px){ .stack-grid{ grid-template-columns: 1fr; } }
.stack-block {
  background: var(--black-card);
  border: 1px solid var(--kiwi-ghost);
  padding: 18px;
}
.stack-title {
  font-size: 9px;
  letter-spacing: 0.25em;
  color: var(--kiwi);
  text-transform: uppercase;
  margin-bottom: 12px;
  padding-bottom: 8px;
  border-bottom: 1px solid var(--kiwi-ghost);
}
.stack-title.meta { color: #DFFF00; text-shadow: 0 0 8px rgba(223,255,0,0.5); }
.tag-list { display: flex; flex-wrap: wrap; gap: 6px; }
.stag {
  display: inline-block;
  padding: 3px 8px;
  border: 1px solid var(--kiwi-ghost);
  font-size: 10px;
  color: var(--grey);
  letter-spacing: 0.05em;
  transition: all 150ms;
  cursor: default;
}
.stag:hover {
  border-color: var(--kiwi);
  color: var(--kiwi);
  background: var(--kiwi-glow);
}
.stag.meta-tag {
  color: var(--kiwi);
  border-color: rgba(204,255,0,0.3);
}

/* ══════════════════════════════════════════════
   AXIOMS
══════════════════════════════════════════════ */
.axiom-list { display: grid; gap: 2px; }
.axiom {
  display: grid;
  grid-template-columns: 60px 1fr;
  gap: 16px;
  padding: 18px 20px;
  background: var(--black-card);
  border: 1px solid var(--kiwi-ghost);
  align-items: start;
  transition: all 180ms;
  cursor: default;
}
.axiom:hover {
  border-color: rgba(204,255,0,0.25);
  background: rgba(204,255,0,0.02);
}
.axiom-num {
  font-family: var(--font-head);
  font-weight: 700;
  font-size: 1.6rem;
  color: var(--kiwi-ghost);
  line-height: 1;
  transition: color 180ms;
}
.axiom:hover .axiom-num { color: var(--kiwi-dim); }
.axiom-text { font-size: 12px; color: var(--grey); line-height: 1.8; }
.axiom-text strong { color: var(--white); font-weight: 400; }

/* ══════════════════════════════════════════════
   METRICS
══════════════════════════════════════════════ */
.metrics-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2px;
  margin-bottom: 2px;
}
@media(max-width:600px){ .metrics-grid{ grid-template-columns: 1fr; } }
.metric-img {
  background: var(--black-card);
  border: 1px solid var(--kiwi-ghost);
  overflow: hidden;
  transition: border-color 200ms;
}
.metric-img:hover { border-color: rgba(204,255,0,0.3); }
.metric-img img { display: block; width: 100%; height: auto; }
.metric-full {
  background: var(--black-card);
  border: 1px solid var(--kiwi-ghost);
  overflow: hidden;
  transition: border-color 200ms;
}
.metric-full:hover { border-color: rgba(204,255,0,0.3); }
.metric-full img { display: block; width: 100%; height: auto; }

/* ══════════════════════════════════════════════
   CONNECT
══════════════════════════════════════════════ */
.connect-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 2px;
  margin-top: 32px;
}
@media(max-width:500px){ .connect-grid{ grid-template-columns: 1fr; } }
.connect-link {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px 20px;
  background: var(--black-card);
  border: 1px solid var(--kiwi-ghost);
  text-decoration: none;
  color: var(--grey);
  font-size: 11px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  transition: all 180ms;
}
.connect-link:hover {
  border-color: var(--kiwi);
  color: var(--kiwi);
  background: var(--kiwi-glow);
  box-shadow: 0 0 16px rgba(204,255,0,0.1);
}
.connect-link .icon { font-size: 16px; flex-shrink: 0; }
.connect-label { display: flex; flex-direction: column; gap: 2px; }
.connect-type { font-size: 9px; color: var(--kiwi-dim); }
.connect-link:hover .connect-type { color: var(--kiwi-mid); }
.cta-banner {
  margin-top: 48px;
  padding: 24px;
  border: 1px solid var(--kiwi-ghost);
  background: var(--black-card);
  text-align: center;
  position: relative;
  overflow: hidden;
}
.cta-banner::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 1px;
  background: linear-gradient(to right, transparent, var(--kiwi), transparent);
}
.cta-banner::after {
  content: '';
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 1px;
  background: linear-gradient(to right, transparent, var(--kiwi), transparent);
}
.cta-label { font-size: 9px; letter-spacing: 0.25em; color: var(--kiwi-dim); margin-bottom: 8px; }
.cta-text { font-size: 11px; color: var(--grey); line-height: 1.8; }
.cta-text strong { color: var(--kiwi); font-weight: 400; }

/* ══════════════════════════════════════════════
   ASCII BOX
══════════════════════════════════════════════ */
.ascii-box {
  font-family: var(--font-mono);
  font-size: 11px;
  color: var(--kiwi-dim);
  text-align: center;
  margin: 40px 0;
  white-space: pre;
  line-height: 1.5;
}
.ascii-box span { color: var(--kiwi); }

/* ══════════════════════════════════════════════
   TTY FOOTER
══════════════════════════════════════════════ */
.tty {
  font-size: 9px;
  letter-spacing: 0.15em;
  color: var(--kiwi-dim);
  padding: 8px 0;
  border-top: 1px solid var(--kiwi-ghost);
  display: flex;
  gap: 16px;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 4px;
}
.tty-item::before { content: '> '; color: var(--kiwi-dim); }

/* ══════════════════════════════════════════════
   FOOTER
══════════════════════════════════════════════ */
footer {
  background: var(--black-tint);
  border-top: 1px solid var(--kiwi-ghost);
  padding: 40px 24px 60px;
  text-align: center;
}
.footer-name {
  font-family: var(--font-head);
  font-size: 1.4rem;
  font-weight: 700;
  color: var(--kiwi);
  letter-spacing: 0.15em;
  margin-bottom: 8px;
  text-shadow: 0 0 12px rgba(204,255,0,0.3);
}
.footer-sub { font-size: 10px; color: var(--kiwi-dim); letter-spacing: 0.2em; }
.footer-line {
  width: 100%;
  height: 1px;
  background: linear-gradient(to right, transparent, var(--kiwi), transparent);
  margin: 24px auto;
  max-width: 400px;
  opacity: 0.3;
}
.footer-copy { font-size: 9px; color: var(--grey-dim); letter-spacing: 0.15em; }

/* ══════════════════════════════════════════════
   CAPSULE FOOTER WAVE
══════════════════════════════════════════════ */
.capsule-footer {
  width: 100%;
  display: block;
  margin: 0;
  padding: 0;
  border: none;
  outline: none;
}

/* ══════════════════════════════════════════════
   UTILITIES
══════════════════════════════════════════════ */
.kiwi  { color: var(--kiwi); }
.dim   { color: var(--kiwi-dim); }
.grey  { color: var(--grey); }
.divider {
  height: 1px;
  background: linear-gradient(to right, transparent, var(--kiwi-ghost), transparent);
  margin: 24px 0;
}
.mt8  { margin-top: 8px; }
.mt16 { margin-top: 16px; }
.mt24 { margin-top: 24px; }

/* explore list */
.explore-list { display: grid; gap: 2px; }
.explore-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px 18px;
  background: var(--black-card);
  border: 1px solid var(--kiwi-ghost);
  font-size: 12px;
  color: var(--grey);
  transition: all 180ms;
  cursor: default;
}
.explore-item:hover {
  border-color: rgba(204,255,0,0.25);
  color: var(--white);
  transform: translateX(4px);
}
.explore-arrow { color: var(--kiwi); flex-shrink: 0; }
</style>
<base target="_blank">
</head>
<body>

<!-- MATRIX CANVAS -->
<canvas id="matrix-canvas"></canvas>

<!-- BOOT SCREEN -->
<div id="boot">
  <div id="boot-text">
    <div class="line dim">SAURABH-GAUR.WORLD // FIELD STATION PRIME</div>
    <div class="line dim">──────────────────────────────────────────</div>
    <div class="line warn">&gt; ESTABLISHING SECURE CONNECTION............</div>
    <div class="line ok">&gt; AUTHORIZATION CHECK .................. <span class="ok">OK</span></div>
    <div class="line ok">&gt; IDENTITY VERIFICATION ................ <span class="ok">OK</span></div>
    <div class="line ok">&gt; INTELLIGENCE LAYER ACCESS ............ <span class="ok">GRANTED</span></div>
    <div class="line warn">&gt; LOADING DOSSIER :: SAURABH GAUR ........</div>
    <div class="line ok">&gt; FRONTIER MODEL CLEARANCE ............. <span class="ok">ACTIVE</span></div>
    <div class="line ok">&gt; SESSION INITIALIZED .................. <span class="ok">OK</span></div>
    <div class="line dim">&gt; WELCOME, OPERATOR. <span class="blink">█</span></div>
  </div>
</div>

<!-- STATUS BAR -->
<div id="statusbar">
  <div class="sb-left">
    <span class="sb-dot"></span>
    SGF-PRIME // INTELLIGENCE LAYER
  </div>
  <div class="sb-right">
    <span>OUTLIER.AI · OPENCLAW ATLAS</span>
    <span id="sb-time"></span>
  </div>
</div>

<!-- APP -->
<div id="app">

  <!-- ═══════════════════════════════
       HERO
  ═══════════════════════════════ -->
  <section class="hero">
    <div class="hero-pre">// DOSSIER SG-01 · ESTABLISHED 2025 · ACTIVE</div>

    <div class="hero-name">
      SAURABH
      <span class="glitch-layer">SAURABH</span>
      <span class="glitch-layer two">SAURABH</span>
    </div>
    <div class="hero-name" style="margin-top:0; text-shadow: 0 0 20px rgba(204,255,0,0.4);">GAUR</div>

    <div class="hero-subtitle">
      <span>intelligence architect</span>
      &nbsp;·&nbsp;
      <span>frontier evaluator</span>
      &nbsp;·&nbsp;
      <span>systems strategist</span>
    </div>

    <div class="hero-ticker">
      <span class="tick-line">&gt; i decide where intelligence ends &amp; hallucination begins.</span>
      <span class="tick-line">&gt; eval → RLHF → the next frontier model runs on my judgment.</span>
      <span class="tick-line">&gt; i don't use AI tools. i shape what they become.</span>
      <span class="tick-line">&gt; operating at the layer most engineers never reach.</span>
      <span class="tick-line">&gt; few people define "correct". i'm one of them.</span>
    </div>

    <div class="hero-badges">
      <div class="badge"><span class="badge-dot"></span>FRONTIER MODEL EVALUATION</div>
      <div class="badge"><span class="badge-dot"></span>OPEN TO REMOTE ROLES</div>
      <div class="badge"><span class="badge-dot"></span>IST / FLEXIBLE</div>
    </div>

    <!-- ── VISITOR BADGE ── -->
    <div class="hero-visitor">
      <img
        src="https://visitor-badge.laobi.icu/badge?page_id=DEVsaurabhgaur.DEVsaurabhgaur&left_color=040800&right_color=3A5200&left_text=signal+received&right_text_color=CCFF00"
        alt="signal received"
        style="height:22px;"
      />
    </div>

    <div class="hero-scroll">↓ &nbsp; SCROLL TO DECRYPT &nbsp; ↓</div>
  </section>


  <!-- ═══════════════════════════════
       INITIALIZE
  ═══════════════════════════════ -->
  <div class="section">
    <div class="section-label">[ SECTOR 01 ]</div>
    <div class="section-title">INITIALIZE</div>

    <div class="terminal">
      <div class="terminal-header">
        <div class="term-btn r"></div>
        <div class="term-btn y"></div>
        <div class="term-btn g"></div>
        <span style="margin-left:8px">tty/0 :: identity.core</span>
      </div>
      <div style="margin-bottom:10px">
        <span class="prompt">┌──[</span><span class="kiwi">saurabh@intelligence-layer</span><span class="prompt"> ~]</span>
      </div>
      <div style="margin-bottom:16px">
        <span class="prompt">└─$ </span><span class="cmd">cat identity.core</span>
      </div>
      <div class="term-row"><span class="term-key">alias</span><span class="dim">──</span><span class="term-val kiwi">ITACHI</span></div>
      <div class="term-row"><span class="term-key">domain</span><span class="dim">──</span><span class="term-val">Frontier AI Evaluation · Agentic Systems Architecture</span></div>
      <div class="term-row"><span class="term-key">base</span><span class="dim">──</span><span class="term-val">Rudrapur, India &nbsp;∷&nbsp; remote-first · timezone-flexible</span></div>
      <div class="term-row"><span class="term-key">active</span><span class="dim">──</span><span class="term-val kiwi">Frontier LLM Eval @ Outlier.ai [ OpenClaw Atlas · present ]</span></div>
      <div class="term-row"><span class="term-key">building</span><span class="dim">──</span><span class="term-val">LaptopPulse · AI Resume Copilot · saurabhgaur.world</span></div>
      <div class="term-row"><span class="term-key">degree</span><span class="dim">──</span><span class="term-val">B.Tech CSE · AKTU 2025 [ First Division ]</span></div>
      <div class="term-row"><span class="term-key">target</span><span class="dim">──</span><span class="term-val">top-tier AI labs · product companies at scale</span></div>
      <div class="term-row"><span class="term-key"></span><span class="dim">──</span><span class="term-val kiwi">roles where the work shapes the field — not just ships features</span></div>
    </div>

    <div class="terminal mt16">
      <div class="terminal-header">
        <div class="term-btn r"></div><div class="term-btn y"></div><div class="term-btn g"></div>
        <span style="margin-left:8px">tty/0 :: signal.txt</span>
      </div>
      <div style="margin-bottom:12px">
        <span class="prompt">└─$ </span><span class="cmd">cat signal.txt</span>
      </div>
      <div class="term-comment">// most engineers implement at the instruction layer.</div>
      <div class="term-comment">// i work one level above that — at the evaluation layer —</div>
      <div class="term-comment">// where the criteria for "correct" actually gets defined.</div>
      <div class="term-comment">//</div>
      <div class="term-comment">// that's where frontier models learn what intelligence means.</div>
      <div class="term-comment">// that's the leverage point. i chose this deliberately.</div>
      <div style="margin-top:12px">
        <span class="prompt">└─$ </span><span class="blink kiwi">█</span>
      </div>
    </div>

    <div class="tty">
      <span class="tty-item">SECTOR 01 DECODED</span>
      <span class="tty-item">SESSION ACTIVE</span>
      <span class="tty-item">STANDING BY</span>
    </div>
  </div>


  <!-- ═══════════════════════════════
       DOMAIN EXPANSION
  ═══════════════════════════════ -->
  <div class="section">
    <div class="section-label">[ SECTOR 02 ]</div>
    <div class="section-title">DOMAIN EXPANSION</div>

    <div class="ascii-box">╔══════════════════════════════════════════════════════╗
║                                                      ║
║   ░▓▓▓  <span>DOMAIN EXPANSION : INFINITE VOID</span>  ▓▓▓░   ║
║                                                      ║
║        [ i n t e l l i g e n c e  ·  l a y e r ]   ║
║                                                      ║
╚══════════════════════════════════════════════════════╝</div>

    <div class="domain-grid">
      <div class="domain-item">
        <div class="domain-num">◈ 01</div>
        <div class="domain-title">Frontier Model Evaluation</div>
        <div class="domain-body">Design eval architectures that expose failure modes standard benchmarks miss. Outputs route directly into RLHF training pipelines. Not just testing — shaping the next version.</div>
      </div>
      <div class="domain-item">
        <div class="domain-num">◈ 02</div>
        <div class="domain-title">Agentic Systems Architecture</div>
        <div class="domain-body">LangGraph · LangChain multi-agent systems. Tool-use chains, memory persistence, context routing under constraint. Architecture over implementation — always.</div>
      </div>
      <div class="domain-item">
        <div class="domain-num">◈ 03</div>
        <div class="domain-title">RAG Infrastructure</div>
        <div class="domain-body">FAISS · Chroma vector stores. Retrieval pipelines, embedding strategies, precision-at-K tuning. Built for production, not demos.</div>
      </div>
      <div class="domain-item">
        <div class="domain-num">◈ 04</div>
        <div class="domain-title">Systems Thinking</div>
        <div class="domain-body">Decomposing the problem correctly is 80% of the work. Most engineers skip this and wonder why their implementation fails. I don't skip this.</div>
      </div>
    </div>

    <div class="tty">
      <span class="tty-item">4 DOMAINS ACTIVE</span>
      <span class="tty-item">EVALUATION LAYER UNLOCKED</span>
    </div>
  </div>


  <!-- ═══════════════════════════════
       ACTIVE TRANSMISSIONS
  ═══════════════════════════════ -->
  <div class="section">
    <div class="section-label">[ SECTOR 03 ]</div>
    <div class="section-title">ACTIVE TRANSMISSIONS</div>

    <!-- Transmission 01 -->
    <div class="transmission">
      <div class="trans-header">
        <span class="trans-num">⟨ 01 ⟩</span>
        <span class="trans-title">OpenClaw Atlas</span>
        <span class="trans-tag">OUTLIER.AI · APR 2026 – PRESENT</span>
      </div>
      <div class="trans-body">
        <div class="trans-label">// PROBLEM</div>
        <div class="trans-text">Frontier agents break in specific patterns across multi-server environments. Standard benchmarks never catch it at this fidelity. Nobody had built a framework that did.</div>

        <div class="trans-label">// BUILT</div>
        <div class="trans-text">Evaluation architecture spanning 6 synthetic data servers with deliberate cross-domain contradictions. Surfaces:</div>
        <div class="mt8">
          <div class="tree-item"><span class="tree-icon">├──</span>time-domain reasoning failures under conflicting calendar signals</div>
          <div class="tree-item"><span class="tree-icon">├──</span>memory persistence degradation across multi-turn agent tasks</div>
          <div class="tree-item"><span class="tree-icon">├──</span>privacy boundary violations</div>
          <div class="tree-item"><span class="tree-icon">├──</span>multi-source synthesis errors across domain personas</div>
          <div class="tree-item"><span class="tree-icon">└──</span>defamation-risk refusal pattern failures</div>
        </div>

        <div class="trans-label">// OUTPUT</div>
        <div class="trans-text">Directly into RLHF training pipelines. This isn't a portfolio project. This shapes live frontier models.</div>

        <div class="status-row">
          <div class="status-item"><span class="status-key">STATUS</span><span class="status-val kiwi">ACTIVE</span></div>
        </div>
      </div>
    </div>

    <!-- Transmission 02 -->
    <div class="transmission">
      <div class="trans-header">
        <span class="trans-num">⟨ 02 ⟩</span>
        <span class="trans-title">LaptopPulse</span>
        <span class="trans-tag">INDIE · PYTHON · WINDOWS · JUN 2026</span>
      </div>
      <div class="trans-body">
        <div class="trans-label">// PROBLEM</div>
        <div class="trans-text">Non-technical users have zero signal on hardware health until the machine fails completely. The thermal data exists. The plain-language insight doesn't.</div>

        <div class="trans-label">// BUILT</div>
        <div class="trans-text">Windows background daemon · real-time CPU/GPU thermal reads via LibreHardwareMonitor · pattern detection → LLM inference → plain-english maintenance report · zero technical knowledge required.</div>

        <div class="status-row">
          <div class="status-item"><span class="status-key">STATUS</span><span class="status-val kiwi">LIVE · DEPLOYED</span></div>
          <div class="status-item"><span class="status-key">HEALTH</span><span class="status-val">92/100 on dev machine</span></div>
          <div class="status-item"><span class="status-key">STACK</span><span class="status-val">Python · Flask · React/Vite · TypeScript</span></div>
          <div class="status-item"><span class="status-key">REPO</span><span class="status-val"><a href="https://github.com/DEVsaurabhgaur/LaptopPulse" style="color:var(--kiwi);text-decoration:none">github.com/DEVsaurabhgaur/LaptopPulse ↗</a></span></div>
        </div>
      </div>
    </div>

    <!-- Transmission 03 -->
    <div class="transmission">
      <div class="trans-header">
        <span class="trans-num">⟨ 03 ⟩</span>
        <span class="trans-title">AI Resume Copilot</span>
        <span class="trans-tag">TOOL · HTML/JS + CLAUDE API · 2026</span>
      </div>
      <div class="trans-body">
        <div class="trans-label">// BUILT</div>
        <div class="trans-text">ATS optimization · role targeting · cover letter generation · application tracking — single tool, zero backend, zero friction. Claude API · deployed on GitHub Pages.</div>

        <div class="trans-label">// INSIGHT</div>
        <div class="trans-text">Job applications are a system. Most people play it like luck. This turns it into a pipeline.</div>
      </div>
    </div>

    <div class="tty">
      <span class="tty-item">3 TRANSMISSIONS LOGGED</span>
      <span class="tty-item">SIGNAL CONFIRMED</span>
    </div>
  </div>


  <!-- ═══════════════════════════════
       STACK
  ═══════════════════════════════ -->
  <div class="section">
    <div class="section-label">[ SECTOR 04 ]</div>
    <div class="section-title">STACK</div>

    <div class="stack-grid">
      <div class="stack-block">
        <div class="stack-title">[ AI · INTELLIGENCE ]</div>
        <div class="tag-list">
          <span class="stag">LangChain</span><span class="stag">LangGraph</span><span class="stag">LlamaIndex</span>
          <span class="stag">FAISS</span><span class="stag">Chroma</span><span class="stag">OpenAI API</span>
          <span class="stag">Anthropic API</span><span class="stag">HuggingFace</span>
          <span class="stag">RLHF Pipelines</span><span class="stag">Adversarial Testing</span>
          <span class="stag">Benchmark Architecture</span><span class="stag">Synthetic Dataset Design</span>
          <span class="stag">LLM Evaluation</span><span class="stag">Prompt Engineering</span>
        </div>
      </div>
      <div class="stack-block">
        <div class="stack-title">[ ENGINEERING ]</div>
        <div class="tag-list">
          <span class="stag">Python</span><span class="stag">TypeScript</span><span class="stag">React</span>
          <span class="stag">Next.js</span><span class="stag">Flask</span><span class="stag">Supabase</span>
          <span class="stag">Docker</span><span class="stag">GitHub Actions</span>
          <span class="stag">Linux CLI</span><span class="stag">Windows API</span><span class="stag">SQLite</span>
        </div>
      </div>
      <div class="stack-block" style="grid-column: span 1">
        <div class="stack-title meta">[ META-LAYER ] ← real work</div>
        <div class="tag-list">
          <span class="stag meta-tag">Failure Taxonomy Design</span>
          <span class="stag meta-tag">Systems Architecture</span>
          <span class="stag meta-tag">Evaluation Criteria Design</span>
          <span class="stag meta-tag">Cross-domain Reasoning</span>
          <span class="stag meta-tag">Strategic Decomposition</span>
        </div>
      </div>
    </div>

    <div class="tty">
      <span class="tty-item">STACK INVENTORIED</span>
      <span class="tty-item">META-LAYER ACTIVE</span>
    </div>
  </div>


  <!-- ═══════════════════════════════
       CURRENTLY EXPLORING
  ═══════════════════════════════ -->
  <div class="section">
    <div class="section-label">[ SECTOR 05 ]</div>
    <div class="section-title">CURRENTLY EXPLORING</div>

    <div class="terminal" style="margin-bottom:20px">
      <div style="margin-bottom:10px"><span class="prompt">└─$ </span><span class="cmd">tail -f research.log</span></div>
    </div>

    <div class="explore-list">
      <div class="explore-item"><span class="explore-arrow">▸</span> Constitutional AI · RLAIF · Direct Preference Optimization (DPO)</div>
      <div class="explore-item"><span class="explore-arrow">▸</span> Multi-agent coordination protocols · Emergent agent behavior</div>
      <div class="explore-item"><span class="explore-arrow">▸</span> Interpretability methods for frontier models</div>
      <div class="explore-item"><span class="explore-arrow">▸</span> Evaluation frameworks for agentic long-horizon tasks</div>
    </div>

    <div class="tty">
      <span class="tty-item">RESEARCH LOG STREAMING</span>
      <span class="tty-item">4 VECTORS ACTIVE</span>
    </div>
  </div>


  <!-- ═══════════════════════════════
       AXIOMS
  ═══════════════════════════════ -->
  <div class="section">
    <div class="section-label">[ SECTOR 06 ]</div>
    <div class="section-title">AXIOMS</div>

    <div class="axiom-list">
      <div class="axiom">
        <div class="axiom-num">01</div>
        <div class="axiom-text">The person who writes the evaluation criteria controls what "intelligent" means. <strong>Most people never realize this is a job.</strong></div>
      </div>
      <div class="axiom">
        <div class="axiom-num">02</div>
        <div class="axiom-text">Tools change every quarter. Thinking doesn't. <strong>Learn to think — not what to think.</strong></div>
      </div>
      <div class="axiom">
        <div class="axiom-num">03</div>
        <div class="axiom-text"><strong>Ship. Break. Fix. Repeat.</strong> Perfection at zero users is fear wearing a lab coat.</div>
      </div>
      <div class="axiom">
        <div class="axiom-num">04</div>
        <div class="axiom-text"><strong>Compound in silence.</strong> Don't announce the plan. Show up with the outcome.</div>
      </div>
      <div class="axiom">
        <div class="axiom-num">05</div>
        <div class="axiom-text"><strong>If it isn't in git, it didn't happen.</strong></div>
      </div>
    </div>

    <div class="tty">
      <span class="tty-item">5 AXIOMS ENCODED</span>
      <span class="tty-item">IMMUTABLE</span>
    </div>
  </div>


  <!-- ═══════════════════════════════
       SIGNAL METRICS
  ═══════════════════════════════ -->
  <div class="section">
    <div class="section-label">[ SECTOR 07 ]</div>
    <div class="section-title">SIGNAL METRICS</div>

    <!-- stats + top langs -->
    <div class="metrics-grid">
      <div class="metric-img">
        <img src="https://github-readme-stats.vercel.app/api?username=DEVsaurabhgaur&show_icons=true&bg_color=040800&border_color=3A5200&title_color=CCFF00&icon_color=88BB00&text_color=7A9A60&count_private=true&include_all_commits=true" alt="GitHub Stats"/>
      </div>
      <div class="metric-img">
        <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=DEVsaurabhgaur&layout=compact&bg_color=040800&border_color=CCFF00&title_color=88BB00&text_color=7A9A60&langs_count=6" alt="Top Languages"/>
      </div>
    </div>

    <!-- streak -->
    <div class="metric-full" style="margin: 2px 0;">
      <img src="https://streak-stats.demolab.com/?user=DEVsaurabhgaur&background=040800&ring=CCFF00&fire=DFFF00&currStreakLabel=CCFF00&sideLabels=88BB00&currStreakNum=E8FFD0&sideNums=7A9A60&border=3A5200&dates=3A5200" alt="Streak Stats"/>
    </div>

    <!-- ── SNAKE CONTRIBUTION GRID ── -->
    <div class="metric-full" style="margin: 2px 0; padding: 12px 0;">
      <img
        src="https://raw.githubusercontent.com/DEVsaurabhgaur/DEVsaurabhgaur/output/github-contribution-grid-snake-dark.svg"
        alt="Contribution Snake"
        style="display:block;width:100%;height:auto;"
      />
    </div>

    <!-- activity graph -->
    <div class="metric-full" style="margin: 2px 0;">
      <img src="https://github-readme-activity-graph.vercel.app/graph?username=DEVsaurabhgaur&bg_color=040800&color=88BB00&line=CCFF00&point=DFFF00&area=true&area_color=1A2600&hide_border=false&border_color=3A5200&radius=4" alt="Activity Graph"/>
    </div>

    <!-- trophies -->
    <div class="metric-full" style="margin: 2px 0; padding: 16px;">
      <img src="https://github-profile-trophy.vercel.app/?username=DEVsaurabhgaur&theme=matrix&column=6&margin-w=8&margin-h=8&no-bg=true&no-frame=true" alt="Trophies"/>
    </div>

    <div class="tty">
      <span class="tty-item">METRICS SYNCHRONIZED</span>
      <span class="tty-item">SIGNAL NOMINAL</span>
    </div>
  </div>


  <!-- ═══════════════════════════════
       ESTABLISH CONNECTION
  ═══════════════════════════════ -->
  <div class="section">
    <div class="section-label">[ SECTOR 08 ]</div>
    <div class="section-title">ESTABLISH CONNECTION</div>

    <div class="terminal">
      <div style="color:var(--grey)">transmission open. choose your frequency.</div>
    </div>

    <div class="connect-grid">
      <a class="connect-link" href="mailto:saurabhgaur122000@gmail.com">
        <span class="icon">✉</span>
        <div class="connect-label">
          <div class="connect-type">EMAIL</div>
          <div>saurabhgaur122000@gmail.com</div>
        </div>
      </a>
      <a class="connect-link" href="https://linkedin.com/in/saurabhgaur-122k" target="_blank">
        <span class="icon">in</span>
        <div class="connect-label">
          <div class="connect-type">LINKEDIN</div>
          <div>saurabhgaur-122k</div>
        </div>
      </a>
      <a class="connect-link" href="https://saurabhgaur.world" target="_blank">
        <span class="icon">⊕</span>
        <div class="connect-label">
          <div class="connect-type">PORTFOLIO</div>
          <div>saurabhgaur.world</div>
        </div>
      </a>
      <a class="connect-link" href="https://github.com/DEVsaurabhgaur" target="_blank">
        <span class="icon">⑂</span>
        <div class="connect-label">
          <div class="connect-type">GITHUB</div>
          <div>DEVsaurabhgaur</div>
        </div>
      </a>
    </div>

    <div class="cta-banner">
      <div class="cta-label">// SEEKING</div>
      <div class="cta-text">
        <strong>Remote AI roles at top-tier labs &amp; product companies</strong>
        &nbsp;·&nbsp;
        <strong>LLM evaluation contracts</strong>
        &nbsp;·&nbsp;
        <strong>Agentic systems consulting</strong>
        <br><br>
        Roles where the work shapes the field — not just ships features.
      </div>
    </div>

    <div class="tty">
      <span class="tty-item">CONNECTION ESTABLISHED</span>
      <span class="tty-item">AWAITING RESPONSE</span>
      <span class="tty-item">END OF TRANSMISSION</span>
    </div>
  </div>

</div><!-- /app -->

<!-- FOOTER -->
<footer>
  <div class="footer-name">SAURABH GAUR</div>
  <div class="footer-sub">INTELLIGENCE LAYER // FIELD STATION PRIME</div>
  <div class="footer-line"></div>
  <div class="footer-copy">© 2026 · SAURABH GAUR · ALL TRANSMISSIONS ARCHIVED</div>
</footer>

<!-- CAPSULE-RENDER FOOTER WAVE -->
<img 
  class="capsule-footer" 
  src="https://capsule-render.vercel.app/api?type=waving&color=0:040800,20:080D00,45:1A2600,70:080D00,100:040800&height=100&section=footer" 
  alt=""
/>

<!-- ══════════════════════════════════════════
     SCRIPTS
══════════════════════════════════════════ -->
<script>
/* MATRIX RAIN */
(function(){
  const canvas = document.getElementById('matrix-canvas');
  const ctx = canvas.getContext('2d');
  let cols, drops;
  const CHARS = 'アイウエオカキクケコサシスセソタチツテトナニヌネノ01アイウ01ABCDEF';

  function resize(){
    canvas.width  = window.innerWidth;
    canvas.height = window.innerHeight;
    cols  = Math.floor(canvas.width / 18);
    drops = new Array(cols).fill(1);
  }
  resize();
  window.addEventListener('resize', resize);

  function draw(){
    ctx.fillStyle = 'rgba(0,0,0,0.055)';
    ctx.fillRect(0,0,canvas.width,canvas.height);
    ctx.fillStyle = '#CCFF00';
    ctx.font = '13px "Share Tech Mono", monospace';
    drops.forEach((y, i) => {
      const ch = CHARS[Math.floor(Math.random()*CHARS.length)];
      ctx.fillText(ch, i*18, y*18);
      if(y*18 > canvas.height && Math.random() > 0.975) drops[i] = 0;
      drops[i]++;
    });
  }
  setInterval(draw, 50);
})();

/* STATUS BAR CLOCK */
(function(){
  function update(){
    const d = new Date();
    const h = String(d.getHours()).padStart(2,'0');
    const m = String(d.getMinutes()).padStart(2,'0');
    const s = String(d.getSeconds()).padStart(2,'0');
    const el = document.getElementById('sb-time');
    if(el) el.textContent = h+':'+m+':'+s+' IST';
  }
  update();
  setInterval(update, 1000);
})();

/* BOOT AUTO-CLOSE backup */
setTimeout(function(){
  const b = document.getElementById('boot');
  if(b) b.style.display = 'none';
}, 4000);
</script>
</body>
</html>
