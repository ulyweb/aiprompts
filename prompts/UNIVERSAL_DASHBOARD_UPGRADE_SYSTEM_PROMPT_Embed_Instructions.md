# UNIVERSAL DASHBOARD UPGRADE — SYSTEM PROMPT (v1.0)
**Author: Nobody**
**Purpose:** A single, copy‑paste master prompt that force‑installs every enhancement we built on the YubiKey Readiness Monitor onto **any** existing single‑file HTML telemetry dashboard — new or old. Feed this prompt to an AI agent (Copilot / Claude Code) **together with the target `.html` file** and it will additively upgrade it without breaking existing features.

---

> ## 📌 EMBED INSTRUCTIONS — READ FIRST
> **What this is:** a drop‑in upgrade brief. It does **not** run by itself — an AI agent applies it to a file you provide.
>
> **How to embed it into an existing project (old or new):**
> 1. **Open a chat** with your AI agent (Copilot / Claude Code) and **attach the target single‑file `.html`** you want upgraded.
> 2. **Paste this entire `.md`** as the instructions in the same message.
> 3. **Edit §1 SELECTOR MAP** so the logical roles point at that project's real class/id names (skip if it already matches the defaults).
> 4. **Pick scope (optional):** keep all modules, or tell the agent which of §3–§7 to apply. Storage isolation (§2) and the §9 checklist are **always** required.
> 5. **Run it.** The agent must return the **complete** upgraded file (never a snippet), bump the version + filename, and report the §9 acceptance checklist.
>
> **Guardrails the agent must honor every time:** additive‑only · nothing removed · single file · zero external dependencies · 100% client‑side / offline · `node --check` clean · opens by double‑click · byline `Author: Nobody` · no personal names or program‑specific wording in shipped UI.
>
> **Tip for many projects:** save this file once (e.g. `\\prompts\\UNIVERSAL_DASHBOARD_UPGRADE_SYSTEM_PROMPT.md`) and reuse it verbatim for each dashboard — only §1 changes per project.

---

## 0. HOW TO USE
1. Attach or paste the **single‑file HTML** you want to upgrade.
2. Paste this entire prompt.
3. (Optional) In **§1 SELECTOR MAP**, override the class names if the target project uses different ones.
4. The agent must apply **all modules**, self‑verify (§9), bump the version + filename, and return the **complete** file — never a snippet.

> **Non‑negotiables:** additive only · nothing removed · single file · zero external dependencies · 100% client‑side · offline‑capable · `node --check` clean · works by double‑click. Byline in any authored code = **`Author: Nobody`** (no name/title after it).

---

## 1. SELECTOR MAP (edit to match the target project)
The modules below reference these logical roles. Map each to the target's **real** class/id. Defaults shown are the YubiKey project's names.

| Role | Default selector |
|---|---|
| Card/panel surface | `.panel` |
| KPI tile | `.kpi` (+ `.audkpi`) |
| Hero block | `.hero` |
| Empty‑state banner | `#emptyBanner` |
| Modal dialog | `.overlay .modal` |
| Right‑side drawer | `.drawer` |
| Sticky top bar | `.topbar` |
| Sticky sub bar | `.subbar` |
| Buttons | `.btn` |
| Chips | `.chip` · pills `.pill` |
| Small cards | `.stepcard`, `.capchip` |
| Panel registry array | `Panels.defs` (objects `{id,ico,title,wide,render}`) |
| Render namespace | `Render` (methods `renderX`) |
| localStorage helper | `LS.get/set` with a **per‑tool prefix** |
| Toast helper | `UI.toast(msg,kind,ms)` |

If the target lacks a `Panels`/`Render` pattern, the agent must create a minimal equivalent or attach modules to the closest existing structure **without** restructuring the DOM.

---

## 2. STORAGE ISOLATION (do this first — prevents cross‑tool bleed)
- Give the tool a **unique localStorage prefix** and a **unique IndexedDB name** so two dashboards opened from the same origin never read each other's state.
- Pattern: `LS` keys = `"<toolslug>."` prefix; audit DB = `"<toolslug>_audit"`.
- If the tool ships in multiple variants (e.g. Exec vs All‑Users), each variant gets its **own** prefix (e.g. `ykbx.` vs `ykau.`).

---

## 3. MODULE A — LIVE CAPABILITY CHIP
A sticky top‑bar chip that feature‑detects the platform and reports **native vs fallback**.
- Markup: `<span class="capchip" id="capChip">API: …</span>` in the top bar.
- Detect at runtime: **Popover API** (`HTMLElement.prototype.hasOwnProperty('showPopover')`), native **`<dialog>`** (`HTMLDialogElement.prototype.showModal`), **IndexedDB**, **Web Crypto SHA‑256** (`crypto.subtle.digest`), **View Transitions** (`document.startViewTransition`).
- Render as: `API: Popover✔ · Dialog✔ · IDB✔ · SHA‑256✔ · VT✔` (✔ native, ⚠ fallback). Tooltip explains each.
- Hidden in `@media print`.

---

## 4. MODULE B — TAMPER‑EVIDENT AUDIT TRAIL (KB0032799 pattern)
A verifiable, hash‑chained audit log registered as its **own panel** (do not restructure the DOM; push into `Panels.defs`).
- **Store:** IndexedDB (`<toolslug>_audit`, keyPath `seq`) with **in‑memory fallback**; writes serialized through a `Promise` queue.
- **Chain:** Web Crypto **SHA‑256** over `seq|ts|event|detail|prevHash`; **genesis = 64 zeros**; **FNV‑1a fallback** (with capability‑chip downgrade) when `crypto.subtle` is unavailable.
- **Panel UI:** 🔎 **Verify Integrity** (pinpoints `BROKEN at entry #N`, red‑tints that row + everything after, **never appends to a broken chain**), ⬆️ **Import + Re‑Verify** (keeps original hashes, malformed → graceful error, no throw), ⬇️ **Export JSON/CSV**, 📝 **Add Note**, 🗑️ **Clear**. KPIs: **Audit Entries** + **Chain Integrity**.
- **Auto‑logger:** a non‑invasive `auditWrap(obj,fn,event,detail)` monkey‑patch. `detail` may be a **string OR a function** — must handle both (never invoke `.apply` on a string). Wrap UI actions only.
- **Privacy:** log UI actions only — **never** PINs, recovery keys, secrets, or PII.

---

## 5. MODULE C — LIQUID GLASS LAYER (+ runtime control)
Append a clearly‑commented, **additive** override block **just before the closing `</style>`** (write the tag as "the closing style tag" inside comments so you never emit a literal early‑closing tag).
- **Variable‑driven depth** in `:root` (tunable at runtime): `--lg-blur --lg-sat --lg-bright --lg-radius --lg-radius-sm --lg-tint --lg-tint-2 --lg-border --lg-rim-top --lg-spec --lg-spec-bottom --lg-shadow-1 --lg-shadow-2 --lg-lift --lg-scale`.
- **Traits:** translucent panel fill; `backdrop-filter: blur() saturate() brightness()` (+`-webkit-`); **specular inset top‑highlight**; **squircle radii ~14–22px**; **multi‑layer shadows incl. an inset‑bottom** depth shadow; **hover lift+scale**; working **LIGHT + DARK** via `html[data-theme]`.
- **Gate & safety:** wrap in `@supports((backdrop-filter:blur(1px)) or (-webkit-backdrop-filter:blur(1px)))`; honor `prefers-reduced-motion` **and** the tool's reduce toggle; add a `@media print` reset.
- **⚠ CRITICAL BUG TO AVOID:** do **NOT** force `position` on `.drawer` or `.overlay .modal`. They rely on `position:fixed` + an off‑screen `transform` to stay hidden; forcing `position:relative` drops them into the page. Split the recipe: give **positioned‑context** surfaces (`.panel,#emptyBanner,.hero`) the full glass, and give `.overlay .modal,.drawer` the **glass look only** (background/backdrop/border/shadow) — never `position`.
- **Runtime control (REQUIRED):** a `GlassCtl` object + a **Settings ▸ Liquid Glass** section with **live sliders** for Blur, Saturation, Corner radius, Tint, Top‑highlight, Hover‑lift; **presets** (Subtle/Default/Vivid/Max); **Reset**; writes to the CSS vars in real time via `document.documentElement.style.setProperty(...)`; **persists** to `LS`; include the glass state in JSON backups; add a command‑palette entry ("Liquid Glass appearance").

---

## 6. MODULE D — UX ENHANCEMENTS (apply where the pattern fits)
- **Panels hidden by default:** invert the visibility model to a **`shown` set** (default `{}` = nothing shown). User reveals panels via chips. Add a single **▢ SHOW ALL / ▣ HIDE ALL** toggle that self‑relabels and stays in sync.
- **Details sliding drawer:** a right‑side `position:fixed` drawer (hidden via `translateX(102%)`) showing a row's full record + recent activity; open by clicking a name (`.linkname`) or an ℹ button; Esc/overlay‑close; include it in the `anyOverlay()` guard.
- **3D / cartoon interaction layer:** tilt‑on‑hover panels (pointer‑driven `perspective(...) rotateX/Y`), KPI pop on hover, a spinning mascot with a confetti click, comic pop toasts, button wiggle/squash — **all** gated by `prefers-reduced-motion` and the reduce toggle.
- **Interactive HOW / walkthrough panel** (if the tool teaches a procedure): mode toggle, animated SVG scenes, play‑demo, step dots, "SAY THIS" coaching lines, arrow‑key nav, copy/print.

---

## 7. NATIVE PLATFORM PROGRESSIVE ENHANCEMENT (optional, feature‑detected)
Only if it won't regress working modals/drawers:
- **Native `<dialog>`** `showModal()` with manual‑overlay fallback.
- **Native Popover API** (`popover=manual` + `showPopover/hidePopover`) with JS fallback. Every native popover MUST include the scrollbar override (UA applies `overflow:auto`): `max-height:calc(100vh - 24px);overflow-y:auto;overscroll-behavior:contain;scrollbar-width:none;-ms-overflow-style:none;` plus `.pop::-webkit-scrollbar{width:0;height:0;display:none;}`.
- **View Transitions** for nav with reduced‑motion awareness.

---

## 8. HOUSEKEEPING
- **Version‑bump** every visible version string + the filename; keep prior milestones in the changelog comment at the top of `<style>`.
- **No personal names** in UI copy (use role/policy words: "IT Support", "the ServiceNow CSV export"). **No** organization‑specific wording that shouldn't ship (e.g. remove program‑specific phrasing when generalizing a variant).
- Keep exactly **one** each of `<style>/</style>`, `</body>`, `</html>`.
- Byline in authored code: **`Author: Nobody`**.

---

## 9. ACCEPTANCE CHECKLIST (agent must self‑verify and report each)
- [ ] Opens by double‑click; zero external requests; works offline.
- [ ] `node --check` passes on all `<script>` blocks (extract and concatenate to verify).
- [ ] Exactly one `<style>/</style>`, `</body>`, `</html>`; Liquid Glass sits **before** the closing style tag; no literal early‑closing tag inside comments.
- [ ] **Storage isolated** (unique LS prefix + IDB name); two variants don't share state.
- [ ] Capability chip renders native‑vs‑fallback correctly.
- [ ] Audit chain: clean → VERIFIED; edit a field → flags **exactly that row**; delete a middle row → flags the break; genesis = 64 zeros; **no append to a broken chain**; malformed import handled gracefully; audit log carries **no** secrets. (Test in Node with WebCrypto + in‑memory mode.)
- [ ] Liquid Glass: variables drive depth; light + dark both work; **drawer/modals stay hidden on load** and open normally (drawer keeps `position:fixed`); reduced‑motion + print resets present.
- [ ] **GlassCtl** sliders/presets/reset update the UI live and persist; glass state in JSON backup.
- [ ] Panels‑hidden‑by‑default + SHOW ALL/HIDE ALL work; details drawer opens/closes; 3D layer respects reduce toggle.
- [ ] No unintended personal names or program‑specific phrasing; version + filename bumped; `Author: Nobody` intact.
- [ ] Returned as the **complete** single file — not a snippet.

---

## 10. CANONICAL REFERENCE TOKENS (optional defaults)
Colors: `--bg #05080d · --cyan #00e5ff · --green #00e08a · --amber #ffb020 · --red #ff3b57 · --violet #a97bff · --gold #e8b54b`; mono font for data. Glass defaults: `blur 16px · sat 175% · radius 18px · tint 6% · spec .30 · lift 2px`. Audit genesis = 64 × `0`. These are starting points — always map to the target project's existing tokens first.
