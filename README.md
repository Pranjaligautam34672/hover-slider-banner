{% comment %}
  Everambr Hero Banner Section — Fully Dynamic
  - Hover zone-based slide switching
  - Custom DRAG cursor circle on hover
  - Simple circle on timeline handle (no text)
  - Every text element: color + font-size + font-weight from Shopify Admin
{% endcomment %}

<style>
  @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@300;400;500&display=swap');

  .evr-hero * { margin: 0; padding: 0; box-sizing: border-box; }

  .evr-hero {
    position: relative;
    width: 100%;
    height: 100vh;
    min-height: 500px;
    overflow: hidden;
    font-family: 'DM Sans', sans-serif;
    background: #000;
    user-select: none;
    cursor: none;
  }

  /* ---- SLIDES ---- */
  .evr-slide {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    opacity: 0;
    transition: opacity 0.55s ease;
    pointer-events: none;
  }
  .evr-slide.evr-active { opacity: 1; }

  .evr-slide-bg {
    position: absolute;
    inset: 0;
    background-size: cover;
    background-position: center;
  }
  .evr-slide-overlay { position: absolute; inset: 0; }

  .evr-slide--train .evr-slide-bg {
    background: linear-gradient(105deg, #1a1200 0%, #c8800a 40%, #f0c855 75%, #f8e090 100%);
  }
  .evr-slide--train .evr-slide-overlay {
    background: linear-gradient(90deg, rgba(0,0,0,0.45) 0%, rgba(0,0,0,0) 60%);
  }
  .evr-slide--work .evr-slide-bg {
    background: linear-gradient(105deg, #1a1a1a 0%, #888 45%, #e8e8e8 100%);
  }
  .evr-slide--work .evr-slide-overlay {
    background: linear-gradient(90deg, rgba(200,200,200,0.5) 0%, rgba(200,200,200,0) 60%);
  }
  .evr-slide--sleep .evr-slide-bg {
    background: linear-gradient(105deg, #060202 0%, #4a1010 40%, #8a2020 70%, #1a1a1a 100%);
  }
  .evr-slide--sleep .evr-slide-overlay {
    background: linear-gradient(270deg, rgba(0,0,0,0.4) 0%, rgba(0,0,0,0.05) 60%);
  }

  /* ---- CUSTOM CURSOR CIRCLE ---- */
  #evr-cursor {
    position: absolute;
    width: 52px;
    height: 52px;
    border-radius: 50%;
    border: 1.5px solid rgba(255,255,255,0.5);
    background: rgba(255,255,255,0.08);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 8px;
    letter-spacing: 0.1em;
    color: rgba(255,255,255,0.75);
    font-family: 'DM Sans', sans-serif;
    pointer-events: none;
    z-index: 100;
    transform: translate(-50%, -50%);
    opacity: 0;
    transition: opacity 0.25s ease;
    text-transform: uppercase;
  }

  /* ---- CONTENT ---- */
  .evr-content {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    z-index: 10;
    width: 90%;
    max-width: 680px;
    pointer-events: none;
  }

  /* Eyebrow */
  .evr-eyebrow {
    letter-spacing: 0.25em;
    margin-bottom: 18px;
    text-transform: uppercase;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
    font-size: {{ section.settings.eyebrow_size }}px;
    color: {{ section.settings.eyebrow_color }};
    font-weight: {{ section.settings.eyebrow_weight }};
  }
  .evr-eyebrow::before,
  .evr-eyebrow::after {
    content: '';
    display: block;
    width: 32px;
    height: 1px;
    background: rgba(255,255,255,0.45);
  }

  /* Headline */
  .evr-headline {
    font-family: 'Bebas Neue', sans-serif;
    line-height: 0.92;
    white-space: nowrap;
    letter-spacing: 0.025em;
    margin-bottom: 16px;
    font-size: clamp({{ section.settings.headline_size_min }}px, 9vw, {{ section.settings.headline_size_max }}px);
  }

  /* Highlight word */
  .evr-hl {
    color: {{ section.settings.headline_highlight_color }};
    font-weight: {{ section.settings.headline_weight }};
  }

  /* Suffix word */
  .evr-hr {
    color: {{ section.settings.headline_suffix_color }};
    font-weight: {{ section.settings.headline_weight }};
  }

  /* Subtext */
  .evr-subtext {
    letter-spacing: 0.06em;
    margin-bottom: 28px;
    font-size: {{ section.settings.subtext_size }}px;
    color: {{ section.settings.subtext_color }};
    font-weight: {{ section.settings.subtext_weight }};
  }

  /* ---- TABS ---- */
  .evr-tabs {
    display: inline-flex;
    background: rgba(255,255,255,0.1);
    backdrop-filter: blur(14px);
    -webkit-backdrop-filter: blur(14px);
    border-radius: 50px;
    border: 1px solid rgba(255,255,255,0.18);
    overflow: hidden;
    pointer-events: all;
  }

  .evr-tab {
    padding: 9px 22px;
    font-family: 'DM Sans', sans-serif;
    cursor: pointer;
    transition: all 0.3s ease;
    border-radius: 50px;
    display: flex;
    align-items: center;
    gap: 7px;
    white-space: nowrap;
    font-size: {{ section.settings.tab_size }}px;
    color: {{ section.settings.tab_color }};
    font-weight: {{ section.settings.tab_weight }};
  }

  .evr-tab .evr-dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: rgba(255,255,255,0.35);
    transition: background 0.3s;
    flex-shrink: 0;
  }

  .evr-tab.evr-tab-active {
    background: {{ section.settings.tab_active_bg }};
    color: {{ section.settings.tab_active_color }};
  }
  .evr-tab.evr-tab-active .evr-dot { background: {{ section.settings.tab_active_color }}; }

  /* ---- NAV DOTS ---- */
  .evr-nav-dots {
    position: absolute;
    right: 24px;
    top: 50%;
    transform: translateY(-50%);
    z-index: 20;
    display: flex;
    flex-direction: column;
    gap: 10px;
    pointer-events: all;
  }

  .evr-nav-dot {
    width: 4px;
    height: 22px;
    border-radius: 2px;
    background: rgba(255,255,255,0.22);
    cursor: pointer;
    transition: all 0.35s ease;
  }
  .evr-nav-dot.evr-dot-active {
    background: {{ section.settings.accent_color }};
    height: 38px;
  }

  /* ---- TIMELINE ---- */
  .evr-timeline {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 20;
    padding-bottom: 18px;
    pointer-events: none;
  }

  /* Tagline */
  .evr-tl-tagline {
    text-align: center;
    letter-spacing: 0.2em;
    margin-bottom: 10px;
    text-transform: uppercase;
    font-size: {{ section.settings.tagline_size }}px;
    color: {{ section.settings.tagline_color }};
    font-weight: {{ section.settings.tagline_weight }};
  }
  .evr-tl-tagline span { color: {{ section.settings.accent_color }}; opacity: 0.6; margin: 0 4px; }

  .evr-tl-track {
    position: relative;
    height: 2px;
    margin: 0 24px;
    background: rgba(255,255,255,0.15);
    border-radius: 1px;
    pointer-events: all;
    cursor: pointer;
  }

  .evr-tl-fill {
    height: 100%;
    background: {{ section.settings.accent_color }};
    border-radius: 1px;
    width: 0%;
  }

  /* ---- TIMELINE HANDLE — simple circle, no text ---- */
  .evr-tl-handle {
    position: absolute;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 18px;
    height: 18px;
    border-radius: 50%;
    background: {{ section.settings.accent_color }};
    border: 2px solid rgba(255,255,255,0.6);
    cursor: grab;
    left: 0%;
    pointer-events: all;
    z-index: 5;
    transition: transform 0.15s ease;
  }
  .evr-tl-handle:hover { transform: translate(-50%, -50%) scale(1.3); }
  .evr-tl-handle:active { cursor: grabbing; transform: translate(-50%, -50%) scale(1.15); }

  /* Timeline markers */
  .evr-tl-markers {
    display: flex;
    justify-content: space-between;
    padding: 8px 24px 0;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    pointer-events: all;
    font-size: {{ section.settings.marker_size }}px;
    color: {{ section.settings.marker_color }};
    font-weight: {{ section.settings.marker_weight }};
  }

  .evr-tm { cursor: pointer; transition: color 0.3s; }
  .evr-tm.evr-tm-active { color: {{ section.settings.accent_color }}; }

  @media (max-width: 640px) {
    .evr-hero { cursor: default; }
    #evr-cursor { display: none; }
    .evr-headline { font-size: clamp(42px, 14vw, 80px); white-space: normal; }
    .evr-tab { padding: 8px 14px; }
    .evr-nav-dots { right: 12px; }
  }
</style>

<div class="evr-hero" id="evr-hero">

  <!-- Custom cursor circle (follows mouse) -->
  <div id="evr-cursor">DRAG</div>

  <!-- Slide 1 — Train -->
  <div class="evr-slide evr-slide--train evr-active" data-index="0">
    <div class="evr-slide-bg">
      {%- if section.settings.slide1_image != blank -%}
        <img src="{{ section.settings.slide1_image | img_url: 'master' }}"
             alt="{{ section.settings.slide1_image.alt | escape }}"
             style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;">
      {%- endif -%}
    </div>
    <div class="evr-slide-overlay"></div>
  </div>

  <!-- Slide 2 — Work -->
  <div class="evr-slide evr-slide--work" data-index="1">
    <div class="evr-slide-bg">
      {%- if section.settings.slide2_image != blank -%}
        <img src="{{ section.settings.slide2_image | img_url: 'master' }}"
             alt="{{ section.settings.slide2_image.alt | escape }}"
             style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;">
      {%- endif -%}
    </div>
    <div class="evr-slide-overlay"></div>
  </div>

  <!-- Slide 3 — Sleep -->
  <div class="evr-slide evr-slide--sleep" data-index="2">
    <div class="evr-slide-bg">
      {%- if section.settings.slide3_image != blank -%}
        <img src="{{ section.settings.slide3_image | img_url: 'master' }}"
             alt="{{ section.settings.slide3_image.alt | escape }}"
             style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;">
      {%- endif -%}
    </div>
    <div class="evr-slide-overlay"></div>
  </div>

  <!-- Centre Content -->
  <div class="evr-content">
    <p class="evr-eyebrow">{{ section.settings.eyebrow | default: 'Functional Eyewear · Designed in Ireland' }}</p>
    <div class="evr-headline">
      <span class="evr-hl" id="evr-hl">{{ section.settings.slide1_word | default: 'train' }}</span>
      <span class="evr-hr" id="evr-hr">{{ section.settings.slide1_suffix | default: 'harder.' }}</span>
    </div>
    <p class="evr-subtext" id="evr-sub">{{ section.settings.slide1_subtext | default: 'Performance sunglasses engineered for sun and speed.' }}</p>
    <div class="evr-tabs">
      <div class="evr-tab evr-tab-active" data-goto="0">
        <span class="evr-dot"></span> {{ section.settings.slide1_tab | default: 'train' }}
      </div>
      <div class="evr-tab" data-goto="1">
        <span class="evr-dot"></span> {{ section.settings.slide2_tab | default: 'work' }}
      </div>
      <div class="evr-tab" data-goto="2">
        <span class="evr-dot"></span> {{ section.settings.slide3_tab | default: 'sleep' }}
      </div>
    </div>
  </div>

  <!-- Right nav dots -->
  <div class="evr-nav-dots">
    <div class="evr-nav-dot evr-dot-active" data-goto="0"></div>
    <div class="evr-nav-dot" data-goto="1"></div>
    <div class="evr-nav-dot" data-goto="2"></div>
  </div>

  <!-- Bottom Timeline -->
  <div class="evr-timeline">
    <p class="evr-tl-tagline">
      <span>{{ section.settings.tagline_word1 | default: 'AMBITION' }}</span> ·
      <span>{{ section.settings.tagline_word2 | default: 'MOVEMENT' }}</span> ·
      <span>{{ section.settings.tagline_word3 | default: 'BALANCE' }}</span> ·
      <span>{{ section.settings.tagline_word4 | default: 'RECOVERY' }}</span>
    </p>
    <div class="evr-tl-track" id="evr-tl-track">
      <div class="evr-tl-fill" id="evr-tl-fill"></div>
      <!-- Simple circle handle — no text -->
      <div class="evr-tl-handle" id="evr-tl-handle"></div>
    </div>
    <div class="evr-tl-markers">
      <span class="evr-tm evr-tm-active" data-goto="0">{{ section.settings.marker1 | default: '06:00 — TRAIN' }}</span>
      <span class="evr-tm" data-goto="1">{{ section.settings.marker2 | default: '12:00 — WORK' }}</span>
      <span class="evr-tm" data-goto="2">{{ section.settings.marker3 | default: '22:00 — SLEEP' }}</span>
    </div>
  </div>

</div>

<script>
(function () {

  var DATA = [
    {
      hl:  '{{ section.settings.slide1_word   | default: "train"  }}',
      hr:  '{{ section.settings.slide1_suffix | default: "harder." }}',
      sub: '{{ section.settings.slide1_subtext | default: "Performance sunglasses engineered for sun and speed." }}'
    },
    {
      hl:  '{{ section.settings.slide2_word   | default: "work"   }}',
      hr:  '{{ section.settings.slide2_suffix | default: "deeper." }}',
      sub: '{{ section.settings.slide2_subtext | default: "Blue light glasses for screens and harsh office light." }}'
    },
    {
      hl:  '{{ section.settings.slide3_word   | default: "sleep"  }}',
      hr:  '{{ section.settings.slide3_suffix | default: "better." }}',
      sub: '{{ section.settings.slide3_subtext | default: "Strong evening light-blockers for your most restful sleep." }}'
    }
  ];

  var cur     = 0;
  var autoTmr = null;

  var hero      = document.getElementById('evr-hero');
  var slides    = document.querySelectorAll('.evr-slide');
  var tabs      = document.querySelectorAll('.evr-tab');
  var navDots   = document.querySelectorAll('.evr-nav-dot');
  var tlMarkers = document.querySelectorAll('.evr-tm');
  var tlFill    = document.getElementById('evr-tl-fill');
  var tlHandle  = document.getElementById('evr-tl-handle');
  var tlTrack   = document.getElementById('evr-tl-track');
  var hlEl      = document.getElementById('evr-hl');
  var hrEl      = document.getElementById('evr-hr');
  var subEl     = document.getElementById('evr-sub');
  var cursorEl  = document.getElementById('evr-cursor');

  function setFill(pct) {
    tlFill.style.width  = pct + '%';
    tlHandle.style.left = pct + '%';
  }

  function goTo(n, snapFill) {
    if (n === cur && !snapFill) return;
    var changed = n !== cur;
    if (changed) {
      slides[cur].classList.remove('evr-active');
      tabs[cur].classList.remove('evr-tab-active');
      navDots[cur].classList.remove('evr-dot-active');
      tlMarkers[cur].classList.remove('evr-tm-active');
      cur = n;
      slides[cur].classList.add('evr-active');
      tabs[cur].classList.add('evr-tab-active');
      navDots[cur].classList.add('evr-dot-active');
      tlMarkers[cur].classList.add('evr-tm-active');
      hlEl.textContent  = DATA[cur].hl;
      hrEl.textContent  = DATA[cur].hr;
      subEl.textContent = DATA[cur].sub;
    }
    if (snapFill) {
      tlFill.style.transition   = 'width 0.4s ease';
      tlHandle.style.transition = 'left 0.4s ease';
      var snap = cur === 0 ? 0 : cur === 1 ? 50 : 100;
      setFill(snap);
      setTimeout(function () {
        tlFill.style.transition   = '';
        tlHandle.style.transition = '';
      }, 420);
    }
  }

  function zoneOf(clientX) {
    var r = hero.getBoundingClientRect();
    var p = (clientX - r.left) / r.width;
    return p < 0.333 ? 0 : p < 0.666 ? 1 : 2;
  }

  function pctOf(clientX) {
    var r = hero.getBoundingClientRect();
    return Math.max(0, Math.min(100, (clientX - r.left) / r.width * 100));
  }

  function startAuto() {
    autoTmr = setInterval(function () { goTo((cur + 1) % 3, true); }, 5000);
  }
  function stopAuto() { clearInterval(autoTmr); autoTmr = null; }

  startAuto();

  function isUI(el) {
    return el.closest('.evr-tab') || el.closest('.evr-nav-dot') ||
           el.closest('.evr-tl-markers') || el.closest('#evr-tl-track');
  }

  /* ---- Hover: cursor circle + slide switch ---- */
  var hovering = false;

  hero.addEventListener('mouseenter', function () {
    hovering = true;
    stopAuto();
    cursorEl.style.opacity = '1';
  });

  hero.addEventListener('mouseleave', function () {
    hovering = false;
    cursorEl.style.opacity = '0';
    goTo(cur, true);
    startAuto();
  });

  hero.addEventListener('mousemove', function (e) {
    if (!hovering) return;
    var r = hero.getBoundingClientRect();
    cursorEl.style.left = (e.clientX - r.left) + 'px';
    cursorEl.style.top  = (e.clientY - r.top)  + 'px';
    if (isUI(e.target)) return;
    setFill(pctOf(e.clientX));
    var z = zoneOf(e.clientX);
    if (z !== cur) goTo(z, false);
  });

  /* ---- Tabs / nav dots / markers ---- */
  tabs.forEach(function (t) {
    t.addEventListener('click', function (e) {
      e.stopPropagation(); stopAuto(); goTo(+t.dataset.goto, true);
    });
  });
  navDots.forEach(function (d) {
    d.addEventListener('click', function (e) {
      e.stopPropagation(); stopAuto(); goTo(+d.dataset.goto, true);
    });
  });
  tlMarkers.forEach(function (m) {
    m.addEventListener('click', function (e) {
      e.stopPropagation(); stopAuto(); goTo(+m.dataset.goto, true);
    });
  });

  /* ---- Touch swipe ---- */
  var tDown = false;

  hero.addEventListener('touchstart', function (e) {
    if (isUI(e.target)) return;
    tDown = true; stopAuto();
    setFill(pctOf(e.touches[0].clientX));
    goTo(zoneOf(e.touches[0].clientX), false);
  }, { passive: true });

  hero.addEventListener('touchmove', function (e) {
    if (!tDown) return;
    var cx = e.touches[0].clientX;
    setFill(pctOf(cx));
    var z = zoneOf(cx);
    if (z !== cur) goTo(z, false);
  }, { passive: true });

  hero.addEventListener('touchend', function () {
    if (!tDown) return;
    tDown = false; goTo(cur, true); startAuto();
  });

  /* ---- Timeline handle drag (mouse) ---- */
  var tlDown = false;

  tlHandle.addEventListener('mousedown', function (e) {
    tlDown = true; stopAuto(); e.stopPropagation();
  });

  document.addEventListener('mousemove', function (e) {
    if (!tlDown) return;
    var r   = tlTrack.getBoundingClientRect();
    var pct = Math.max(0, Math.min(100, (e.clientX - r.left) / r.width * 100));
    setFill(pct);
    var z = pct < 33.3 ? 0 : pct < 66.6 ? 1 : 2;
    if (z !== cur) goTo(z, false);
  });

  document.addEventListener('mouseup', function () {
    if (!tlDown) return;
    tlDown = false; goTo(cur, true);
  });

  /* ---- Timeline handle drag (touch) ---- */
  tlHandle.addEventListener('touchstart', function (e) {
    tlDown = true; stopAuto(); e.stopPropagation();
  }, { passive: true });

  document.addEventListener('touchmove', function (e) {
    if (!tlDown) return;
    var r   = tlTrack.getBoundingClientRect();
    var pct = Math.max(0, Math.min(100, (e.touches[0].clientX - r.left) / r.width * 100));
    setFill(pct);
    var z = pct < 33.3 ? 0 : pct < 66.6 ? 1 : 2;
    if (z !== cur) goTo(z, false);
  }, { passive: true });

  document.addEventListener('touchend', function () {
    if (!tlDown) return;
    tlDown = false; goTo(cur, true);
  });

  /* ---- Timeline track click ---- */
  tlTrack.addEventListener('click', function (e) {
    stopAuto();
    var r   = tlTrack.getBoundingClientRect();
    var pct = (e.clientX - r.left) / r.width * 100;
    goTo(pct < 33.3 ? 0 : pct < 66.6 ? 1 : 2, true);
  });

})();
</script>

{% schema %}
{
  "name": "Everambr Hero Banner",
  "settings": [

    { "type": "header", "content": "━━━ GLOBAL ACCENT COLOR ━━━" },
    { "type": "color", "id": "accent_color", "label": "Accent Color (progress bar, active dots, active markers)", "default": "#aaff3e" },

    { "type": "header", "content": "━━━ EYEBROW TEXT ━━━" },
    { "type": "text",   "id": "eyebrow",        "label": "Eyebrow Text",       "default": "Functional Eyewear · Designed in Ireland" },
    { "type": "color",  "id": "eyebrow_color",  "label": "Eyebrow Color",      "default": "#a6a6a6" },
    { "type": "range",  "id": "eyebrow_size",   "label": "Eyebrow Font Size",  "min": 8,  "max": 20, "step": 1, "unit": "px", "default": 11 },
    { "type": "select", "id": "eyebrow_weight", "label": "Eyebrow Font Weight",
      "options": [
        { "value": "300", "label": "Light 300" },
        { "value": "400", "label": "Regular 400" },
        { "value": "500", "label": "Medium 500" }
      ], "default": "400" },

    { "type": "header", "content": "━━━ HEADLINE ━━━" },
    { "type": "color",  "id": "headline_highlight_color", "label": "Highlight Word Color", "default": "#aaff3e" },
    { "type": "color",  "id": "headline_suffix_color",    "label": "Suffix Word Color",    "default": "#ffffff" },
    { "type": "range",  "id": "headline_size_min", "label": "Headline Min Size (mobile)", "min": 30, "max": 80,  "step": 2, "unit": "px", "default": 54 },
    { "type": "range",  "id": "headline_size_max", "label": "Headline Max Size (desktop)","min": 60, "max": 160, "step": 2, "unit": "px", "default": 120 },
    { "type": "select", "id": "headline_weight", "label": "Headline Font Weight",
      "options": [
        { "value": "300", "label": "Light 300" },
        { "value": "400", "label": "Regular 400" },
        { "value": "500", "label": "Medium 500" }
      ], "default": "400" },

    { "type": "header", "content": "━━━ SUBTEXT ━━━" },
    { "type": "color",  "id": "subtext_color",  "label": "Subtext Color",      "default": "#b8b8b8" },
    { "type": "range",  "id": "subtext_size",   "label": "Subtext Font Size",  "min": 10, "max": 24, "step": 1, "unit": "px", "default": 14 },
    { "type": "select", "id": "subtext_weight", "label": "Subtext Font Weight",
      "options": [
        { "value": "300", "label": "Light 300" },
        { "value": "400", "label": "Regular 400" },
        { "value": "500", "label": "Medium 500" }
      ], "default": "300" },

    { "type": "header", "content": "━━━ TABS ━━━" },
    { "type": "color",  "id": "tab_color",       "label": "Tab Text Color (inactive)",  "default": "#8c8c8c" },
    { "type": "color",  "id": "tab_active_bg",   "label": "Tab Active Background",      "default": "#aaff3e" },
    { "type": "color",  "id": "tab_active_color","label": "Tab Active Text Color",      "default": "#111111" },
    { "type": "range",  "id": "tab_size",         "label": "Tab Font Size",              "min": 10, "max": 18, "step": 1, "unit": "px", "default": 13 },
    { "type": "select", "id": "tab_weight",       "label": "Tab Font Weight",
      "options": [
        { "value": "300", "label": "Light 300" },
        { "value": "400", "label": "Regular 400" },
        { "value": "500", "label": "Medium 500" }
      ], "default": "400" },

    { "type": "header", "content": "━━━ TIMELINE TAGLINE ━━━" },
    { "type": "text",   "id": "tagline_word1",   "label": "Tagline Word 1", "default": "AMBITION" },
    { "type": "text",   "id": "tagline_word2",   "label": "Tagline Word 2", "default": "MOVEMENT" },
    { "type": "text",   "id": "tagline_word3",   "label": "Tagline Word 3", "default": "BALANCE"  },
    { "type": "text",   "id": "tagline_word4",   "label": "Tagline Word 4", "default": "RECOVERY" },
    { "type": "color",  "id": "tagline_color",   "label": "Tagline Color",      "default": "#595959" },
    { "type": "range",  "id": "tagline_size",    "label": "Tagline Font Size",  "min": 8,  "max": 16, "step": 1, "unit": "px", "default": 10 },
    { "type": "select", "id": "tagline_weight",  "label": "Tagline Font Weight",
      "options": [
        { "value": "300", "label": "Light 300" },
        { "value": "400", "label": "Regular 400" },
        { "value": "500", "label": "Medium 500" }
      ], "default": "400" },

    { "type": "header", "content": "━━━ TIMELINE MARKERS ━━━" },
    { "type": "text",   "id": "marker1",         "label": "Marker 1 Text", "default": "06:00 — TRAIN" },
    { "type": "text",   "id": "marker2",         "label": "Marker 2 Text", "default": "12:00 — WORK"  },
    { "type": "text",   "id": "marker3",         "label": "Marker 3 Text", "default": "22:00 — SLEEP" },
    { "type": "color",  "id": "marker_color",    "label": "Marker Color (inactive)",  "default": "#4d4d4d" },
    { "type": "range",  "id": "marker_size",     "label": "Marker Font Size",   "min": 8,  "max": 14, "step": 1, "unit": "px", "default": 10 },
    { "type": "select", "id": "marker_weight",   "label": "Marker Font Weight",
      "options": [
        { "value": "300", "label": "Light 300" },
        { "value": "400", "label": "Regular 400" },
        { "value": "500", "label": "Medium 500" }
      ], "default": "400" },

    { "type": "header", "content": "━━━ SLIDE 1 — TRAIN ━━━" },
    { "type": "image_picker", "id": "slide1_image",   "label": "Background Image" },
    { "type": "text",         "id": "slide1_tab",     "label": "Tab Label",      "default": "train"   },
    { "type": "text",         "id": "slide1_word",    "label": "Highlight Word", "default": "train"   },
    { "type": "text",         "id": "slide1_suffix",  "label": "Suffix Word",    "default": "harder." },
    { "type": "text",         "id": "slide1_subtext", "label": "Subtext",        "default": "Performance sunglasses engineered for sun and speed." },

    { "type": "header", "content": "━━━ SLIDE 2 — WORK ━━━" },
    { "type": "image_picker", "id": "slide2_image",   "label": "Background Image" },
    { "type": "text",         "id": "slide2_tab",     "label": "Tab Label",      "default": "work"    },
    { "type": "text",         "id": "slide2_word",    "label": "Highlight Word", "default": "work"    },
    { "type": "text",         "id": "slide2_suffix",  "label": "Suffix Word",    "default": "deeper." },
    { "type": "text",         "id": "slide2_subtext", "label": "Subtext",        "default": "Blue light glasses for screens and harsh office light." },

    { "type": "header", "content": "━━━ SLIDE 3 — SLEEP ━━━" },
    { "type": "image_picker", "id": "slide3_image",   "label": "Background Image" },
    { "type": "text",         "id": "slide3_tab",     "label": "Tab Label",      "default": "sleep"   },
    { "type": "text",         "id": "slide3_word",    "label": "Highlight Word", "default": "sleep"   },
    { "type": "text",         "id": "slide3_suffix",  "label": "Suffix Word",    "default": "better." },
    { "type": "text",         "id": "slide3_subtext", "label": "Subtext",        "default": "Strong evening light-blockers for your most restful sleep." }

  ],
  "presets": [{ "name": "Everambr Hero Banner" }]
}
{% endschema %}
