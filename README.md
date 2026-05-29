# fintech-market-slides
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>How Nations Rebalance: Global Fintech & Market Policy Shifts 2004–2024</title>
<meta name="description" content="An interactive analysis of monetary strategy, regulation, and digital finance adoption across 195 countries.">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1"></script>
<style>
/* ═══════════════════════════════════════════════════════════
   DESIGN SYSTEM — Executive Dark Theme
   ═══════════════════════════════════════════════════════════ */
:root {
  --bg: #0F1923;
  --surface: #1A2332;
  --surface-hover: #1E2A3A;
  --border: #2A3A4A;
  --teal: #00D4AA;
  --gold: #F5A623;
  --blue: #4E9EF5;
  --red: #FF6B6B;
  --purple: #A78BFA;
  --green: #34D399;
  --orange: #FB923C;
  --pink: #F472B6;
  --text: #E8EDF2;
  --text-muted: #7A8FA6;
  --text-dim: #4A5F78;
  --glow-teal: 0 0 20px rgba(0, 212, 170, 0.3);
  --glow-gold: 0 0 20px rgba(245, 166, 35, 0.3);
  --radius: 12px;
  --radius-sm: 8px;
  --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

*, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

html, body {
  width: 100%; height: 100%;
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  background: var(--bg);
  color: var(--text);
  overflow: hidden;
  -webkit-font-smoothing: antialiased;
  user-select: none;
}

/* Slide number badge */
.slide-badge {
  position: absolute; top: 20px; left: 24px;
  font-size: 11px; color: var(--text-dim); font-weight: 600;
  text-transform: uppercase; letter-spacing: 0.1em;
  z-index: 5;
}

/* Keyboard hints */
.keyboard-hints {
  position: absolute; bottom: 24px; left: 50%; transform: translateX(-50%);
  display: flex; gap: 16px; z-index: 5;
}
.key-hint {
  display: flex; align-items: center; gap: 6px;
  font-size: 11px; color: var(--text-dim);
}
.key-hint kbd {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: 4px; padding: 2px 8px;
  font-family: inherit; font-size: 10px; font-weight: 600;
  color: var(--text-muted);
}

/* ─── Slide Framework ─── */
.presentation { position: relative; width: 100%; height: 100%; }

.slide {
  position: absolute; inset: 0;
  display: flex; align-items: center; justify-content: center;
  opacity: 0; visibility: hidden;
  transition: opacity 0.5s ease, transform 0.5s ease;
  transform: translateX(40px);
  overflow-y: auto;
}
.slide.active {
  opacity: 1; visibility: visible; transform: translateX(0);
}
.slide.exit-left {
  opacity: 0; transform: translateX(-40px);
}

.slide-content {
  width: 100%; max-width: 1400px;
  padding: 60px 48px 80px;
  margin: 0 auto;
}

.slide-title {
  font-size: clamp(22px, 2.4vw, 32px);
  font-weight: 700;
  color: var(--text);
  margin-bottom: 6px;
  letter-spacing: -0.02em;
}
.slide-subtitle {
  font-size: clamp(13px, 1.1vw, 16px);
  color: var(--text-muted);
  margin-bottom: 28px;
  font-weight: 400;
}

/* ─── Navigation ─── */
.nav-arrow {
  position: fixed; top: 50%; transform: translateY(-50%);
  width: 44px; height: 44px;
  background: var(--surface); border: 1px solid var(--border);
  border-radius: 50%; display: flex; align-items: center; justify-content: center;
  cursor: pointer; z-index: 100;
  transition: var(--transition);
  color: var(--text-muted);
  font-size: 18px;
}
.nav-arrow:hover { background: var(--surface-hover); color: var(--teal); border-color: var(--teal); box-shadow: var(--glow-teal); }
.nav-arrow:active { transform: translateY(-50%) scale(0.92); }
.nav-arrow.left { left: 16px; }
.nav-arrow.right { right: 16px; }

/* Active slide highlight in menu */
.slide-menu-item.active-slide { border-color: var(--teal); background: rgba(0,212,170,0.06); }
.slide-menu-item.active-slide .num { color: var(--teal); text-shadow: 0 0 12px rgba(0,212,170,0.5); }

.slide-counter {
  position: fixed; top: 20px; right: 24px;
  font-size: 13px; color: var(--text-muted);
  z-index: 100; font-weight: 500;
  background: var(--surface); padding: 6px 14px;
  border-radius: 20px; border: 1px solid var(--border);
}

.progress-bar {
  position: fixed; bottom: 0; left: 0;
  height: 3px; background: var(--teal);
  transition: width 0.5s ease; z-index: 100;
  box-shadow: 0 0 10px var(--teal);
}

/* ─── Slide Menu (ESC) ─── */
.slide-menu {
  position: fixed; inset: 0;
  background: rgba(15, 25, 35, 0.95);
  backdrop-filter: blur(20px);
  z-index: 200; display: none;
  align-items: center; justify-content: center;
  padding: 40px;
}
.slide-menu.open { display: flex; }
.slide-menu-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px; max-width: 900px; width: 100%;
}
.slide-menu-item {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 16px;
  cursor: pointer; transition: var(--transition);
  text-align: center;
}
.slide-menu-item:hover { border-color: var(--teal); box-shadow: var(--glow-teal); }
.slide-menu-item .num { font-size: 24px; font-weight: 700; color: var(--teal); }
.slide-menu-item .label { font-size: 11px; color: var(--text-muted); margin-top: 4px; line-height: 1.3; }
.slide-menu-close {
  position: absolute; top: 20px; right: 24px;
  background: none; border: none; color: var(--text-muted);
  font-size: 28px; cursor: pointer;
}

/* ─── Common Components ─── */
.chart-container {
  position: relative;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 24px;
  box-shadow: 0 4px 24px rgba(0,0,0,0.2);
}
.chart-container::before {
  content: '';
  position: absolute; inset: -1px;
  border-radius: var(--radius);
  background: linear-gradient(135deg, rgba(0,212,170,0.1), transparent 40%, transparent 60%, rgba(245,166,35,0.08));
  z-index: -1; pointer-events: none;
}
.insight-box {
  background: linear-gradient(135deg, rgba(0,212,170,0.08), rgba(0,212,170,0.02));
  border: 1px solid rgba(0,212,170,0.2);
  border-left: 3px solid var(--teal);
  border-radius: var(--radius-sm);
  padding: 14px 18px; margin-top: 16px;
  font-size: 13px; color: var(--text-muted); line-height: 1.5;
}
.insight-box strong { color: var(--teal); }

.toggle-group {
  display: flex; gap: 4px;
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius-sm); padding: 3px;
  width: fit-content;
}
.toggle-btn {
  padding: 6px 16px; border-radius: 6px;
  font-size: 12px; font-weight: 500;
  color: var(--text-muted); background: none;
  border: none; cursor: pointer;
  transition: var(--transition);
  font-family: inherit;
}
.toggle-btn.active {
  background: var(--teal); color: var(--bg); font-weight: 600;
}
.toggle-btn:hover:not(.active) { color: var(--text); }

.filter-group {
  display: flex; gap: 6px; flex-wrap: wrap;
  margin-bottom: 16px;
}
.filter-btn {
  padding: 5px 14px; border-radius: 20px;
  font-size: 11px; font-weight: 500;
  background: var(--surface); color: var(--text-muted);
  border: 1px solid var(--border);
  cursor: pointer; transition: var(--transition);
  font-family: inherit;
}
.filter-btn.active {
  background: rgba(0,212,170,0.15); color: var(--teal);
  border-color: var(--teal);
}
.filter-btn:hover:not(.active) { border-color: var(--text-muted); }

.popup-overlay {
  position: fixed; inset: 0;
  background: rgba(15,25,35,0.8);
  backdrop-filter: blur(8px);
  z-index: 300; display: none;
  align-items: center; justify-content: center;
}
.popup-overlay.show { display: flex; }
.popup-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 28px;
  max-width: 420px; width: 90%; position: relative;
}
.popup-close {
  position: absolute; top: 12px; right: 14px;
  background: none; border: none; color: var(--text-muted);
  font-size: 20px; cursor: pointer;
}

/* ─── Scrollbar ─── */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: var(--bg); }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }

/* ═══════════════════════════════════════════════════════════
   SLIDE 1 — TITLE
   ═══════════════════════════════════════════════════════════ */
#slide-1 .slide-content {
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  text-align: center; min-height: 100vh;
  position: relative;
}
.title-main {
  font-size: clamp(28px, 3.5vw, 52px);
  font-weight: 800; letter-spacing: -0.03em;
  line-height: 1.15; margin-bottom: 16px;
  background: linear-gradient(135deg, var(--text) 0%, var(--teal) 50%, var(--gold) 100%);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  background-clip: text;
}
.title-sub {
  font-size: clamp(14px, 1.3vw, 20px);
  color: var(--text-muted); font-weight: 300;
  max-width: 640px; margin-bottom: 48px;
}
.title-stats {
  display: flex; gap: 48px; flex-wrap: wrap;
  justify-content: center;
}
.title-stat {
  text-align: center;
}
.title-stat .num {
  font-size: clamp(28px, 3vw, 44px);
  font-weight: 800; color: var(--teal);
  font-variant-numeric: tabular-nums;
}
.title-stat .num.gold { color: var(--gold); }
.title-stat .num.blue { color: var(--blue); }
.title-stat .label {
  font-size: 12px; color: var(--text-muted);
  margin-top: 4px; text-transform: uppercase;
  letter-spacing: 0.08em; font-weight: 500;
}

/* World map SVG */
.world-map-bg {
  position: absolute; inset: 0;
  display: flex; align-items: center; justify-content: center;
  opacity: 0.2; pointer-events: none;
}
.world-map-bg svg { width: 90%; max-width: 1000px; }

/* Pulsing dots */
@keyframes pulse {
  0%, 100% { transform: scale(1); opacity: 0.8; }
  50% { transform: scale(1.8); opacity: 0.3; }
}
.map-dot {
  fill: var(--teal); animation: pulse 2.5s ease-in-out infinite;
}
.map-dot.gold { fill: var(--gold); }
.map-dot.blue { fill: var(--blue); }

/* Floating particles */
.particles {
  position: absolute; inset: 0; overflow: hidden; pointer-events: none;
}
.particle {
  position: absolute; width: 2px; height: 2px;
  background: var(--teal); border-radius: 50%;
  opacity: 0;
  animation: float-up 8s ease-in-out infinite;
}
@keyframes float-up {
  0% { opacity: 0; transform: translateY(100vh) scale(0); }
  20% { opacity: 0.6; }
  80% { opacity: 0.3; }
  100% { opacity: 0; transform: translateY(-10vh) scale(1); }
}

/* ═══════════════════════════════════════════════════════════
   SLIDE 2 — MARKET CAP
   ═══════════════════════════════════════════════════════════ */
.slide2-header {
  display: flex; justify-content: space-between;
  align-items: center; flex-wrap: wrap; gap: 12px;
  margin-bottom: 16px;
}

/* ═══════════════════════════════════════════════════════════
   SLIDE 3 — MONETARY POLICY COMPASS
   ═══════════════════════════════════════════════════════════ */
.slide3-controls {
  display: flex; gap: 16px; align-items: center;
  flex-wrap: wrap; margin-bottom: 16px;
}
.year-slider-wrap {
  display: flex; align-items: center; gap: 10px;
}
.year-slider-wrap label {
  font-size: 12px; color: var(--text-muted); font-weight: 500;
}
input[type="range"] {
  -webkit-appearance: none; width: 180px; height: 4px;
  background: var(--border); border-radius: 2px; outline: none;
}
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none; width: 16px; height: 16px;
  background: var(--teal); border-radius: 50%; cursor: pointer;
  box-shadow: 0 0 8px rgba(0,212,170,0.5);
}
.year-display {
  font-size: 14px; font-weight: 700; color: var(--teal);
  min-width: 40px;
}

/* ═══════════════════════════════════════════════════════════
   SLIDE 4 — TIMELINE
   ═══════════════════════════════════════════════════════════ */
.timeline-container {
  position: relative; overflow-x: auto; overflow-y: visible;
  padding: 20px 0 40px;
  scrollbar-width: thin;
  scroll-behavior: smooth;
}
.timeline-track {
  display: flex; gap: 0; min-width: max-content;
  position: relative; padding: 80px 40px 0;
}
.timeline-track::before {
  content: '';
  position: absolute; top: 80px; left: 20px; right: 20px;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--border) 5%, var(--border) 95%, transparent);
}
.timeline-event {
  position: relative; width: 140px; flex-shrink: 0;
  cursor: pointer; transition: var(--transition);
}
.timeline-event .dot {
  width: 14px; height: 14px; border-radius: 50%;
  position: absolute; top: -7px; left: 50%;
  transform: translateX(-50%);
  border: 2px solid var(--bg);
  z-index: 2; transition: var(--transition);
}
.timeline-event:hover .dot { transform: translateX(-50%) scale(1.4); }
.timeline-event .year-label {
  text-align: center; font-size: 12px; font-weight: 700;
  margin-top: 16px; color: var(--text);
}
.timeline-event .event-title {
  text-align: center; font-size: 11px; color: var(--text-muted);
  margin-top: 6px; line-height: 1.3; padding: 0 4px;
}
.timeline-event .detail-card {
  position: absolute; bottom: calc(100% + 20px); left: 50%;
  transform: translateX(-50%); width: 260px;
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius-sm); padding: 14px;
  font-size: 12px; color: var(--text-muted);
  opacity: 0; visibility: hidden; pointer-events: none;
  transition: var(--transition); z-index: 10;
  line-height: 1.5; box-shadow: 0 8px 32px rgba(0,0,0,0.4);
}
.timeline-event:hover .detail-card {
  opacity: 1; visibility: visible;
}
.timeline-event .detail-card .dc-type {
  font-size: 10px; text-transform: uppercase;
  font-weight: 600; letter-spacing: 0.06em;
  margin-bottom: 6px;
}

/* ═══════════════════════════════════════════════════════════
   SLIDE 5 — CBDC TRACKER
   ═══════════════════════════════════════════════════════════ */
.cbdc-layout {
  display: grid; grid-template-columns: 1fr 340px; gap: 24px;
}
.cbdc-map-area { position: relative; }
.cbdc-grid {
  display: grid; grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
  gap: 10px;
}
.cbdc-dot-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius-sm); padding: 12px;
  cursor: pointer; transition: var(--transition);
  text-align: center;
}
.cbdc-dot-card:hover { border-color: var(--teal); transform: translateY(-2px); }
.cbdc-dot-card.active { border-color: var(--teal); box-shadow: var(--glow-teal); }
.cbdc-dot-card .status-dot {
  width: 10px; height: 10px; border-radius: 50%;
  margin: 0 auto 6px; display: block;
}
.cbdc-dot-card .country-name {
  font-size: 12px; font-weight: 600; color: var(--text);
}
.cbdc-dot-card .status-label {
  font-size: 10px; color: var(--text-muted); margin-top: 2px;
}
.cbdc-detail-panel {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 20px;
  min-height: 300px;
}
.cbdc-detail-panel h3 {
  font-size: 18px; color: var(--teal); margin-bottom: 4px;
}
.cbdc-detail-panel .currency-name {
  font-size: 13px; color: var(--gold); margin-bottom: 16px;
}
.cbdc-detail-row {
  display: flex; justify-content: space-between;
  padding: 8px 0; border-bottom: 1px solid var(--border);
  font-size: 12px;
}
.cbdc-detail-row .lbl { color: var(--text-muted); }
.cbdc-detail-row .val { color: var(--text); font-weight: 500; }
.cbdc-status-legend {
  display: flex; gap: 16px; margin-bottom: 16px; flex-wrap: wrap;
}
.cbdc-legend-item {
  display: flex; align-items: center; gap: 6px; font-size: 11px; color: var(--text-muted);
}
.cbdc-legend-dot { width: 8px; height: 8px; border-radius: 50%; }

/* ═══════════════════════════════════════════════════════════
   SLIDE 6 — INTEREST RATES
   ═══════════════════════════════════════════════════════════ */
.slide6-legend-note {
  display: flex; gap: 24px; margin-top: 12px; font-size: 11px; color: var(--text-muted);
}
.slide6-legend-note span { display: flex; align-items: center; gap: 6px; }
.solid-line { width: 20px; height: 2px; background: var(--text-muted); }
.dashed-line { width: 20px; height: 2px; border-top: 2px dashed var(--text-muted); }

/* ═══════════════════════════════════════════════════════════
   SLIDE 7 — FINTECH INVESTMENT
   ═══════════════════════════════════════════════════════════ */
.slide7-layout {
  display: grid; grid-template-columns: 340px 1fr; gap: 24px;
}
.bubble-viz {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 20px;
  display: flex; flex-wrap: wrap; align-items: center;
  justify-content: center; gap: 8px;
  position: relative; min-height: 320px;
}
.bubble-circle {
  border-radius: 50%; display: flex;
  align-items: center; justify-content: center;
  font-size: 9px; font-weight: 600; color: var(--bg);
  cursor: pointer; transition: var(--transition);
  position: relative;
}
.bubble-circle:hover { transform: scale(1.1); z-index: 2; }
.bubble-label {
  position: absolute; bottom: -16px; left: 50%;
  transform: translateX(-50%); font-size: 9px;
  color: var(--text-muted); white-space: nowrap;
}

/* ═══════════════════════════════════════════════════════════
   SLIDE 8 — REGULATORY SANDBOXES
   ═══════════════════════════════════════════════════════════ */
.sandbox-grid {
  display: grid; grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}
.sandbox-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 20px;
  transition: var(--transition); cursor: pointer;
}
.sandbox-card:hover {
  border-color: var(--teal); transform: translateY(-4px);
  box-shadow: var(--glow-teal);
}
.sandbox-card .flag { font-size: 28px; margin-bottom: 8px; }
.sandbox-card .country { font-size: 16px; font-weight: 700; color: var(--text); }
.sandbox-card .regulator { font-size: 12px; color: var(--text-muted); margin-bottom: 10px; }
.sandbox-card .stat-row {
  display: flex; justify-content: space-between;
  font-size: 12px; padding: 4px 0;
}
.sandbox-card .stat-row .k { color: var(--text-muted); }
.sandbox-card .stat-row .v { color: var(--text); font-weight: 500; }
.openness-bar {
  height: 4px; background: var(--border); border-radius: 2px;
  margin-top: 8px; overflow: hidden;
}
.openness-fill {
  height: 100%; background: var(--teal); border-radius: 2px;
  transition: width 0.8s ease;
}
.sector-tags {
  display: flex; flex-wrap: wrap; gap: 4px; margin-top: 8px;
}
.sector-tag {
  font-size: 10px; padding: 2px 8px; border-radius: 10px;
  background: rgba(0,212,170,0.1); color: var(--teal);
  border: 1px solid rgba(0,212,170,0.2);
}
.model-badge {
  display: inline-block; font-size: 10px; padding: 2px 10px;
  border-radius: 10px; margin-top: 8px; font-weight: 600;
  text-transform: uppercase; letter-spacing: 0.04em;
}
.model-badge.sandbox { background: rgba(0,212,170,0.12); color: var(--teal); }
.model-badge.innovation_hub { background: rgba(245,166,35,0.12); color: var(--gold); }
.success-companies {
  margin-top: 10px; font-size: 11px; color: var(--text-muted); line-height: 1.5;
}
.success-companies strong { color: var(--text); font-weight: 500; }

/* ═══════════════════════════════════════════════════════════
   SLIDE 9 — CURRENCY VOLATILITY HEATMAP
   ═══════════════════════════════════════════════════════════ */
.heatmap-grid {
  display: grid;
  grid-template-columns: 60px repeat(10, 1fr);
  gap: 3px;
}
.heatmap-header {
  font-size: 11px; font-weight: 600; color: var(--text-muted);
  text-align: center; padding: 8px 4px;
}
.heatmap-row-label {
  font-size: 12px; font-weight: 600; color: var(--text);
  display: flex; align-items: center; padding-right: 8px;
}
.heatmap-cell {
  aspect-ratio: 1.5; border-radius: 4px;
  display: flex; align-items: center; justify-content: center;
  font-size: 11px; font-weight: 600;
  cursor: pointer; transition: var(--transition);
  position: relative;
}
.heatmap-cell:hover {
  transform: scale(1.1); z-index: 2;
  box-shadow: 0 0 12px rgba(0,0,0,0.5);
}
.heatmap-cell .ht-tooltip {
  position: absolute; bottom: calc(100% + 8px);
  left: 50%; transform: translateX(-50%);
  background: var(--surface); border: 1px solid var(--border);
  border-radius: 6px; padding: 8px 12px;
  font-size: 11px; color: var(--text-muted);
  white-space: nowrap; opacity: 0; pointer-events: none;
  transition: opacity 0.2s; z-index: 10;
  box-shadow: 0 4px 16px rgba(0,0,0,0.4);
}
.heatmap-cell:hover .ht-tooltip { opacity: 1; }
.heatmap-legend {
  display: flex; align-items: center; gap: 8px;
  margin-top: 16px; justify-content: center;
}
.heatmap-legend-bar {
  width: 200px; height: 8px; border-radius: 4px;
  background: linear-gradient(to right, #0d6b4f, #f5a623, #e07b3c, #e53e3e);
}
.heatmap-legend span { font-size: 10px; color: var(--text-muted); }

/* ═══════════════════════════════════════════════════════════
   SLIDE 10 — PAYMENTS
   ═══════════════════════════════════════════════════════════ */
.callout-cards {
  display: flex; gap: 16px; margin-top: 16px;
}
.callout-card {
  flex: 1; background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm); padding: 14px;
}
.callout-card .cc-value {
  font-size: 22px; font-weight: 800; color: var(--teal);
}
.callout-card .cc-label {
  font-size: 12px; color: var(--text-muted); margin-top: 4px;
}

/* ═══════════════════════════════════════════════════════════
   SLIDE 11 — POLICY MATRIX
   ═══════════════════════════════════════════════════════════ */
.matrix-quadrant-labels {
  position: relative;
}

/* ═══════════════════════════════════════════════════════════
   SLIDE 12 — CONCLUSION
   ═══════════════════════════════════════════════════════════ */
.stat-cards {
  display: grid; grid-template-columns: repeat(5, 1fr);
  gap: 16px; margin-bottom: 32px;
}
.stat-card {
  background: rgba(26, 35, 50, 0.6);
  backdrop-filter: blur(16px);
  border: 1px solid var(--border);
  border-radius: var(--radius); padding: 24px;
  text-align: center;
  transition: var(--transition);
}
.stat-card:hover {
  border-color: var(--teal);
  transform: translateY(-4px);
  box-shadow: var(--glow-teal);
}
.stat-card .icon { font-size: 28px; margin-bottom: 8px; }
.stat-card .stat-num {
  font-size: clamp(28px, 2.5vw, 40px);
  font-weight: 800; color: var(--teal);
  font-variant-numeric: tabular-nums;
}
.stat-card .stat-label {
  font-size: 12px; color: var(--text-muted);
  margin-top: 6px; font-weight: 500;
}
.stat-card .stat-desc {
  font-size: 11px; color: var(--text-dim);
  margin-top: 4px;
}
.key-insights {
  display: flex; flex-direction: column; gap: 12px;
  margin-bottom: 28px;
}
.key-insight {
  border-left: 3px solid var(--teal); padding: 12px 16px;
  background: rgba(0,212,170,0.04);
  border-radius: 0 var(--radius-sm) var(--radius-sm) 0;
  font-size: 13px; color: var(--text-muted); line-height: 1.6;
}
.action-buttons {
  display: flex; gap: 12px; justify-content: center;
}
.action-btn {
  padding: 12px 28px; border-radius: var(--radius-sm);
  font-size: 14px; font-weight: 600; cursor: pointer;
  transition: var(--transition); font-family: inherit;
  border: none;
}
.action-btn.primary {
  background: var(--teal); color: var(--bg);
}
.action-btn.primary:hover { box-shadow: var(--glow-teal); transform: translateY(-2px); }
.action-btn.secondary {
  background: transparent; color: var(--teal);
  border: 1px solid var(--teal);
}
.action-btn.secondary:hover { background: rgba(0,212,170,0.1); }

/* Sources modal */
.sources-modal {
  position: fixed; inset: 0;
  background: rgba(15,25,35,0.85);
  backdrop-filter: blur(12px);
  z-index: 300; display: none;
  align-items: center; justify-content: center;
}
.sources-modal.show { display: flex; }
.sources-content {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 32px;
  max-width: 520px; width: 90%; max-height: 70vh; overflow-y: auto;
}
.sources-content h3 { font-size: 18px; margin-bottom: 16px; color: var(--teal); }
.sources-content ul { list-style: none; }
.sources-content li {
  padding: 8px 0; border-bottom: 1px solid var(--border);
  font-size: 13px; color: var(--text-muted);
}
.sources-content li::before { content: "📊 "; }
.sources-close {
  position: absolute; top: 16px; right: 20px;
  background: none; border: none; color: var(--text-muted);
  font-size: 24px; cursor: pointer;
}

/* ─── Print styles ─── */
@media print {
  .nav-arrow, .slide-counter, .progress-bar, .slide-menu { display: none !important; }
  .slide { position: relative !important; opacity: 1 !important; visibility: visible !important;
    transform: none !important; page-break-after: always; overflow: visible !important; }
  body { overflow: visible !important; }
  .presentation { overflow: visible !important; }
}

/* ─── Responsive ─── */
@media (max-width: 1024px) {
  .slide-content { padding: 50px 24px 70px; }
  .cbdc-layout { grid-template-columns: 1fr; }
  .slide7-layout { grid-template-columns: 1fr; }
  .sandbox-grid { grid-template-columns: repeat(2, 1fr); }
  .stat-cards { grid-template-columns: repeat(3, 1fr); }
  .slide-menu-grid { grid-template-columns: repeat(3, 1fr); }
}
@media (max-width: 768px) {
  .slide-content { padding: 40px 16px 60px; }
  .sandbox-grid { grid-template-columns: 1fr; }
  .stat-cards { grid-template-columns: repeat(2, 1fr); }
  .title-stats { gap: 24px; }
  .callout-cards { flex-direction: column; }
  .heatmap-grid { overflow-x: auto; }
  .slide-menu-grid { grid-template-columns: repeat(2, 1fr); }
}
</style>
</head>
<body>

<div class="presentation" id="presentation">
  <!-- ══════════════ SLIDE 1 — TITLE ══════════════ -->
  <div class="slide active" id="slide-1">
    <div class="particles" id="particles"></div>
    <div class="world-map-bg">
      <svg viewBox="0 0 1000 500" xmlns="http://www.w3.org/2000/svg">
        <!-- Simplified continent outlines -->
        <path d="M180,120 Q200,100 240,110 Q270,105 290,120 Q300,140 280,160 Q260,180 240,190 Q220,195 200,185 Q185,170 180,150 Z" fill="#1A2332" opacity="0.5"/>
        <path d="M170,200 Q190,190 230,200 Q260,210 280,240 Q300,280 290,320 Q270,360 240,370 Q210,365 195,340 Q180,300 175,260 Z" fill="#1A2332" opacity="0.5"/>
        <path d="M440,100 Q500,80 560,90 Q600,100 580,140 Q560,160 520,170 Q480,175 450,160 Q430,140 440,100 Z" fill="#1A2332" opacity="0.5"/>
        <path d="M480,180 Q520,170 560,180 Q580,200 570,240 Q550,280 520,290 Q490,285 480,260 Q470,230 480,180 Z" fill="#1A2332" opacity="0.5"/>
        <path d="M600,100 Q700,80 800,100 Q850,130 840,170 Q820,200 780,190 Q740,180 700,200 Q680,220 650,210 Q620,190 610,160 Q600,130 600,100 Z" fill="#1A2332" opacity="0.5"/>
        <path d="M700,210 Q750,200 790,220 Q810,250 800,280 Q780,310 750,320 Q720,315 710,290 Q700,260 700,210 Z" fill="#1A2332" opacity="0.5"/>
        <path d="M850,300 Q900,280 940,300 Q960,340 940,380 Q910,400 880,390 Q850,370 850,340 Z" fill="#1A2332" opacity="0.5"/>
        <!-- Country dots -->
        <circle cx="230" cy="140" r="5" class="map-dot" style="animation-delay:0s"/><!-- USA -->
        <circle cx="470" cy="130" r="4" class="map-dot gold" style="animation-delay:0.3s"/><!-- UK -->
        <circle cx="490" cy="145" r="4" class="map-dot" style="animation-delay:0.6s"/><!-- EU -->
        <circle cx="730" cy="150" r="5" class="map-dot blue" style="animation-delay:0.9s"/><!-- China -->
        <circle cx="680" cy="200" r="4" class="map-dot gold" style="animation-delay:1.2s"/><!-- India -->
        <circle cx="260" cy="310" r="4" class="map-dot" style="animation-delay:1.5s"/><!-- Brazil -->
        <circle cx="720" cy="230" r="3" class="map-dot blue" style="animation-delay:1.8s"/><!-- Singapore -->
        <circle cx="620" cy="180" r="3" class="map-dot gold" style="animation-delay:2.1s"/><!-- UAE -->
        <circle cx="800" cy="150" r="4" class="map-dot" style="animation-delay:0.4s"/><!-- Japan -->
        <circle cx="510" cy="250" r="3" class="map-dot blue" style="animation-delay:0.7s"/><!-- Nigeria -->
        <circle cx="530" cy="290" r="3" class="map-dot" style="animation-delay:1.0s"/><!-- S.Africa -->
      </svg>
    </div>
    <div class="slide-content">
      <h1 class="title-main">How Nations Rebalance:<br>Global Fintech &amp; Market Policy Shifts 2004–2024</h1>
      <p class="title-sub">An interactive analysis of monetary strategy, regulation, and digital finance adoption</p>
      <div class="title-stats">
        <div class="title-stat">
          <div class="num" data-target="195" data-suffix="">0</div>
          <div class="label">Countries Tracked</div>
        </div>
        <div class="title-stat">
          <div class="num gold" data-target="105" data-prefix="$" data-suffix="T">$0T</div>
          <div class="label">Global Market Cap</div>
        </div>
        <div class="title-stat">
          <div class="num blue" data-target="48" data-suffix="">0</div>
          <div class="label">Policy Pivots Analyzed</div>
        </div>
      </div>
      <div class="keyboard-hints">
        <div class="key-hint"><kbd>←</kbd><kbd>→</kbd> Navigate</div>
        <div class="key-hint"><kbd>ESC</kbd> Slide Menu</div>
      </div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 2 — MARKET CAP ══════════════ -->
  <div class="slide" id="slide-2">
    <div class="slide-content">
      <div class="slide2-header">
        <div>
          <h2 class="slide-title">The Big Picture: Global Market Cap Shifts</h2>
          <p class="slide-subtitle">Equity market capitalization by region, 2004–2024</p>
        </div>
        <div class="toggle-group" id="s2-toggle">
          <button class="toggle-btn active" data-mode="absolute">Absolute ($T)</button>
          <button class="toggle-btn" data-mode="percent">% of World</button>
        </div>
      </div>
      <div class="chart-container" style="height: 420px;">
        <canvas id="chart-slide2"></canvas>
      </div>
      <div class="insight-box">
        <strong>Key Insight:</strong> Asia-Pacific's share grew from <strong>22% → 31%</strong> of global equity market cap, while North America maintained dominance at ~42%. Emerging markets tripled in absolute terms from $2.1T to $9T.
      </div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 3 — MONETARY POLICY COMPASS ══════════════ -->
  <div class="slide" id="slide-3">
    <div class="slide-content">
      <h2 class="slide-title">Monetary Policy Compass</h2>
      <p class="slide-subtitle">Central bank interest rate vs. inflation rate — bubble size represents GDP</p>
      <div class="slide3-controls">
        <div class="year-slider-wrap">
          <label>Year:</label>
          <input type="range" id="s3-slider" min="0" max="3" step="1" value="3">
          <span class="year-display" id="s3-year">2023</span>
        </div>
        <div class="filter-group" id="s3-filters">
          <button class="filter-btn active" data-region="all">All Regions</button>
          <button class="filter-btn" data-region="north_america">N. America</button>
          <button class="filter-btn" data-region="europe">Europe</button>
          <button class="filter-btn" data-region="asia_pacific">Asia-Pacific</button>
          <button class="filter-btn" data-region="emerging">Emerging</button>
        </div>
      </div>
      <div class="chart-container" style="height: 400px;">
        <canvas id="chart-slide3"></canvas>
      </div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 4 — TIMELINE ══════════════ -->
  <div class="slide" id="slide-4">
    <div class="slide-content">
      <h2 class="slide-title">The Fintech Regulatory Timeline</h2>
      <p class="slide-subtitle">Key milestones in global fintech policy and innovation, 2008–2024</p>
      <div class="filter-group" id="s4-filters">
        <button class="filter-btn active" data-type="all">All</button>
        <button class="filter-btn" data-type="regulation" style="--dot-color: #F5A623">⬤ Regulation</button>
        <button class="filter-btn" data-type="technology" style="--dot-color: #4E9EF5">⬤ Technology</button>
        <button class="filter-btn" data-type="crisis" style="--dot-color: #FF6B6B">⬤ Crisis</button>
        <button class="filter-btn" data-type="market" style="--dot-color: #00D4AA">⬤ Market</button>
      </div>
      <div class="timeline-container" id="timeline-container">
        <div class="timeline-track" id="timeline-track"></div>
      </div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 5 — CBDC TRACKER ══════════════ -->
  <div class="slide" id="slide-5">
    <div class="slide-content">
      <h2 class="slide-title">CBDC Tracker: Central Bank Digital Currencies</h2>
      <p class="slide-subtitle">Global status of central bank digital currency initiatives as of 2024</p>
      <div class="cbdc-status-legend">
        <div class="cbdc-legend-item"><span class="cbdc-legend-dot" style="background:#34D399"></span> Launched</div>
        <div class="cbdc-legend-item"><span class="cbdc-legend-dot" style="background:#00D4AA"></span> Pilot</div>
        <div class="cbdc-legend-item"><span class="cbdc-legend-dot" style="background:#F5A623"></span> Research</div>
      </div>
      <div style="margin-bottom:16px">
        <div class="toggle-group" id="s5-toggle">
          <button class="toggle-btn active" data-mode="all">All CBDCs</button>
          <button class="toggle-btn" data-mode="retail">Retail CBDC</button>
          <button class="toggle-btn" data-mode="wholesale">Wholesale CBDC</button>
        </div>
      </div>
      <div class="cbdc-layout">
        <div class="cbdc-map-area">
          <div class="cbdc-grid" id="cbdc-grid"></div>
          <div style="margin-top:20px">
            <h4 style="font-size:13px;color:var(--text-muted);margin-bottom:10px">Adoption Rate (%)</h4>
            <div class="chart-container" style="height:220px">
              <canvas id="chart-slide5"></canvas>
            </div>
          </div>
        </div>
        <div class="cbdc-detail-panel" id="cbdc-detail">
          <div style="color:var(--text-muted);font-size:13px;padding:60px 0;text-align:center">
            ← Click a country to view CBDC details
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 6 — INTEREST RATE WARS ══════════════ -->
  <div class="slide" id="slide-6">
    <div class="slide-content">
      <h2 class="slide-title">Interest Rate Wars: G7 vs BRICS</h2>
      <p class="slide-subtitle">Central bank policy rates 2010–2024 — solid lines: G7, dashed lines: BRICS</p>
      <div class="chart-container" style="height: 420px;">
        <canvas id="chart-slide6"></canvas>
      </div>
      <div class="slide6-legend-note">
        <span><span class="solid-line"></span> G7 Nations</span>
        <span><span class="dashed-line"></span> BRICS Nations</span>
      </div>
      <div class="insight-box">
        <strong>Key Divergence:</strong> BRICS nations consistently maintained higher rates (4–15%) while G7 nations went near-zero post-2008. The post-COVID hiking cycle saw unprecedented convergence — with the Fed reaching 5.5% while Brazil peaked at 13.75%.
      </div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 7 — FINTECH INVESTMENT ══════════════ -->
  <div class="slide" id="slide-7">
    <div class="slide-content">
      <h2 class="slide-title">Fintech Investment Heat Map</h2>
      <p class="slide-subtitle">Global venture capital investment in fintech by country, 2015–2023</p>
      <div class="slide3-controls">
        <div class="year-slider-wrap">
          <label>Year:</label>
          <input type="range" id="s7-slider" min="2015" max="2023" step="1" value="2023">
          <span class="year-display" id="s7-year">All Years</span>
        </div>
        <button class="toggle-btn active" id="s7-cumulative" onclick="toggleS7Cumulative()">Cumulative</button>
      </div>
      <div class="slide7-layout">
        <div class="bubble-viz" id="bubble-viz"></div>
        <div class="chart-container" style="flex:1">
          <canvas id="chart-slide7"></canvas>
        </div>
      </div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 8 — REGULATORY SANDBOXES ══════════════ -->
  <div class="slide" id="slide-8">
    <div class="slide-content">
      <h2 class="slide-title">Regulatory Sandbox Comparison</h2>
      <p class="slide-subtitle">How nations foster fintech innovation through regulatory experimentation</p>
      <div class="filter-group" id="s8-filters">
        <button class="filter-btn active" data-sort="all">All</button>
        <button class="filter-btn" data-sort="open">Most Open</button>
        <button class="filter-btn" data-sort="largest">Largest</button>
        <button class="filter-btn" data-sort="newest">Newest</button>
        <button class="filter-btn" data-sort="sandbox">Sandbox Only</button>
        <button class="filter-btn" data-sort="hub">Innovation Hub</button>
      </div>
      <div class="sandbox-grid" id="sandbox-grid"></div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 9 — CURRENCY VOLATILITY ══════════════ -->
  <div class="slide" id="slide-9">
    <div class="slide-content">
      <h2 class="slide-title">Currency Volatility Index</h2>
      <p class="slide-subtitle">Annualized volatility scores by currency (0–100). Higher = more volatile.</p>
      <div class="filter-group" id="s9-filters">
        <button class="filter-btn active" data-group="all">Show All</button>
        <button class="filter-btn" data-group="g7">G7 Only</button>
        <button class="filter-btn" data-group="emerging">Emerging Only</button>
      </div>
      <div class="chart-container" style="padding:16px; overflow-x:auto">
        <div class="heatmap-grid" id="heatmap-grid"></div>
        <div class="heatmap-legend">
          <span>Stable</span>
          <div class="heatmap-legend-bar"></div>
          <span>Volatile</span>
        </div>
      </div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 10 — PAYMENTS INFRASTRUCTURE ══════════════ -->
  <div class="slide" id="slide-10">
    <div class="slide-content">
      <h2 class="slide-title">Payments Infrastructure Race</h2>
      <p class="slide-subtitle">Payment system adoption by country (% of transactions)</p>
      <div style="margin-bottom:16px">
        <div class="toggle-group" id="s10-toggle">
          <button class="toggle-btn" data-year="2019">2019</button>
          <button class="toggle-btn active" data-year="2024">2024</button>
        </div>
      </div>
      <div class="chart-container" style="height:400px">
        <canvas id="chart-slide10"></canvas>
      </div>
      <div class="callout-cards">
        <div class="callout-card">
          <div class="cc-value">117B</div>
          <div class="cc-label">India's UPI processed 117 billion transactions in 2023 — the world's largest real-time payment system</div>
        </div>
        <div class="callout-card">
          <div class="cc-value" style="color:var(--gold)">90%</div>
          <div class="cc-label">China's Alipay & WeChat Pay dominate 90% of all mobile payments in the country</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 11 — POLICY MATRIX ══════════════ -->
  <div class="slide" id="slide-11">
    <div class="slide-content">
      <h2 class="slide-title">Policy Response Matrix</h2>
      <p class="slide-subtitle">Countries mapped by speed of policy response vs. market openness</p>
      <div class="chart-container" style="height:440px; position:relative">
        <canvas id="chart-slide11"></canvas>
      </div>
      <div class="insight-box">
        <strong>Quadrant Analysis:</strong> Singapore, UK, and UAE lead as "Fast & Open" regulators. China demonstrates rapid policy response but with restrictive market access. The US and EU, despite large markets, are categorized as slower responders.
      </div>
    </div>
  </div>

  <!-- ══════════════ SLIDE 12 — CONCLUSION ══════════════ -->
  <div class="slide" id="slide-12">
    <div class="slide-content">
      <h2 class="slide-title" style="text-align:center">Key Takeaways & The Road Ahead</h2>
      <p class="slide-subtitle" style="text-align:center">The numbers that define the future of global finance</p>
      <div class="stat-cards" id="stat-cards">
        <div class="stat-card">
          <div class="icon">🌍</div>
          <div class="stat-num" data-target="134" data-suffix="" data-decimals="0">0</div>
          <div class="stat-label">Countries Exploring CBDCs</div>
          <div class="stat-desc">Up from just 35 in 2020</div>
        </div>
        <div class="stat-card">
          <div class="icon">💰</div>
          <div class="stat-num" data-target="4.8" data-suffix="T" data-decimals="1">0</div>
          <div class="stat-label">Fintech Sector Value (2025)</div>
          <div class="stat-desc">In USD, projected global valuation</div>
        </div>
        <div class="stat-card">
          <div class="icon">📱</div>
          <div class="stat-num" data-target="48" data-suffix="%" data-decimals="0">0</div>
          <div class="stat-label">Sub-Saharan Africa Mobile Money</div>
          <div class="stat-desc">Of adults use mobile money services</div>
        </div>
        <div class="stat-card">
          <div class="icon">🏛️</div>
          <div class="stat-num" data-target="60" data-suffix="+" data-decimals="0">0</div>
          <div class="stat-label">Regulatory Sandboxes</div>
          <div class="stat-desc">Active globally — up from 1 in 2014</div>
        </div>
        <div class="stat-card">
          <div class="icon">⚡</div>
          <div class="stat-num" data-target="117" data-suffix="B" data-decimals="0">0</div>
          <div class="stat-label">India UPI Transactions</div>
          <div class="stat-desc">2023 annual transaction volume</div>
        </div>
      </div>
      <div class="key-insights">
        <div class="key-insight">The global financial architecture is undergoing its most significant transformation since Bretton Woods. Digital currencies, real-time payments, and decentralized finance are reshaping how value moves across borders.</div>
        <div class="key-insight">Regulatory sandboxes have proven their worth — the UK's FCA sandbox model has been replicated by 60+ jurisdictions, enabling controlled innovation while managing systemic risk.</div>
        <div class="key-insight">The rise of Asia-Pacific as a fintech powerhouse is reshaping capital flows. India's UPI and China's mobile payment ecosystems serve as blueprints for emerging market digital transformation.</div>
      </div>
      <div class="action-buttons">
        <button class="action-btn primary" onclick="window.print()">📄 Download Full Report</button>
        <button class="action-btn secondary" onclick="document.getElementById('sources-modal').classList.add('show')">📊 Explore Data Sources</button>
      </div>
    </div>
  </div>
</div>

<!-- Navigation -->
<button class="nav-arrow left" onclick="changeSlide(-1)" id="nav-left" aria-label="Previous slide">◂</button>
<button class="nav-arrow right" onclick="changeSlide(1)" id="nav-right" aria-label="Next slide">▸</button>
<div class="slide-counter" id="slide-counter">1 / 12</div>
<div class="progress-bar" id="progress-bar" style="width:8.33%"></div>

<!-- Slide Menu -->
<div class="slide-menu" id="slide-menu">
  <button class="slide-menu-close" onclick="toggleMenu()">&times;</button>
  <div class="slide-menu-grid">
    <div class="slide-menu-item" onclick="goToSlide(1);toggleMenu()"><div class="num">01</div><div class="label">Title</div></div>
    <div class="slide-menu-item" onclick="goToSlide(2);toggleMenu()"><div class="num">02</div><div class="label">Market Cap Shifts</div></div>
    <div class="slide-menu-item" onclick="goToSlide(3);toggleMenu()"><div class="num">03</div><div class="label">Monetary Policy</div></div>
    <div class="slide-menu-item" onclick="goToSlide(4);toggleMenu()"><div class="num">04</div><div class="label">Fintech Timeline</div></div>
    <div class="slide-menu-item" onclick="goToSlide(5);toggleMenu()"><div class="num">05</div><div class="label">CBDC Tracker</div></div>
    <div class="slide-menu-item" onclick="goToSlide(6);toggleMenu()"><div class="num">06</div><div class="label">Interest Rates</div></div>
    <div class="slide-menu-item" onclick="goToSlide(7);toggleMenu()"><div class="num">07</div><div class="label">Fintech Investment</div></div>
    <div class="slide-menu-item" onclick="goToSlide(8);toggleMenu()"><div class="num">08</div><div class="label">Sandboxes</div></div>
    <div class="slide-menu-item" onclick="goToSlide(9);toggleMenu()"><div class="num">09</div><div class="label">Currency Volatility</div></div>
    <div class="slide-menu-item" onclick="goToSlide(10);toggleMenu()"><div class="num">10</div><div class="label">Payments</div></div>
    <div class="slide-menu-item" onclick="goToSlide(11);toggleMenu()"><div class="num">11</div><div class="label">Policy Matrix</div></div>
    <div class="slide-menu-item" onclick="goToSlide(12);toggleMenu()"><div class="num">12</div><div class="label">Conclusion</div></div>
  </div>
</div>

<!-- Sources Modal -->
<div class="sources-modal" id="sources-modal">
  <button class="sources-close" onclick="document.getElementById('sources-modal').classList.remove('show')">&times;</button>
  <div class="sources-content">
    <h3>Data Sources</h3>
    <ul>
      <li>World Bank — Global Financial Development Database</li>
      <li>International Monetary Fund (IMF) — World Economic Outlook</li>
      <li>Bank for International Settlements (BIS) — Central Bank Survey</li>
      <li>Atlantic Council — CBDC Tracker</li>
      <li>CB Insights — State of Fintech Report (2015–2024)</li>
      <li>KPMG — Pulse of Fintech (Biannual)</li>
      <li>Financial Conduct Authority (FCA) — Regulatory Sandbox Reports</li>
      <li>Monetary Authority of Singapore (MAS) — Fintech Publications</li>
      <li>Reserve Bank of India (RBI) — Digital Payments Statistics</li>
      <li>NPCI — Unified Payments Interface (UPI) Transaction Data</li>
      <li>European Central Bank — Digital Euro Project Updates</li>
      <li>People's Bank of China — e-CNY Pilot Reports</li>
      <li>World Economic Forum — Global Financial Stability Reports</li>
    </ul>
  </div>
</div>

<!-- Popup for country detail clicks -->
<div class="popup-overlay" id="country-popup">
  <div class="popup-card" id="country-popup-card">
    <button class="popup-close" onclick="document.getElementById('country-popup').classList.remove('show')">&times;</button>
    <div id="country-popup-content"></div>
  </div>
</div>

<script>
/* ═══════════════════════════════════════════════════════════
   DATA
   ═══════════════════════════════════════════════════════════ */
const DATA = {
  marketCap: {
    labels: [2004,2006,2008,2010,2012,2014,2016,2018,2020,2022,2024],
    regions: {
      'North America': [14.5,18.2,12.1,17.1,20.6,25.8,27.4,30.1,37.5,32.0,42.0],
      'Europe':        [10.2,14.4,8.5,11.0,10.3,12.4,11.7,12.0,13.2,11.5,14.0],
      'Asia-Pacific':  [8.1,11.6,7.9,13.4,14.1,16.5,17.2,18.8,25.4,22.0,31.0],
      'Emerging':      [2.1,4.3,2.8,5.1,5.4,6.2,5.9,6.3,8.0,7.2,9.0],
      'ME & Africa':   [0.9,1.4,1.0,1.3,1.5,1.8,2.0,2.4,3.1,3.5,4.5]
    }
  },
  bubbleYears: {
    2008: [
      {country:'USA',x:2.0,y:3.8,gdp:14.7,region:'north_america'},
      {country:'EU',x:4.0,y:3.3,gdp:14.2,region:'europe'},
      {country:'China',x:7.47,y:5.9,gdp:4.6,region:'asia_pacific'},
      {country:'India',x:8.0,y:8.3,gdp:1.2,region:'asia_pacific'},
      {country:'Brazil',x:13.75,y:5.7,gdp:1.7,region:'emerging'},
      {country:'Turkey',x:16.75,y:10.4,gdp:0.8,region:'emerging'},
      {country:'Japan',x:0.50,y:1.4,gdp:5.0,region:'asia_pacific'},
      {country:'UK',x:5.0,y:3.6,gdp:2.9,region:'europe'},
      {country:'Saudi Arabia',x:5.5,y:9.9,gdp:0.5,region:'emerging'},
      {country:'South Africa',x:12.0,y:11.5,gdp:0.3,region:'emerging'}
    ],
    2015: [
      {country:'USA',x:0.50,y:0.1,gdp:18.2,region:'north_america'},
      {country:'EU',x:0.05,y:0.2,gdp:11.6,region:'europe'},
      {country:'China',x:4.85,y:1.4,gdp:11.1,region:'asia_pacific'},
      {country:'India',x:6.75,y:4.9,gdp:2.1,region:'asia_pacific'},
      {country:'Brazil',x:14.25,y:9.0,gdp:1.8,region:'emerging'},
      {country:'Turkey',x:7.5,y:7.7,gdp:0.9,region:'emerging'},
      {country:'Japan',x:0.0,y:0.8,gdp:4.4,region:'asia_pacific'},
      {country:'UK',x:0.50,y:0.0,gdp:2.9,region:'europe'},
      {country:'Saudi Arabia',x:2.0,y:2.2,gdp:0.7,region:'emerging'},
      {country:'South Africa',x:6.25,y:4.6,gdp:0.3,region:'emerging'}
    ],
    2020: [
      {country:'USA',x:0.25,y:1.2,gdp:21.1,region:'north_america'},
      {country:'EU',x:0.0,y:0.3,gdp:13.3,region:'europe'},
      {country:'China',x:3.85,y:2.5,gdp:14.7,region:'asia_pacific'},
      {country:'India',x:4.0,y:6.2,gdp:2.7,region:'asia_pacific'},
      {country:'Brazil',x:2.0,y:3.2,gdp:1.4,region:'emerging'},
      {country:'Turkey',x:17.0,y:12.3,gdp:0.7,region:'emerging'},
      {country:'Japan',x:-0.10,y:0.0,gdp:5.0,region:'asia_pacific'},
      {country:'UK',x:0.10,y:0.9,gdp:2.7,region:'europe'},
      {country:'Saudi Arabia',x:1.0,y:3.4,gdp:0.7,region:'emerging'},
      {country:'South Africa',x:3.5,y:3.3,gdp:0.3,region:'emerging'}
    ],
    2023: [
      {country:'USA',x:5.50,y:3.4,gdp:27.4,region:'north_america'},
      {country:'EU',x:4.50,y:5.4,gdp:18.3,region:'europe'},
      {country:'China',x:3.45,y:0.2,gdp:17.8,region:'asia_pacific'},
      {country:'India',x:6.50,y:5.6,gdp:3.7,region:'asia_pacific'},
      {country:'Brazil',x:11.75,y:4.6,gdp:2.1,region:'emerging'},
      {country:'Turkey',x:17.0,y:53.9,gdp:1.1,region:'emerging'},
      {country:'Japan',x:0.10,y:2.5,gdp:4.2,region:'asia_pacific'},
      {country:'UK',x:5.25,y:7.3,gdp:3.1,region:'europe'},
      {country:'Saudi Arabia',x:6.0,y:2.1,gdp:1.1,region:'emerging'},
      {country:'South Africa',x:8.25,y:5.9,gdp:0.4,region:'emerging'}
    ]
  },
  timeline: [
    {year:2008,event:'Global Financial Crisis',type:'crisis',detail:'Lehman Brothers collapse triggers $22T in lost global wealth; Basel III reforms begin'},
    {year:2009,event:'Bitcoin Goes Live',type:'technology',detail:'Satoshi Nakamoto\'s Bitcoin goes live Jan 3, 2009 — block reward: 50 BTC'},
    {year:2010,event:'Dodd-Frank Act',type:'regulation',detail:'Most sweeping US financial reform since Great Depression; creates CFPB'},
    {year:2011,event:'EU PSD1 Directive',type:'regulation',detail:'First EU Payment Services Directive; mandates open banking groundwork'},
    {year:2013,event:'China P2P Boom',type:'market',detail:'China reaches 800+ P2P platforms; later collapses to near-zero by 2020'},
    {year:2014,event:'Alibaba IPO',type:'technology',detail:'Alibaba $25B IPO; Alipay reaches 300M users — largest digital payment platform'},
    {year:2015,event:'P2P Crackdown',type:'regulation',detail:'CBRC issues regulations after P2P fraud losses exceed $7B; thousands shut'},
    {year:2016,event:'FCA Sandbox',type:'regulation',detail:'World\'s first fintech regulatory sandbox launches; 18 companies in cohort 1'},
    {year:2017,event:'ICO Boom',type:'market',detail:'$5.6B raised via ICOs; China bans ICOs Sept 2017; SEC issues guidance'},
    {year:2018,event:'GDPR + PSD2',type:'regulation',detail:'EU data regulation + open banking directive reshape fintech landscape'},
    {year:2019,event:'Libra Announced',type:'technology',detail:'Facebook announces Libra stablecoin; unprecedented regulatory backlash'},
    {year:2020,event:'COVID Payment Surge',type:'market',detail:'Contactless payments up 40% globally; e-commerce +27% YoY; Sand Dollar launches'},
    {year:2021,event:'DeFi + China Ban',type:'market',detail:'DeFi TVL reaches $180B; China bans all crypto transactions Sept 2021'},
    {year:2022,event:'FTX Collapse',type:'crisis',detail:'$32B FTX valuation collapses in 72 hours; $8B customer funds missing'},
    {year:2023,event:'EU MiCA + SEC',type:'regulation',detail:'Markets in Crypto-Assets regulation passed; SEC charges Binance, Coinbase'},
    {year:2024,event:'CBDC Expansion',type:'technology',detail:'134 countries in CBDC exploration; Bitcoin ETFs approved; HK spot ETFs'}
  ],
  cbdc: [
    {name:'China',code:'CN',status:3,label:'Pilot',currency:'e-CNY / Digital Yuan',year:2020,adoption:11,tech:'Centralized, two-tier',notes:'Largest CBDC pilot globally — 260M wallets, 6 major cities',type:'retail'},
    {name:'Nigeria',code:'NG',status:4,label:'Launched',currency:'eNaira',year:2021,adoption:6,tech:'Hyperledger Fabric',notes:'First African nation to launch CBDC',type:'retail'},
    {name:'Bahamas',code:'BS',status:4,label:'Launched',currency:'Sand Dollar',year:2020,adoption:22,tech:'NZIA proprietary',notes:'World\'s first officially launched CBDC',type:'retail'},
    {name:'Jamaica',code:'JM',status:4,label:'Launched',currency:'JAM-DEX',year:2022,adoption:9,tech:'Bitt Inc.',notes:'Legal tender, government-backed wallets',type:'retail'},
    {name:'India',code:'IN',status:3,label:'Pilot',currency:'Digital Rupee (e₹)',year:2022,adoption:3,tech:'RBI-managed, blockchain',notes:'Retail + wholesale pilots underway',type:'retail'},
    {name:'EU',code:'EU',status:2,label:'Research',currency:'Digital Euro',year:null,adoption:0,tech:'ECB research phase',notes:'Preparation phase started Oct 2023',type:'retail'},
    {name:'USA',code:'US',status:2,label:'Research',currency:'FedNow / Digital Dollar',year:null,adoption:0,tech:'FRB research',notes:'Political opposition slowing progress',type:'retail'},
    {name:'UAE',code:'AE',status:3,label:'Pilot',currency:'Digital Dirham',year:2023,adoption:4,tech:'mBridge platform',notes:'Cross-border CBDC project with BIS',type:'wholesale'},
    {name:'Saudi Arabia',code:'SA',status:3,label:'Pilot',currency:'Project Aber',year:2020,adoption:2,tech:'Distributed ledger',notes:'Wholesale CBDC for interbank settlements',type:'wholesale'},
    {name:'Sweden',code:'SE',status:3,label:'Pilot',currency:'e-Krona',year:2020,adoption:7,tech:'Accenture/R3 Corda',notes:'Phase 3 pilot focused on offline payments',type:'retail'},
    {name:'South Korea',code:'KR',status:3,label:'Pilot',currency:'Digital Won',year:2023,adoption:1,tech:'BOK infrastructure',notes:'100,000 user pilot launched 2024',type:'retail'},
    {name:'Brazil',code:'BR',status:3,label:'Pilot',currency:'Drex',year:2023,adoption:2,tech:'Hyperledger Besu',notes:'Wholesale CBDC, DeFi integration planned',type:'wholesale'}
  ],
  rates: {
    years: [2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023,2024],
    g7: {
      USA:[0.25,0.25,0.25,0.25,0.25,0.50,0.75,1.50,2.50,1.75,0.25,0.25,4.50,5.50,5.25],
      EU:[1.00,1.25,0.75,0.25,0.05,0.05,0.00,0.00,0.00,0.00,0.00,0.00,2.50,4.50,4.00],
      UK:[0.50,0.50,0.50,0.50,0.50,0.50,0.25,0.50,0.75,0.75,0.10,0.25,3.50,5.25,5.00],
      Japan:[0.10,0.10,0.10,0.10,0.10,0.00,-0.10,-0.10,-0.10,-0.10,-0.10,-0.10,0.00,0.10,0.25]
    },
    brics: {
      China:[5.81,6.56,6.00,6.00,5.60,4.85,4.35,4.35,4.35,4.35,3.85,3.80,3.65,3.45,3.35],
      India:[6.25,8.50,8.00,7.75,8.00,6.75,6.25,6.00,6.50,5.15,4.00,4.00,6.25,6.50,6.50],
      Brazil:[10.75,11.00,7.25,10.00,11.75,14.25,13.75,7.00,6.50,4.50,2.00,9.25,13.75,11.75,10.50],
      Russia:[7.75,8.25,8.25,5.50,9.50,11.00,10.00,7.75,7.75,6.25,4.25,8.50,17.00,16.00,16.00],
      'S. Africa':[6.00,5.50,5.00,5.00,5.75,6.25,7.00,6.75,6.75,6.50,3.50,3.75,7.00,8.25,8.25]
    }
  },
  fintech: {
    years: [2015,2016,2017,2018,2019,2020,2021,2022,2023],
    countries: [
      {name:'USA',total:91.2,yearly:[5.1,7.3,9.2,14.0,13.3,11.5,23.5,12.5,8.0],color:'#00D4AA'},
      {name:'China',total:54.3,yearly:[4.2,6.8,8.3,10.3,9.1,5.4,6.3,4.1,3.0],color:'#F5A623'},
      {name:'UK',total:29.1,yearly:[1.5,1.9,2.6,3.3,3.8,3.1,6.7,4.5,3.2],color:'#4E9EF5'},
      {name:'India',total:24.4,yearly:[0.8,1.2,1.9,2.7,3.4,2.8,8.4,2.8,2.1],color:'#FF6B6B'},
      {name:'Brazil',total:9.1,yearly:[0.2,0.3,0.5,0.8,1.2,1.1,3.4,1.3,0.7],color:'#A78BFA'},
      {name:'Germany',total:8.2,yearly:[0.4,0.6,0.8,1.0,1.2,0.9,2.0,1.1,0.8],color:'#34D399'},
      {name:'Singapore',total:6.3,yearly:[0.3,0.4,0.7,0.8,0.9,0.7,1.3,1.0,0.7],color:'#FB923C'},
      {name:'Israel',total:5.1,yearly:[0.3,0.4,0.5,0.6,0.7,0.5,1.4,0.7,0.5],color:'#F472B6'},
      {name:'Canada',total:4.9,yearly:[0.3,0.4,0.5,0.7,0.7,0.5,1.2,0.6,0.5],color:'#60A5FA'},
      {name:'Sweden',total:3.8,yearly:[0.2,0.3,0.4,0.5,0.6,0.4,0.9,0.5,0.3],color:'#86EFAC'}
    ],
    sectors: {payments:32,lending:21,crypto_defi:18,wealthtech:12,insurtech:9,other:8}
  },
  sandboxes: [
    {country:'UK',flag:'🇬🇧',regulator:'FCA',year:2016,companies:890,cohorts:14,sectors:['payments','lending','crypto','insurance','wealthtech'],openness:9,success:['Monzo','Revolut','Wise','Starling Bank'],model:'sandbox',notes:"World's first, most replicated model"},
    {country:'Singapore',flag:'🇸🇬',regulator:'MAS',year:2016,companies:420,cohorts:8,sectors:['payments','insurance','crypto','lending'],openness:9,success:['Grab Financial','Nium','Funding Societies'],model:'sandbox',notes:'Fintech Festival hub, ASEAN gateway'},
    {country:'UAE',flag:'🇦🇪',regulator:'FSRA / DFSA',year:2017,companies:310,cohorts:6,sectors:['crypto','payments','wealthtech','regtech'],openness:8,success:['Sarwa','Mamo','Fuze'],model:'innovation_hub',notes:'Dual-zone model (ADGM + DIFC)'},
    {country:'India',flag:'🇮🇳',regulator:'RBI / SEBI / IRDAI',year:2019,companies:150,cohorts:3,sectors:['payments','lending','insurance'],openness:6,success:['Razorpay','PhonePe','BharatPe'],model:'sandbox',notes:'Multiple regulators, fragmented approach'},
    {country:'USA',flag:'🇺🇸',regulator:'OCC / CFPB',year:2018,companies:95,cohorts:null,sectors:['payments','lending','banking'],openness:5,success:['Chime','SoFi','Stripe'],model:'innovation_hub',notes:'Fragmented; state-by-state regulation'},
    {country:'Australia',flag:'🇦🇺',regulator:'ASIC',year:2017,companies:280,cohorts:5,sectors:['payments','lending','crypto','insurance'],openness:7,success:['Afterpay','Airwallex','Zip Co'],model:'sandbox',notes:'Enhanced sandbox with CDR open banking'}
  ],
  volatility: {
    currencies: ['USD','EUR','CNY','INR','BRL','TRY','JPY','GBP','ZAR','RUB'],
    years: [2015,2016,2017,2018,2019,2020,2021,2022,2023,2024],
    values: {
      USD:[12,14,10,15,11,22,13,16,11,10], EUR:[14,13,11,13,10,19,12,20,13,12],
      CNY:[8,12,7,11,14,10,8,15,10,9], INR:[13,14,10,17,12,16,11,18,14,13],
      BRL:[34,38,21,29,24,45,30,37,28,25], TRY:[22,31,19,56,28,35,63,75,89,70],
      JPY:[11,13,8,10,9,16,10,28,19,22], GBP:[14,35,12,15,20,18,11,18,14,12],
      ZAR:[25,28,18,22,19,38,24,27,22,20], RUB:[18,22,14,25,18,31,22,85,40,38]
    },
    events: {'GBP-2016':'Brexit referendum shock','TRY-2018':'Turkish currency & debt crisis','RUB-2022':'Russia-Ukraine war + sanctions','BRL-2020':'COVID + political uncertainty','TRY-2023':'Erdogan re-election, rate policy reversal'},
    g7: ['USD','EUR','JPY','GBP'], emerging: ['CNY','INR','BRL','TRY','ZAR','RUB']
  },
  payments: {
    systems: ['SWIFT/Wire','Real-Time Payments','CBDC','Crypto','Mobile Money','Card Networks'],
    colors: ['#4E9EF5','#00D4AA','#F5A623','#A78BFA','#34D399','#FF6B6B'],
    countries: ['USA','China','India','EU','Brazil','Nigeria','Kenya','Indonesia'],
    2024: {USA:[15,28,0,3,8,46],China:[5,10,8,2,67,8],India:[5,48,2,2,25,18],EU:[18,30,0,3,4,45],Brazil:[8,45,1,3,12,31],Nigeria:[10,12,4,3,58,13],Kenya:[3,8,0,2,78,9],Indonesia:[8,22,1,3,42,24]},
    2019: {USA:[22,15,0,1,5,57],China:[8,12,0,2,68,10],India:[10,22,0,1,18,49],EU:[25,20,0,1,3,51],Brazil:[20,15,0,1,10,54],Nigeria:[15,8,0,1,48,28],Kenya:[5,5,0,1,75,14],Indonesia:[12,10,0,1,35,42]}
  },
  policyMatrix: [
    {name:'Singapore',speed:9.2,openness:9.1,region:'asia_pacific',highlight:true,study:'Singapore\'s MAS leads with proactive licensing, sandbox programs, and cross-border CBDC experiments.'},
    {name:'UK',speed:7.8,openness:8.7,region:'europe',highlight:true,study:'The FCA sandbox model has been replicated globally. UK maintains its position as Europe\'s fintech capital.'},
    {name:'UAE',speed:8.5,openness:8.2,region:'middle_east',highlight:true,study:'UAE\'s dual-zone regulatory approach (ADGM + DIFC) attracts global fintech firms with tax-free innovation hubs.'},
    {name:'USA',speed:5.2,openness:7.8,region:'north_america',highlight:false,study:'Fragmented state-level regulation slows fintech innovation. Federal vs. state jurisdiction creates complexity.'},
    {name:'EU',speed:4.8,openness:6.5,region:'europe',highlight:false,study:'EU\'s comprehensive approach (MiCA, PSD2) is thorough but slow. Multi-state consensus delays implementation.'},
    {name:'Australia',speed:6.5,openness:7.2,region:'asia_pacific',highlight:false,study:'Australia\'s CDR framework enables open banking. ASIC sandbox balances innovation with consumer protection.'},
    {name:'China',speed:8.9,openness:3.5,region:'asia_pacific',highlight:true,study:'China moves fast on digital yuan and fintech regulation but maintains strict capital controls and crypto bans.'},
    {name:'India',speed:7.5,openness:5.8,region:'asia_pacific',highlight:false,study:'India\'s UPI success shows rapid digital payment adoption. Multiple regulators create fragmented sandbox approach.'},
    {name:'Russia',speed:7.2,openness:2.8,region:'europe',highlight:false,study:'Post-sanctions Russia accelerates domestic payment systems but faces severe international isolation.'},
    {name:'Brazil',speed:6.8,openness:5.5,region:'south_america',highlight:false,study:'Pix real-time payment system transformed payments. Central bank actively pilots Drex CBDC.'},
    {name:'S. Africa',speed:4.5,openness:5.2,region:'africa',highlight:false,study:'SARB explores CBDC via Project Khokha. Financial inclusion remains a key driver of fintech policy.'},
    {name:'Nigeria',speed:5.8,openness:4.5,region:'africa',highlight:false,study:'First African CBDC (eNaira) launched. Mobile money regulation evolving to boost financial inclusion.'},
    {name:'Japan',speed:4.2,openness:6.8,region:'asia_pacific',highlight:false,study:'Conservative approach to fintech innovation. BOJ CBDC research ongoing but implementation timeline unclear.'},
    {name:'Canada',speed:5.8,openness:7.5,region:'north_america',highlight:false,study:'Open banking framework in development. Collaborative approach between regulators and industry.'}
  ]
};

/* ═══════════════════════════════════════════════════════════
   CHART.JS DEFAULTS
   ═══════════════════════════════════════════════════════════ */
Chart.defaults.color = '#E8EDF2';
Chart.defaults.font.family = 'Inter, system-ui, sans-serif';
Chart.defaults.font.size = 12;
Chart.defaults.plugins.tooltip.backgroundColor = '#1A2332';
Chart.defaults.plugins.tooltip.titleColor = '#E8EDF2';
Chart.defaults.plugins.tooltip.bodyColor = '#7A8FA6';
Chart.defaults.plugins.tooltip.borderColor = '#2A3A4A';
Chart.defaults.plugins.tooltip.borderWidth = 1;
Chart.defaults.plugins.tooltip.padding = 12;
Chart.defaults.plugins.tooltip.cornerRadius = 8;
Chart.defaults.plugins.legend.labels.color = '#7A8FA6';

/* ═══════════════════════════════════════════════════════════
   SLIDE NAVIGATION
   ═══════════════════════════════════════════════════════════ */
let currentSlide = 1;
const totalSlides = 12;
const charts = {};
let slideInitialized = {};
let popupChartInstances = [];
let isTransitioning = false;

function changeSlide(dir) {
  if (isTransitioning) return;
  const next = currentSlide + dir;
  if (next < 1 || next > totalSlides) return;
  goToSlide(next);
}

function goToSlide(n) {
  if (n === currentSlide || isTransitioning) return;
  isTransitioning = true;
  const prev = document.getElementById('slide-' + currentSlide);
  const next = document.getElementById('slide-' + n);
  prev.classList.remove('active');
  if (n > currentSlide) {
    prev.classList.add('exit-left');
  } else {
    prev.style.transform = 'translateX(40px)';
    prev.style.opacity = '0';
  }
  setTimeout(() => {
    prev.classList.remove('exit-left');
    prev.style.transform = '';
    prev.style.opacity = '';
  }, 500);
  next.classList.add('active');
  currentSlide = n;
  document.getElementById('slide-counter').textContent = n + ' / ' + totalSlides;
  document.getElementById('progress-bar').style.width = (n / totalSlides * 100) + '%';
  setTimeout(() => { isTransitioning = false; }, 500);
  initSlide(n);
}

function toggleMenu() {
  const menu = document.getElementById('slide-menu');
  menu.classList.toggle('open');
  // Highlight current slide in menu
  menu.querySelectorAll('.slide-menu-item').forEach((item, i) => {
    item.classList.toggle('active-slide', i + 1 === currentSlide);
  });
}

// Touch swipe support
let touchStartX = 0;
let touchStartY = 0;
document.addEventListener('touchstart', e => {
  touchStartX = e.changedTouches[0].screenX;
  touchStartY = e.changedTouches[0].screenY;
}, { passive: true });
document.addEventListener('touchend', e => {
  const dx = e.changedTouches[0].screenX - touchStartX;
  const dy = e.changedTouches[0].screenY - touchStartY;
  if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > 60) {
    if (dx < 0) changeSlide(1);
    else changeSlide(-1);
  }
}, { passive: true });

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') { toggleMenu(); return; }
  if (document.getElementById('slide-menu').classList.contains('open')) return;
  if (e.key === 'ArrowRight' || e.key === 'ArrowDown' || e.key === ' ') { e.preventDefault(); changeSlide(1); }
  if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') { e.preventDefault(); changeSlide(-1); }
});

// Destroy popup charts to prevent memory leaks
function destroyPopupCharts() {
  popupChartInstances.forEach(c => { try { c.destroy(); } catch(e) {} });
  popupChartInstances = [];
}

/* ═══════════════════════════════════════════════════════════
   SLIDE INITIALIZERS
   ═══════════════════════════════════════════════════════════ */
function initSlide(n) {
  if (slideInitialized[n]) return;
  slideInitialized[n] = true;
  switch(n) {
    case 1: initSlide1(); break;
    case 2: initSlide2(); break;
    case 3: initSlide3(); break;
    case 4: initSlide4(); break;
    case 5: initSlide5(); break;
    case 6: initSlide6(); break;
    case 7: initSlide7(); break;
    case 8: initSlide8(); break;
    case 9: initSlide9(); break;
    case 10: initSlide10(); break;
    case 11: initSlide11(); break;
    case 12: initSlide12(); break;
  }
}

/* ─── SLIDE 1: Title ─── */
function initSlide1() {
  // Particles
  const pc = document.getElementById('particles');
  for (let i = 0; i < 30; i++) {
    const p = document.createElement('div');
    p.className = 'particle';
    p.style.left = Math.random() * 100 + '%';
    p.style.animationDelay = Math.random() * 8 + 's';
    p.style.animationDuration = (6 + Math.random() * 6) + 's';
    if (Math.random() > 0.5) p.style.background = '#F5A623';
    pc.appendChild(p);
  }
  // Counter animation
  document.querySelectorAll('#slide-1 .num').forEach(el => {
    const target = parseFloat(el.dataset.target);
    const prefix = el.dataset.prefix || '';
    const suffix = el.dataset.suffix || '';
    animateValue(el, 0, target, 2000, prefix, suffix, Number.isInteger(target) ? 0 : 0);
  });
}

function animateValue(el, start, end, duration, prefix, suffix, decimals) {
  const startTime = performance.now();
  function update(now) {
    const elapsed = now - startTime;
    const progress = Math.min(elapsed / duration, 1);
    const eased = 1 - Math.pow(1 - progress, 3);
    const current = start + (end - start) * eased;
    el.textContent = prefix + (decimals > 0 ? current.toFixed(decimals) : Math.round(current)) + suffix;
    if (progress < 1) requestAnimationFrame(update);
  }
  requestAnimationFrame(update);
}

/* ─── SLIDE 2: Market Cap ─── */
function initSlide2() {
  const regions = DATA.marketCap.regions;
  const colors = ['#00D4AA','#F5A623','#4E9EF5','#FF6B6B','#A78BFA'];
  const bgColors = ['rgba(0,212,170,0.2)','rgba(245,166,35,0.15)','rgba(78,158,245,0.15)','rgba(255,107,107,0.15)','rgba(167,139,250,0.15)'];
  const names = Object.keys(regions);

  charts.s2 = new Chart(document.getElementById('chart-slide2'), {
    type: 'line',
    data: {
      labels: DATA.marketCap.labels,
      datasets: names.map((name, i) => ({
        label: name, data: [...regions[name]],
        fill: true, backgroundColor: bgColors[i], borderColor: colors[i],
        borderWidth: 2, tension: 0.4, pointRadius: 3, pointHoverRadius: 6,
        pointBackgroundColor: colors[i]
      }))
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      interaction: { mode: 'index', intersect: false },
      plugins: {
        title: { display: false },
        tooltip: {
          callbacks: {
            label: ctx => ` ${ctx.dataset.label}: $${ctx.parsed.y.toFixed(1)}T`
          }
        }
      },
      scales: {
        x: { grid:{color:'#2A3A4A',lineWidth:0.5}, ticks:{color:'#7A8FA6'}, title:{display:true,text:'Year',color:'#7A8FA6'} },
        y: { grid:{color:'#2A3A4A',lineWidth:0.5}, ticks:{color:'#7A8FA6'}, title:{display:true,text:'Market Cap ($T)',color:'#7A8FA6'} }
      }
    }
  });

  // Toggle absolute vs %
  document.querySelectorAll('#s2-toggle .toggle-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('#s2-toggle .toggle-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      const mode = btn.dataset.mode;
      const chart = charts.s2;
      if (mode === 'percent') {
        DATA.marketCap.labels.forEach((_, i) => {
          const total = names.reduce((s, n) => s + regions[n][i], 0);
          names.forEach((n, j) => {
            chart.data.datasets[j].data[i] = (regions[n][i] / total * 100).toFixed(1);
          });
        });
        chart.options.scales.y.title.text = '% of Global Market';
        chart.options.plugins.tooltip.callbacks.label = ctx => ` ${ctx.dataset.label}: ${ctx.parsed.y}%`;
      } else {
        names.forEach((n, j) => { chart.data.datasets[j].data = [...regions[n]]; });
        chart.options.scales.y.title.text = 'Market Cap ($T)';
        chart.options.plugins.tooltip.callbacks.label = ctx => ` ${ctx.dataset.label}: $${ctx.parsed.y}T`;
      }
      chart.update('active');
    });
  });
}

/* ─── SLIDE 3: Bubble ─── */
function initSlide3() {
  const yearKeys = [2008,2015,2020,2023];
  const regionColors = {
    north_america: {bg:'rgba(0,212,170,0.65)',border:'#00D4AA'},
    europe: {bg:'rgba(245,166,35,0.65)',border:'#F5A623'},
    asia_pacific: {bg:'rgba(78,158,245,0.65)',border:'#4E9EF5'},
    emerging: {bg:'rgba(255,107,107,0.65)',border:'#FF6B6B'}
  };
  let activeFilter = 'all';

  function buildDatasets(year) {
    const data = DATA.bubbleYears[year];
    const filtered = activeFilter === 'all' ? data : data.filter(d => d.region === activeFilter);
    return [{
      data: filtered.map(d => ({
        x: Math.min(d.x, 20), y: Math.min(d.y, 60),
        r: Math.max(Math.sqrt(d.gdp) * 4, 5),
        country: d.country, actualRate: d.x, actualInflation: d.y, gdp: d.gdp, region: d.region
      })),
      backgroundColor: filtered.map(d => regionColors[d.region]?.bg || 'rgba(200,200,200,0.5)'),
      borderColor: filtered.map(d => regionColors[d.region]?.border || '#ccc'),
      borderWidth: 1.5
    }];
  }

  charts.s3 = new Chart(document.getElementById('chart-slide3'), {
    type: 'bubble',
    data: { datasets: buildDatasets(2023) },
    options: {
      responsive: true, maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: {
          callbacks: {
            label: ctx => {
              const d = ctx.raw;
              return [d.country, `Rate: ${d.actualRate}%`, `Inflation: ${d.actualInflation}%`, `GDP: $${d.gdp}T`];
            }
          }
        }
      },
      scales: {
        x: {min:0,max:20,grid:{color:'#2A3A4A'},ticks:{color:'#7A8FA6',callback:v=>v+'%'},title:{display:true,text:'Central Bank Policy Rate (%)',color:'#7A8FA6'}},
        y: {min:0,max:60,grid:{color:'#2A3A4A'},ticks:{color:'#7A8FA6',callback:v=>v+'%'},title:{display:true,text:'Inflation Rate (%)',color:'#7A8FA6'}}
      },
      onClick: (e, elements) => {
        if (elements.length) {
          const d = charts.s3.data.datasets[0].data[elements[0].index];
          showCountryPopup(d);
        }
      }
    }
  });

  document.getElementById('s3-slider').addEventListener('input', e => {
    const year = yearKeys[parseInt(e.target.value)];
    document.getElementById('s3-year').textContent = year;
    charts.s3.data.datasets = buildDatasets(year);
    charts.s3.update('active');
  });

  document.querySelectorAll('#s3-filters .filter-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('#s3-filters .filter-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      activeFilter = btn.dataset.region;
      const yearIdx = parseInt(document.getElementById('s3-slider').value);
      charts.s3.data.datasets = buildDatasets(yearKeys[yearIdx]);
      charts.s3.update('active');
    });
  });
}

function showCountryPopup(d) {
  const popup = document.getElementById('country-popup');
  document.getElementById('country-popup-content').innerHTML = `
    <h3 style="font-size:20px;color:var(--teal);margin-bottom:12px">${d.country}</h3>
    <div class="cbdc-detail-row"><span class="lbl">Policy Rate</span><span class="val">${d.actualRate}%</span></div>
    <div class="cbdc-detail-row"><span class="lbl">Inflation</span><span class="val">${d.actualInflation}%</span></div>
    <div class="cbdc-detail-row"><span class="lbl">GDP</span><span class="val">$${d.gdp}T</span></div>
  `;
  popup.classList.add('show');
}

/* ─── SLIDE 4: Timeline ─── */
function initSlide4() {
  const typeColors = {crisis:'#FF6B6B',technology:'#4E9EF5',regulation:'#F5A623',market:'#00D4AA'};
  const track = document.getElementById('timeline-track');
  track.innerHTML = '';

  DATA.timeline.forEach(ev => {
    const div = document.createElement('div');
    div.className = 'timeline-event';
    div.dataset.type = ev.type;
    div.innerHTML = `
      <div class="dot" style="background:${typeColors[ev.type]}"></div>
      <div class="year-label">${ev.year}</div>
      <div class="event-title">${ev.event}</div>
      <div class="detail-card">
        <div class="dc-type" style="color:${typeColors[ev.type]}">${ev.type}</div>
        <div>${ev.detail}</div>
      </div>
    `;
    track.appendChild(div);
  });

  // Horizontal scroll with mouse wheel
  const container = document.getElementById('timeline-container');
  container.addEventListener('wheel', e => {
    e.preventDefault();
    container.scrollLeft += e.deltaY;
  }, { passive: false });

  // Filters
  document.querySelectorAll('#s4-filters .filter-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('#s4-filters .filter-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      const type = btn.dataset.type;
      document.querySelectorAll('.timeline-event').forEach(ev => {
        ev.style.display = (type === 'all' || ev.dataset.type === type) ? '' : 'none';
      });
    });
  });
}

/* ─── SLIDE 5: CBDC Tracker ─── */
function initSlide5() {
  const statusColors = {4:'#34D399',3:'#00D4AA',2:'#F5A623'};
  const grid = document.getElementById('cbdc-grid');

  DATA.cbdc.forEach((c, i) => {
    const card = document.createElement('div');
    card.className = 'cbdc-dot-card';
    card.dataset.type = c.type;
    card.innerHTML = `
      <span class="status-dot" style="background:${statusColors[c.status]}"></span>
      <div class="country-name">${c.name}</div>
      <div class="status-label">${c.label}</div>
    `;
    card.addEventListener('click', () => {
      document.querySelectorAll('.cbdc-dot-card').forEach(c => c.classList.remove('active'));
      card.classList.add('active');
      showCBDCDetail(c);
    });
    grid.appendChild(card);
  });

  // Adoption bar chart
  const sorted = [...DATA.cbdc].sort((a,b) => b.adoption - a.adoption).filter(c => c.adoption > 0);
  charts.s5 = new Chart(document.getElementById('chart-slide5'), {
    type: 'bar',
    data: {
      labels: sorted.map(c => c.name),
      datasets: [{
        data: sorted.map(c => c.adoption),
        backgroundColor: sorted.map(c => statusColors[c.status]),
        borderRadius: 4
      }]
    },
    options: {
      indexAxis: 'y', responsive: true, maintainAspectRatio: false,
      plugins: { legend:{display:false}, tooltip:{callbacks:{label:ctx=>`${ctx.parsed.x}% adoption`}} },
      scales: {
        x: {grid:{color:'#2A3A4A'},ticks:{color:'#7A8FA6',callback:v=>v+'%'},title:{display:true,text:'Adoption Rate (%)',color:'#7A8FA6'}},
        y: {grid:{display:false},ticks:{color:'#E8EDF2',font:{size:11}}}
      }
    }
  });

  // Toggle retail vs wholesale
  document.querySelectorAll('#s5-toggle .toggle-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('#s5-toggle .toggle-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      const mode = btn.dataset.mode;
      document.querySelectorAll('.cbdc-dot-card').forEach((card, i) => {
        const c = DATA.cbdc[i];
        card.style.display = (mode === 'all' || c.type === mode) ? '' : 'none';
      });
    });
  });
}

function showCBDCDetail(c) {
  const panel = document.getElementById('cbdc-detail');
  panel.innerHTML = `
    <h3>${c.name}</h3>
    <div class="currency-name">${c.currency}</div>
    <div class="cbdc-detail-row"><span class="lbl">Status</span><span class="val" style="color:${c.status===4?'#34D399':c.status===3?'#00D4AA':'#F5A623'}">${c.label}</span></div>
    <div class="cbdc-detail-row"><span class="lbl">Launch Year</span><span class="val">${c.year||'TBD'}</span></div>
    <div class="cbdc-detail-row"><span class="lbl">Adoption Rate</span><span class="val">${c.adoption}%</span></div>
    <div class="cbdc-detail-row"><span class="lbl">Technology</span><span class="val">${c.tech}</span></div>
    <div class="cbdc-detail-row"><span class="lbl">Type</span><span class="val">${c.type.charAt(0).toUpperCase()+c.type.slice(1)} CBDC</span></div>
    <div style="margin-top:12px;font-size:12px;color:var(--text-muted);line-height:1.5">${c.notes}</div>
  `;
}

/* ─── SLIDE 6: Interest Rates ─── */
function initSlide6() {
  const g7Colors = ['#00D4AA','#4E9EF5','#34D399','#FB923C'];
  const bricsColors = ['#FF6B6B','#F5A623','#A78BFA','#F472B6','#86EFAC'];
  const g7Names = Object.keys(DATA.rates.g7);
  const bricsNames = Object.keys(DATA.rates.brics);

  const datasets = [
    ...g7Names.map((name, i) => ({
      label: name + ' (G7)', data: DATA.rates.g7[name],
      borderColor: g7Colors[i], borderWidth: 2.5, borderDash: [],
      tension: 0.3, pointRadius: 2, pointHoverRadius: 5, fill: false,
      pointBackgroundColor: g7Colors[i]
    })),
    ...bricsNames.map((name, i) => ({
      label: name + ' (BRICS)', data: DATA.rates.brics[name],
      borderColor: bricsColors[i], borderWidth: 2, borderDash: [6,4],
      tension: 0.3, pointRadius: 2, pointHoverRadius: 5, fill: false,
      pointBackgroundColor: bricsColors[i]
    }))
  ];

  // Recession band plugin
  const recessionPlugin = {
    id: 'recessionBands',
    beforeDraw(chart) {
      const {ctx, chartArea:{top,bottom}, scales:{x}} = chart;
      const bands = [[2020,2020],[2022,2022]];
      bands.forEach(([start,end]) => {
        const startIdx = DATA.rates.years.indexOf(start);
        const endIdx = DATA.rates.years.indexOf(end);
        if (startIdx < 0) return;
        const x1 = x.getPixelForValue(startIdx);
        const x2 = x.getPixelForValue(endIdx);
        ctx.fillStyle = 'rgba(255,107,107,0.08)';
        ctx.fillRect(x1 - 15, top, (x2 - x1) + 30, bottom - top);
      });
    }
  };

  charts.s6 = new Chart(document.getElementById('chart-slide6'), {
    type: 'line',
    data: { labels: DATA.rates.years, datasets },
    options: {
      responsive: true, maintainAspectRatio: false,
      interaction: { mode: 'index', intersect: false },
      plugins: {
        legend: { labels:{usePointStyle:true,pointStyle:'line',color:'#7A8FA6',font:{size:11},padding:12} },
        tooltip: {
          callbacks: {
            label: ctx => ` ${ctx.dataset.label}: ${ctx.parsed.y.toFixed(2)}%`
          }
        }
      },
      scales: {
        x: {grid:{color:'#2A3A4A',lineWidth:0.5},ticks:{color:'#7A8FA6'},title:{display:true,text:'Year',color:'#7A8FA6'}},
        y: {min:-1,max:18,grid:{color:'#2A3A4A',lineWidth:0.5},ticks:{color:'#7A8FA6',callback:v=>v+'%'},title:{display:true,text:'Policy Rate (%)',color:'#7A8FA6'}}
      }
    },
    plugins: [recessionPlugin]
  });
}

/* ─── SLIDE 7: Fintech Investment ─── */
let s7Cumulative = true;
function toggleS7Cumulative() {
  s7Cumulative = !s7Cumulative;
  document.getElementById('s7-cumulative').classList.toggle('active', s7Cumulative);
  updateS7Chart();
}

function initSlide7() {
  // Bubbles
  const bubbleViz = document.getElementById('bubble-viz');
  DATA.fintech.countries.forEach(c => {
    const size = Math.sqrt(c.total) * 12;
    const div = document.createElement('div');
    div.className = 'bubble-circle';
    div.style.width = size + 'px';
    div.style.height = size + 'px';
    div.style.background = c.color;
    div.title = `${c.name}: $${c.total}B`;
    div.innerHTML = `<span class="bubble-label">${c.name}</span>`;
    div.addEventListener('click', () => showSectorPie(c));
    bubbleViz.appendChild(div);
  });

  // Bar chart
  charts.s7 = new Chart(document.getElementById('chart-slide7'), {
    type: 'bar',
    data: {
      labels: DATA.fintech.countries.map(c => c.name),
      datasets: [{
        label: 'Fintech Investment ($B)',
        data: DATA.fintech.countries.map(c => c.total),
        backgroundColor: DATA.fintech.countries.map(c => c.color),
        borderRadius: 6
      }]
    },
    options: {
      indexAxis: 'y', responsive: true, maintainAspectRatio: false,
      plugins: { legend:{display:false}, tooltip:{callbacks:{label:ctx=>` $${ctx.parsed.x.toFixed(1)}B`}} },
      scales: {
        x: {grid:{color:'#2A3A4A'},ticks:{color:'#7A8FA6'},title:{display:true,text:'Investment ($B)',color:'#7A8FA6'}},
        y: {grid:{display:false},ticks:{color:'#E8EDF2',font:{size:11}}}
      }
    }
  });

  // Year slider
  document.getElementById('s7-slider').addEventListener('input', e => {
    const year = parseInt(e.target.value);
    document.getElementById('s7-year').textContent = s7Cumulative ? 'All Years' : year;
    s7Cumulative = false;
    document.getElementById('s7-cumulative').classList.remove('active');
    updateS7Chart(year);
  });
}

function updateS7Chart(year) {
  const chart = charts.s7;
  if (s7Cumulative) {
    chart.data.datasets[0].data = DATA.fintech.countries.map(c => c.total);
    chart.options.plugins.title = {display:false};
    document.getElementById('s7-year').textContent = 'All Years';
  } else {
    const idx = DATA.fintech.years.indexOf(year || 2023);
    chart.data.datasets[0].data = DATA.fintech.countries.map(c => c.yearly[idx] || 0);
  }
  chart.update('active');
}

function showSectorPie(country) {
  const popup = document.getElementById('country-popup');
  const sectors = DATA.fintech.sectors;
  const names = Object.keys(sectors);
  const sColors = ['#00D4AA','#F5A623','#4E9EF5','#A78BFA','#FF6B6B','#34D399'];
  document.getElementById('country-popup-content').innerHTML = `
    <h3 style="font-size:18px;color:var(--teal);margin-bottom:6px">${country.name}</h3>
    <p style="font-size:13px;color:var(--text-muted);margin-bottom:16px">Total: $${country.total}B — Sector Breakdown</p>
    <canvas id="sector-pie" height="200"></canvas>
  `;
  popup.classList.add('show');
  setTimeout(() => {
    destroyPopupCharts();
    const chart = new Chart(document.getElementById('sector-pie'), {
      type: 'doughnut',
      data: {
        labels: names.map(n => n.replace('_',' ').replace(/\b\w/g,l=>l.toUpperCase())),
        datasets: [{ data: Object.values(sectors), backgroundColor: sColors, borderWidth: 0 }]
      },
      options: {
        responsive: true,
        plugins: { legend:{position:'bottom',labels:{color:'#7A8FA6',font:{size:11},padding:8}} }
      }
    });
    popupChartInstances.push(chart);
  }, 100);
}

/* ─── SLIDE 8: Sandboxes ─── */
function initSlide8() {
  renderSandboxes(DATA.sandboxes);

  document.querySelectorAll('#s8-filters .filter-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('#s8-filters .filter-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      let sorted = [...DATA.sandboxes];
      const s = btn.dataset.sort;
      if (s === 'open') sorted.sort((a,b) => b.openness - a.openness);
      else if (s === 'largest') sorted.sort((a,b) => b.companies - a.companies);
      else if (s === 'newest') sorted.sort((a,b) => b.year - a.year);
      else if (s === 'sandbox') sorted = sorted.filter(x => x.model === 'sandbox');
      else if (s === 'hub') sorted = sorted.filter(x => x.model === 'innovation_hub');
      renderSandboxes(sorted);
    });
  });
}

function renderSandboxes(list) {
  const grid = document.getElementById('sandbox-grid');
  grid.innerHTML = '';
  list.forEach(sb => {
    const card = document.createElement('div');
    card.className = 'sandbox-card';
    card.innerHTML = `
      <div class="flag">${sb.flag}</div>
      <div class="country">${sb.country}</div>
      <div class="regulator">${sb.regulator}</div>
      <div class="stat-row"><span class="k">Launched</span><span class="v">${sb.year}</span></div>
      <div class="stat-row"><span class="k">Companies</span><span class="v">${sb.companies}</span></div>
      <div class="stat-row"><span class="k">Cohorts</span><span class="v">${sb.cohorts || 'N/A'}</span></div>
      <div class="stat-row"><span class="k">Openness</span><span class="v">${sb.openness}/10</span></div>
      <div class="openness-bar"><div class="openness-fill" style="width:${sb.openness*10}%"></div></div>
      <div class="sector-tags">${sb.sectors.map(s => `<span class="sector-tag">${s}</span>`).join('')}</div>
      <span class="model-badge ${sb.model}">${sb.model === 'sandbox' ? '🧪 Sandbox' : '💡 Innovation Hub'}</span>
      <div class="success-companies"><strong>Success Stories:</strong> ${sb.success.join(', ')}</div>
    `;
    grid.appendChild(card);
  });
}

/* ─── SLIDE 9: Currency Volatility ─── */
function initSlide9() {
  renderHeatmap('all');

  document.querySelectorAll('#s9-filters .filter-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('#s9-filters .filter-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      renderHeatmap(btn.dataset.group);
    });
  });
}

function renderHeatmap(group) {
  const grid = document.getElementById('heatmap-grid');
  grid.innerHTML = '';
  let currencies = DATA.volatility.currencies;
  if (group === 'g7') currencies = DATA.volatility.g7;
  else if (group === 'emerging') currencies = DATA.volatility.emerging;

  // Update grid columns
  grid.style.gridTemplateColumns = `60px repeat(${DATA.volatility.years.length}, 1fr)`;

  // Header row
  grid.innerHTML += '<div class="heatmap-header"></div>';
  DATA.volatility.years.forEach(y => { grid.innerHTML += `<div class="heatmap-header">${y}</div>`; });

  // Data rows
  currencies.forEach(cur => {
    grid.innerHTML += `<div class="heatmap-row-label">${cur}</div>`;
    DATA.volatility.values[cur].forEach((val, yi) => {
      const color = getVolColor(val);
      const eventKey = cur + '-' + DATA.volatility.years[yi];
      const event = DATA.volatility.events[eventKey] || '';
      grid.innerHTML += `
        <div class="heatmap-cell" style="background:${color};color:${val>50?'#fff':'#E8EDF2'}"
             onclick="showVolChart('${cur}',${DATA.volatility.years[yi]},${val})">
          ${val}
          <div class="ht-tooltip">
            <strong>${cur}</strong> ${DATA.volatility.years[yi]}: Volatility ${val}
            ${event ? '<br><em style="color:#F5A623">'+event+'</em>' : ''}
          </div>
        </div>`;
    });
  });
}

function getVolColor(val) {
  if (val <= 15) return '#0d5e46';
  if (val <= 25) return '#1a7a5a';
  if (val <= 35) return '#b8860b';
  if (val <= 50) return '#d4760a';
  if (val <= 70) return '#c0392b';
  return '#a31621';
}

function showVolChart(currency, year, val) {
  const popup = document.getElementById('country-popup');
  // Generate synthetic monthly data
  const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const monthlyData = months.map(() => {
    return (val + (Math.random() - 0.5) * val * 0.6).toFixed(1);
  });

  document.getElementById('country-popup-content').innerHTML = `
    <h3 style="font-size:18px;color:var(--teal)">${currency} — ${year}</h3>
    <p style="font-size:12px;color:var(--text-muted);margin-bottom:12px">Monthly volatility (synthetic approximation)</p>
    <canvas id="vol-line" height="180"></canvas>
  `;
  popup.classList.add('show');
  setTimeout(() => {
    destroyPopupCharts();
    const chart = new Chart(document.getElementById('vol-line'), {
      type: 'line',
      data: {
        labels: months,
        datasets: [{
          data: monthlyData, borderColor: '#00D4AA', backgroundColor: 'rgba(0,212,170,0.1)',
          fill: true, tension: 0.4, borderWidth: 2, pointRadius: 3, pointBackgroundColor: '#00D4AA'
        }]
      },
      options: {
        responsive: true, plugins:{legend:{display:false}},
        scales: {
          x:{grid:{color:'#2A3A4A'},ticks:{color:'#7A8FA6',font:{size:10}}},
          y:{grid:{color:'#2A3A4A'},ticks:{color:'#7A8FA6'}}
        }
      }
    });
    popupChartInstances.push(chart);
  }, 100);
}

/* ─── SLIDE 10: Payments ─── */
function initSlide10() {
  charts.s10 = new Chart(document.getElementById('chart-slide10'), {
    type: 'bar',
    data: buildPaymentsData('2024'),
    options: {
      indexAxis: 'y', responsive: true, maintainAspectRatio: false,
      plugins: {
        legend: { labels:{color:'#7A8FA6',font:{size:11},padding:10} },
        tooltip: { callbacks:{label:ctx=>` ${ctx.dataset.label}: ${ctx.parsed.x}%`} }
      },
      scales: {
        x: {stacked:true,max:100,grid:{color:'#2A3A4A'},ticks:{color:'#7A8FA6',callback:v=>v+'%'},title:{display:true,text:'% of Transactions',color:'#7A8FA6'}},
        y: {stacked:true,grid:{display:false},ticks:{color:'#E8EDF2',font:{size:12}}}
      },
      onClick: (e, elements) => {
        if (elements.length) {
          const dsIdx = elements[0].datasetIndex;
          const system = DATA.payments.systems[dsIdx];
          const descs = {
            'SWIFT/Wire':'Traditional interbank wire transfers via SWIFT network — secure but slow (1-3 days) and expensive.',
            'Real-Time Payments':'Instant payment systems like UPI (India), Pix (Brazil), FedNow (USA) — settling in seconds.',
            'CBDC':'Central Bank Digital Currencies — government-issued digital money for direct transactions.',
            'Crypto':'Cryptocurrency payments including Bitcoin, stablecoins, and DeFi protocols.',
            'Mobile Money':'Mobile wallet platforms like M-Pesa, Alipay, WeChat Pay — dominant in emerging markets.',
            'Card Networks':'Traditional card rails (Visa, Mastercard, Amex) — still dominant in Western economies.'
          };
          const popup = document.getElementById('country-popup');
          document.getElementById('country-popup-content').innerHTML = `
            <h3 style="color:var(--teal);font-size:18px">${system}</h3>
            <p style="font-size:13px;color:var(--text-muted);margin-top:8px;line-height:1.6">${descs[system]}</p>
          `;
          popup.classList.add('show');
        }
      }
    }
  });

  document.querySelectorAll('#s10-toggle .toggle-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('#s10-toggle .toggle-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      charts.s10.data = buildPaymentsData(btn.dataset.year);
      charts.s10.update('active');
    });
  });
}

function buildPaymentsData(year) {
  const d = DATA.payments[year];
  return {
    labels: DATA.payments.countries,
    datasets: DATA.payments.systems.map((sys, i) => ({
      label: sys,
      data: DATA.payments.countries.map(c => d[c][i]),
      backgroundColor: DATA.payments.colors[i],
      borderWidth: 0
    }))
  };
}

/* ─── SLIDE 11: Policy Matrix ─── */
function initSlide11() {
  const regionColors = {
    asia_pacific:'#4E9EF5',europe:'#F5A623',middle_east:'#00D4AA',
    north_america:'#FF6B6B',south_america:'#A78BFA',africa:'#34D399'
  };

  // Quadrant plugin
  const quadrantPlugin = {
    id: 'quadrantColors',
    beforeDraw(chart) {
      const {ctx,chartArea:{left,right,top,bottom},scales:{x,y}} = chart;
      const midX = x.getPixelForValue(5);
      const midY = y.getPixelForValue(5);
      const quads = [
        {x1:midX,y1:top,x2:right,y2:midY,color:'rgba(0,212,170,0.04)',label:'Fast & Open'},
        {x1:left,y1:top,x2:midX,y2:midY,color:'rgba(78,158,245,0.04)',label:'Slow & Open'},
        {x1:midX,y1:midY,x2:right,y2:bottom,color:'rgba(255,107,107,0.04)',label:'Fast & Restrictive'},
        {x1:left,y1:midY,x2:midX,y2:bottom,color:'rgba(167,139,250,0.04)',label:'Slow & Restrictive'}
      ];
      quads.forEach(q => {
        ctx.fillStyle = q.color;
        ctx.fillRect(q.x1,q.y1,q.x2-q.x1,q.y2-q.y1);
        ctx.fillStyle = 'rgba(122,143,166,0.3)';
        ctx.font = '11px Inter';
        ctx.textAlign = 'center';
        ctx.fillText(q.label, (q.x1+q.x2)/2, q.y1 + 20);
      });
      // Crosshairs
      ctx.strokeStyle = 'rgba(42,58,74,0.8)';
      ctx.lineWidth = 1; ctx.setLineDash([4,4]);
      ctx.beginPath(); ctx.moveTo(midX,top); ctx.lineTo(midX,bottom); ctx.stroke();
      ctx.beginPath(); ctx.moveTo(left,midY); ctx.lineTo(right,midY); ctx.stroke();
      ctx.setLineDash([]);
    }
  };

  charts.s11 = new Chart(document.getElementById('chart-slide11'), {
    type: 'scatter',
    data: {
      datasets: [{
        data: DATA.policyMatrix.map(c => ({
          x: c.speed, y: c.openness,
          name: c.name, highlight: c.highlight, study: c.study, region: c.region
        })),
        backgroundColor: DATA.policyMatrix.map(c => regionColors[c.region] || '#7A8FA6'),
        borderColor: DATA.policyMatrix.map(c => c.highlight ? '#fff' : 'transparent'),
        borderWidth: DATA.policyMatrix.map(c => c.highlight ? 2 : 0),
        pointRadius: DATA.policyMatrix.map(c => c.highlight ? 10 : 7),
        pointHoverRadius: 13
      }]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      plugins: {
        legend: {display:false},
        tooltip: {
          callbacks: {
            label: ctx => {
              const d = ctx.raw;
              return [`${d.name}`, `Speed: ${d.x}`, `Openness: ${d.y}`];
            }
          }
        }
      },
      scales: {
        x: {min:3,max:10,grid:{color:'#2A3A4A'},ticks:{color:'#7A8FA6'},title:{display:true,text:'Speed of Policy Response →',color:'#7A8FA6'}},
        y: {min:2,max:10,grid:{color:'#2A3A4A'},ticks:{color:'#7A8FA6'},title:{display:true,text:'Market Openness →',color:'#7A8FA6'}}
      },
      onClick: (e, elements) => {
        if (elements.length) {
          const d = charts.s11.data.datasets[0].data[elements[0].index];
          const popup = document.getElementById('country-popup');
          document.getElementById('country-popup-content').innerHTML = `
            <h3 style="color:var(--teal);font-size:18px">${d.name}</h3>
            <div class="cbdc-detail-row"><span class="lbl">Policy Speed</span><span class="val">${d.x}/10</span></div>
            <div class="cbdc-detail-row"><span class="lbl">Market Openness</span><span class="val">${d.y}/10</span></div>
            <p style="font-size:13px;color:var(--text-muted);margin-top:12px;line-height:1.6">${d.study}</p>
          `;
          popup.classList.add('show');
        }
      }
    },
    plugins: [quadrantPlugin]
  });

  // Add country name labels
  const originalDraw = charts.s11.draw.bind(charts.s11);
  const labelPlugin = {
    id: 'countryLabels',
    afterDraw(chart) {
      const {ctx} = chart;
      const meta = chart.getDatasetMeta(0);
      meta.data.forEach((point, i) => {
        const d = chart.data.datasets[0].data[i];
        ctx.fillStyle = '#E8EDF2';
        ctx.font = '10px Inter';
        ctx.textAlign = 'center';
        ctx.fillText(d.name, point.x, point.y - 14);
      });
    }
  };
  charts.s11.config.plugins.push(labelPlugin);
  charts.s11.update();
}

/* ─── SLIDE 12: Conclusion ─── */
function initSlide12() {
  document.querySelectorAll('#stat-cards .stat-num').forEach(el => {
    const target = parseFloat(el.dataset.target);
    const suffix = el.dataset.suffix || '';
    const decimals = parseInt(el.dataset.decimals) || 0;
    animateValue(el, 0, target, 2200, '', suffix, decimals);
  });
}

/* ═══════════════════════════════════════════════════════════
   BOOT
   ═══════════════════════════════════════════════════════════ */
document.addEventListener('DOMContentLoaded', () => {
  initSlide(1);
  
  // Close popup on overlay click
  document.getElementById('country-popup').addEventListener('click', e => {
    if (e.target.id === 'country-popup') {
      e.target.classList.remove('show');
      destroyPopupCharts();
    }
  });
  document.getElementById('sources-modal').addEventListener('click', e => {
    if (e.target.id === 'sources-modal') e.target.classList.remove('show');
  });
});
</script>
</body>
</html>
