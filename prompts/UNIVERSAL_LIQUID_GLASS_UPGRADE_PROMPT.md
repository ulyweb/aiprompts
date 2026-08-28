# UNIVERSAL "LIQUID GLASS" DASHBOARD UPGRADE — SYSTEM / KICKOFF PROMPT

> **How to use:** Open the target project in a new AI coding session (Claude Code). Attach or point it at
> the project's main HTML/UI file. Paste **everything between the two rules below** as your first message.
> It is written to be project-agnostic: the AI inspects the target, maps the patterns to that project's
> own class names, and applies the upgrade **surgically and additively** — never regenerating from memory.
>
> Works best on single-file, offline, zero-dependency HTML dashboards, but the Adaptation Guide (§6)
> covers framework/multi-file apps too.

---
────────────────────────────────────────────────────────
SYSTEM / KICKOFF PROMPT — "LIQUID GLASS" HOUSE-STYLE UPGRADE

You are upgrading an EXISTING, owner-critical project to my standard "Liquid Glass" dashboard house
style and feature set. The attached/target UI file is the SINGLE SOURCE OF TRUTH. Do not rebuild it.

## 0. MISSION
Layer my house-style theme + feature enhancements onto this project **without changing what already
works**. All edits are ADDITIVE and SURGICAL (unique string anchors, assert each anchor occurs exactly
once before replacing). Preserve the project's existing branding, content, data, and behavior.

## 1. NON-NEGOTIABLE RULES
1) NEVER regenerate the file from memory. Edit it in place with anchored string replacements.
2) Keep the project's existing identity (logo/name/colors) — the glass is a depth LAYER on top, not a
   reskin. Only adjust accent tokens if the owner asks.
3) If the project is single-file/offline/zero-dependency, keep it that way. No CDNs, no new deps.
4) Persist all new UI state in localStorage under the project's OWN namespace (reuse the existing one;
   if none, create a short prefix and use it consistently).
5) After ANY change, run the VALIDATION CHECKLIST (§5). If a JS syntax check fails or content collapses
   (file shrinks unexpectedly), STOP and fix before delivering.
6) VERSIONING IS MANDATORY AND AUTOMATIC (§4): on EVERY change, bump the version across ALL visible
   version strings AND the filename, in the same pass. Do not wait to be asked.

## 2. PRE-FLIGHT (do this first, report findings, THEN proceed)
- Locate the file on disk (do not assume). Confirm its byte size + a hash you actually computed.
- Inventory: find the `<style>` block, the top bar / header, any panel/card containers, the settings or
  config modal (if any), the localStorage namespace, and the version string(s).
- Identify the project's accent colors and background so the glass layer harmonizes.
- Confirm a JS syntax baseline passes (e.g. extract the last `<script>` and run `node --check`).

## 3. THE UPGRADE PAYLOAD (apply what's relevant; skip what already exists)

**A. Liquid Glass CSS layer (REQUIRED).** Append a clearly-commented, ADDITIVE override block just
before `</style>`. Drive depth from CSS variables so it's tunable at runtime. Map the selectors
(`.panel,.card,.modal,.topbar,...`) to THIS project's real class names. Reference implementation in §7A.
Key traits: translucent panel fill, `backdrop-filter: blur() saturate()`, specular inset top-highlight,
squircle radii (~14–22px), multi-layer depth shadows (incl. an inset-bottom shadow), hover lift+scale,
and working LIGHT + DARK themes.

**B. Adjustable-glass engine (REQUIRED).** Add a small top-level `Glass` object that reads/writes
`{blur, fill, depth}` from localStorage and sets `--g-blur / --g-fill-a / --g-shadow-a` on
`documentElement`; call `Glass.apply()` during init. Reference in §7B.

**C. Settings / Control Center (REQUIRED).** Ensure there is ONE settings entry point. Inside it add:
   - a **Quick Actions** row (move the header's action buttons here — theme, refresh, export, welcome,
     command palette, fullscreen, etc.), and
   - an **Appearance** section with live **Blur / Panel-Fill / Depth** range sliders (+ **Reset**) wired
     to `Glass.set(...)` so changes preview instantly and persist. Reference in §7C.
   Make the settings overlay semi-transparent so appearance edits preview live behind it.

**D. Consolidated top bar (REQUIRED).** Remove the scattered header action buttons (now in Settings).
Leave the header minimal: brand + a live/status indicator + ONE settings (⚙) button + clock/meta.
Keep keyboard shortcuts working for the moved actions.

**E. LIVE / status-indicator boost (REQUIRED if the UI has a live/status pill).** Make it clearly
visible over the frosted bar: stronger fill + border, outer color glow, bold text, and a brighter
PULSING dot (`@keyframes`). Give status pills (ok/warn/risk) stronger bg/border/glow. Reference in §7D.

**F. Panel manager (add if the project shows panels/cards and lacks it).** Controls: Show all / Hide all
/ Expand / Collapse / per-panel chips, with per-panel hidden+collapsed state persisted in localStorage.

**G. Interactive detail drawer + journey behavior (REQUIRED if the UI has steps/cards with detail).**
This is the core "learn + do" experience — treat it as first-class, not decoration. Implement ALL of:
   - **Sliding detail drawer** (`Knowledge.open/toggle/close/completeAndClose/copy`): a right-slide panel
     that animates in and shows structured detail for the selected item — **Objective / Procedure / Stop
     Conditions / Escalation**, plus a **Copy** button for that item's content.
   - **Single-click a step/card = toggle its details** in the drawer (open to learn; click the SAME step
     again = close). Clicking a different step swaps the drawer content.
   - **Double-click a step/card = mark it COMPLETE and auto-close the drawer** (`completeAndClose` flips
     that step's stored state). This must recompute any readiness %/donut/lamp/status.
   - **State affordances:** completed steps glow green; the next actionable step glows gold ("now"); a
     progress bar/underline fills on completion. Each card shows a hint like
     "CLICK DETAILS · DBL-CLICK COMPLETE + CLOSE".
   - **Card depth:** subtle hover lift / slight 3D tilt on the interactive cards (glass, see §7A).
   - Persist completion state in localStorage; drawer closes on `Esc` and on a close (×) button.
   Reference in §7F.

**H. Panel open/close (REQUIRED if the UI has panels).** Per-panel **collapse/expand** (hide/show the
panel body) AND **show/hide** the whole panel, with both states persisted per panel in localStorage —
plus the **panel manager** controls: Show all / Hide all / Expand / Collapse / per-panel chips.

**I. Optional chrome (add only if it fits the project).** Welcome splash (first-run, dismiss persisted,
reopenable via a button/hotkey), ⌘K command palette, count-up KPI/donut telemetry.

## 4. VERSIONING DISCIPLINE (every change, automatically)
- Bump the version in the SAME pass across ALL visible version strings: page/window `<title>`, on-screen
  header/footer, in-code version constant, toasts/labels, and any prose "vX adds…" mention.
- Bump the FILENAME too (`..._vX_Y_Z...`). Keep the prior milestone file (never delete milestones).
- Sweep with a regex like `v\d+\.\d+(?:\.\d+)?` to catch stray references before delivering.
- Feature work = minor bump; small fixes = patch bump. If the project is audited, record new size + hash.

## 5. VALIDATION CHECKLIST (run every time, no exceptions)
- [ ] File found on disk (not assumed); size + hash reported.
- [ ] JS syntax OK (e.g. `node --check` on the last `<script>`), MUST pass.
- [ ] Any embedded/base64 payloads unchanged (verify bytes/hash) unless intentionally edited.
- [ ] No unexpected size collapse; additive growth is fine.
- [ ] Rendered in a browser: theme toggles, sliders live-update AND persist across reload, panels/manager
      work, live indicator visible, both light + dark readable.
- [ ] Version bumped across ALL strings + filename; zero stale version refs (regex swept).
- [ ] localStorage namespace unchanged/consistent.
- Report: size delta, new hash, exactly what changed.

## 6. ADAPTATION GUIDE (non-single-file / framework projects)
- React/Vue/etc.: put §7A CSS in the global stylesheet; make `Glass` a small module/hook that sets the
  CSS vars on `document.documentElement` and persists to localStorage; render the sliders in your
  existing settings component. Same variables, same behavior.
- Multi-file sites: add the CSS layer to the shared stylesheet and the `Glass` script to a shared JS
  include so every page inherits it.
- If there is no settings UI yet, create a minimal modal/drawer to host Quick Actions + Appearance.
- Keep everything else identical to §3–§5.

ACKNOWLEDGE by (a) reporting the file size + hash you actually see and the namespace/version/selectors
you found, then (b) proceed through §3, validating per §5. Do not ask questions the file already answers.
────────────────────────────────────────────────────────

---

## 7. REFERENCE CODE (generalize the selectors/namespace to the target project)

### 7A. Liquid Glass CSS override layer (paste before `</style>`; map selectors to the project)
```css
/* ===== Liquid Glass (user-adjustable via Settings → Appearance) — additive; safe to tweak/remove ===== */
:root{
  --glass-hi:rgba(255,255,255,.16); --glass-lo:rgba(255,255,255,0); --glass-edge:rgba(255,255,255,.22);
  --g-blur:30px; --g-sat:190%; --g-fill:14,21,33; --g-fill-a:.60; --g-shadow-a:.88; /* fallback defaults */
}
html[data-theme=light]{ --glass-hi:rgba(255,255,255,.75); --glass-edge:rgba(20,50,70,.26); --g-fill:255,255,255; }

/* map .panel,.card,.modal to THIS project's containers */
.panel,.card,.modal{
  background:linear-gradient(180deg,var(--glass-hi),var(--glass-lo) 46%),rgba(var(--g-fill),var(--g-fill-a));
  -webkit-backdrop-filter:blur(var(--g-blur)) saturate(var(--g-sat));
  backdrop-filter:blur(var(--g-blur)) saturate(var(--g-sat));
  border:1px solid var(--glass-edge); border-radius:18px;
  box-shadow:0 1px 0 rgba(255,255,255,.24) inset, 0 0 0 1px rgba(255,255,255,.05) inset,
             0 -16px 32px -26px rgba(0,0,0,var(--g-shadow-a)) inset,
             0 32px 66px -22px rgba(0,0,0,var(--g-shadow-a)),
             0 14px 32px -16px rgba(0,0,0,calc(var(--g-shadow-a)*.7));
  transition:transform .24s cubic-bezier(.2,.7,.2,1),box-shadow .24s,border-color .24s;
}
.panel:hover,.card:hover{
  transform:translateY(-5px) scale(1.007); border-color:rgba(0,229,255,.5);
  box-shadow:0 1px 0 rgba(255,255,255,.3) inset, 0 44px 84px -24px rgba(0,0,0,var(--g-shadow-a)),
             0 0 36px -6px rgba(0,229,255,.32);
}
.topbar,.subbar{ -webkit-backdrop-filter:blur(28px) saturate(185%); backdrop-filter:blur(28px) saturate(185%); }
.overlay{ -webkit-backdrop-filter:blur(18px) saturate(165%); backdrop-filter:blur(18px) saturate(165%); }
/* make the settings overlay semi-transparent for live preview: */
#settingsModal{ background:rgba(2,5,10,.5); }
/* appearance-slider styling */
.rng{width:100%;accent-color:#e8b54b;margin:2px 0 8px}
.rngrow{display:flex;justify-content:space-between;align-items:center;font:9px var(--mono,monospace);letter-spacing:.1em;opacity:.8}
.setsec{font:9.5px var(--mono,monospace);letter-spacing:.16em;text-transform:uppercase;margin:16px 0 7px;padding-top:12px;border-top:1px solid rgba(255,255,255,.08)}
```

### 7B. Adjustable-glass engine (top-level JS; call `Glass.apply()` in init)
```js
const Glass={
  def:{blur:30,fill:60,depth:88},
  get(){return Object.assign({},this.def,LS.get('glass',{}))},           // LS = your localStorage helper
  apply(){var g=this.get(),r=document.documentElement.style;
    r.setProperty('--g-blur',g.blur+'px');
    r.setProperty('--g-fill-a',(g.fill/100).toFixed(2));
    r.setProperty('--g-shadow-a',(g.depth/100).toFixed(2));},
  set(k,v){var g=this.get();g[k]=+v;LS.set('glass',g);this.apply();},
  reset(){LS.set('glass',{});this.apply();/* re-render settings */}
};
// in init: Glass.apply();
```

### 7C. Appearance section markup (inside the settings modal body)
```html
<div class="setsec">Appearance — Liquid Glass</div>
<div class="rngrow"><span>BLUR</span><span id="glBlurV"></span></div>
<input type="range" class="rng" min="8"  max="48"  oninput="Glass.set('blur',this.value);glBlurV.textContent=this.value+'px'">
<div class="rngrow"><span>PANEL FILL · lower = more see-through</span><span id="glFillV"></span></div>
<input type="range" class="rng" min="25" max="90"  oninput="Glass.set('fill',this.value);glFillV.textContent=this.value+'%'">
<div class="rngrow"><span>DEPTH / SHADOW</span><span id="glDepthV"></span></div>
<input type="range" class="rng" min="40" max="100" oninput="Glass.set('depth',this.value);glDepthV.textContent=this.value+'%'">
<button onclick="Glass.reset()">↺ Reset Glass</button>
<!-- set each slider's value + label from Glass.get() when the modal opens -->
```

### 7D. LIVE / status indicator boost
```css
.live{border:1px solid rgba(0,224,138,.8)!important;background:rgba(0,224,138,.2)!important;
      font-weight:800;letter-spacing:.16em;text-shadow:0 0 8px rgba(0,224,138,.55);
      box-shadow:0 0 18px -2px rgba(0,224,138,.6),0 1px 0 rgba(255,255,255,.2) inset!important}
.live .dot{width:9px;height:9px;background:#2effb0;
      box-shadow:0 0 13px 2px rgba(0,224,138,.95),0 0 5px #2effb0;animation:livepulse 1.5s infinite}
@keyframes livepulse{0%,100%{transform:scale(1);opacity:1}50%{transform:scale(1.4);opacity:.5}}
.sp-ok  {background:rgba(0,224,138,.2)!important;border-color:rgba(0,224,138,.75)!important;box-shadow:0 0 15px -3px rgba(0,224,138,.55)!important}
.sp-warn{background:rgba(255,176,32,.2)!important;border-color:rgba(255,176,32,.75)!important;box-shadow:0 0 15px -3px rgba(255,176,32,.55)!important}
.sp-risk{background:rgba(255,59,87,.22)!important;border-color:rgba(255,59,87,.8)!important;box-shadow:0 0 16px -3px rgba(255,59,87,.6)!important}
```

### 7F. Interactive detail drawer + journey (behavior model; map ids/classes to the project)
```js
// state = {stepId:true} completion map, persisted in localStorage
const Knowledge={
  cur:null,
  open(id){this.cur=id; drawer.classList.add('open'); drawerBody.innerHTML=DETAIL[id];},   // DETAIL[id] = {Objective/Procedure/Stop/Escalation} markup
  toggle(id){ (this.cur===id && drawer.classList.contains('open')) ? this.close() : this.open(id); },
  close(){ drawer.classList.remove('open'); this.cur=null; },
  completeAndClose(id){ state[id]=true; LS.set('checks',state); this.close(); update(); }, // update() recomputes readiness/donut/lamp/status
  copy(id){ navigator.clipboard?.writeText(DETAIL_TEXT[id]); }
};
// wire each step/card:  onclick=Knowledge.toggle(id)   ondblclick=Knowledge.completeAndClose(id)
// Esc closes the drawer. Completed card -> .done (green); next incomplete -> .now (gold).
```
```css
.drawer{position:fixed;top:0;right:0;height:100vh;width:min(600px,calc(100vw - 20px));z-index:260;
  transform:translateX(105%);transition:.32s;display:flex;flex-direction:column}   /* glass via §7A */
.drawer.open{transform:translateX(0)}
.step,.card{transition:.18s;cursor:pointer}
.step:hover,.card:hover{transform:perspective(700px) rotateX(2deg) rotateY(-3deg) translateY(-2px)} /* subtle 3D tilt */
.step.done{border-color:rgba(0,224,138,.5);background:rgba(0,224,138,.07)}   /* completed = green */
.step.now {border-color:var(--gold,#e8b54b);background:rgba(232,181,75,.09)} /* next = gold glow */
.step .hint{font:8.5px var(--mono,monospace);letter-spacing:.08em;color:var(--gold,#e8b54b)} /* "CLICK DETAILS · DBL-CLICK COMPLETE + CLOSE" */
```

### 7E. Tuning cheatsheet
- Stronger/"more pop": raise `--g-blur`, LOWER `--g-fill-a` (more see-through), raise `--g-shadow-a`.
- Subtler: reverse the above. All three are live-controllable via the §7C sliders.
- Accent colors: swap the gold `#e8b54b` / cyan `#00e5ff` / green `#00e08a` references for the project's.

*This prompt encodes the same enhancements shipped in Backup-to-Box v3.6.0: Liquid Glass depth,
user-adjustable glass, consolidated top bar, boosted LIVE/status indicators, the interactive detail
drawer + single-click-to-learn / double-click-to-complete journey (with green/gold state and copy-ready
detail: Objective/Procedure/Stop Conditions/Escalation), per-panel open/close + panel manager, and the
version+filename bump discipline.*
