# CLAUDE.md — portfolio-demos

Single-file interactive demos, one per `<slug>/index.html`. Root `index.html` is the
portfolio landing — a `SECTIONS` array renders the cards (strength-ranked, real products
first, creative pieces in the **"Creative & interactive"** section). Deployed via GitHub
Pages → `s1rt3ge.github.io/portfolio`. No Node/Python in this shell; preview + screenshots
go through headless Edge + a PowerShell HttpListener (see the project memory for exact
commands).

## House design style — the default for any new "creative / showcase / make-something-cool" demo

Goal: cinematic, premium, awwwards-grade single-page pieces. The bar is set by `laocoon/`,
`lumora/`, `loopstack/` — study those three before building a new one. (This is the default
for *visual showcase* pieces; functional app demos — CRM, dashboards, etc. — keep their own
clean product UI and do NOT need to be forced into this aesthetic.)

### Aesthetic
- **Palette:** near-black (`#000`/`#0a0a0a`) or near-white surfaces, and exactly **one**
  saturated accent (neon green, burnt bronze, electric blue…). High contrast, almost no
  other color.
- **Type:** editorial pairing — a display serif (Playfair Display, Italiana) + a clean
  grotesk/sans (Outfit, General Sans, Inter). Oversized type, tight tracking, a giant
  wordmark pinned to an edge.
- **Space & pacing:** lots of negative space; slow, luxurious, restrained. Calm over busy.

### Signature ingredients (use 2–4 per piece, never all at once)
- **One hero centerpiece that owns the screen:** WebGL 3D (Three.js + a GLTF model or a
  particle system), a fullscreen GLSL shader, a canvas effect (liquid / displacement /
  trail), or a faded looping video.
- **Masked text reveals:** wrap each word/letter in `overflow:hidden` + an inner span that
  animates `translateY/X(100%→0)` + `filter:blur(20px→0)`, staggered per word/letter.
  Easing `cubic-bezier(0.05,0.9,0.1,1)` (~1.2–1.3s) or `cubic-bezier(0.16,1,0.3,1)`.
- **Scroll-driven everything:** a tall page with `position:fixed` layers (or Lenis smooth
  scroll); drive camera / shader / reveals from scroll progress; smooth with rAF lerp
  (`cur += (target - cur) * 0.025`).
- **Custom cursor:** an instant ring + a lagging element (glass pill / dot) with LERP
  physics and hover states; disable on touch / `prefers-reduced-motion`.
- **Editorial overlay:** hairline grid lines, thin dividers, small uppercase mono labels, a
  fixed header (brand + nav + one CTA), a slim progress indicator.
- **An intro moment:** a loader/counter or a first-paint reveal, then the page "arrives."

### Tech rules
- One self-contained `index.html`: all CSS in one `<style>`, all JS in one `<script>` (or
  `<script type="module">`). No build step, no framework, no bundler.
- CDN libs only when truly needed, via importmap (e.g. `three@0.160.0`, `lenis`). Fonts via
  `@import` / `<link>`. Assets from their bucket URL or generated procedurally.
- rem-based adaptive grid (root font-size scales with viewport) for proportional layouts.
- Respect `prefers-reduced-motion`; hide native scrollbars/cursor only when something
  replaces them. Hardcode constants; keep LCP (first image/frame) fast.

## Shipping a new demo (the established workflow)
1. Build `<slug>/index.html`.
2. Verify with headless Edge (`--headless --virtual-time-budget=… --screenshot`; add
   `--enable-unsafe-swiftshader --use-angle=swiftshader` for WebGL). All-`fixed` scroll
   pages only capture cleanly at scroll-0.
3. Add a card to the right `SECTIONS` group in root `index.html` (strongest-first; creative
   pieces → "Creative & interactive"). Generate desktop (1280×769@2x) + mobile (390×844@2x)
   thumbnails → `_gig-screens/<slug>.jpg`; register `<slug>` in `gig-shots.js`.
4. Commit + push, then confirm live on Pages.
