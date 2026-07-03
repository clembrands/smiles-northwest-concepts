# Handoff: Smiles Northwest — Homepage Concepts Gallery

## Overview
Four homepage design concepts for **Smiles Northwest**, a boutique family & cosmetic
dental practice in Beaverton, OR. The goal of this handoff is to turn the concepts into
a small **client-review website**: one central **gallery/index page** that shows a
thumbnail of each concept, and clicking a thumbnail opens that concept as its own
full-screen HTML page.

## About the Design Files
The file `Smiles Northwest Homepages.dc.html` is a **design reference created in HTML**,
not production code to ship as-is. All four concepts currently live inside that ONE file
as sibling `<section>` elements on a pan/zoom "canvas" (a presentation surface). Your job
is to **split them into standalone pages and build an index that links to them** — recreating
the visuals faithfully in whatever environment you choose (plain static HTML is perfectly
fine here; a React/Vite app is also fine if you prefer).

The concepts are marked up with inline styles only (no shared stylesheet) and use a small
web component, `image-slot.js`, for the photo placeholders. See "Image placeholders" below.

## Fidelity
**High-fidelity.** Colors, typography, spacing, copy, and interactions are final-intent.
Recreate pixel-faithfully. Exact hex values and fonts are listed under "Design Tokens."

---

## Target architecture (what to build)

```
/                 → index.html      (gallery: 4 thumbnails, clickable)
/concepts/2a.html → Concept 2A  "Warm & Approachable"   (full page)
/concepts/1a.html → Concept 1A  "Editorial Boutique"    (full page)
/concepts/1b.html → Concept 1B  "Clean Clinical Calm"   (full page)
/concepts/1c.html → Concept 1C  "Modern Tech-Forward"   (full page)
```

### Step 1 — Split the one design file into four standalone pages
Open `Smiles Northwest Homepages.dc.html`. Inside it, each concept is one
`<div class="dv-opt" id="…">` wrapper containing a `.dv-card` with a fake browser chrome
bar and then the actual homepage markup. The four ids are **`2a`, `1a`, `1b`, `1c`**.

For each concept, create `concepts/<id>.html` containing:
- A normal HTML document shell (`<!doctype html><html><head>…`).
- The **same Google Fonts `<link>`** used in the original `<helmet>` (Cormorant Garamond,
  Manrope, Spectral, Mulish, Space Grotesk).
- `<script src="../image-slot.js"></script>` in the `<head>` (needed for photo slots).
- `body{margin:0}` plus the concept's homepage markup — i.e. the inner content of that
  concept's coloured page `<div>` (the element **inside** `.dv-card`, right after the
  `.dv-chrome` bar). Drop the `.dv-opt`, `.dv-olabel`, `.dv-card`, and `.dv-chrome`
  wrappers — those are presentation scaffolding, not part of the website.
- The scroll-reveal used in Concept 1C: copy the `.c-img` CSS rules and the
  `IntersectionObserver` snippet (in the original's logic class / `componentDidMount`)
  into a plain `<script>` that runs on `DOMContentLoaded`.

Adjust image paths: the concepts reference `uploads/logo-navy.png`, `uploads/logo-teal.png`,
`uploads/logo-white2.png`. Repoint them to the copies in `assets/` (`logo-navy.png`,
`logo-teal.png`, `logo-white.png`).

### Step 2 — Build the central gallery `index.html`
A responsive grid of four cards. Each card = a **live scaled-down preview** of the full
page + a label, wrapped in an `<a>` that links to the full page. The simplest robust way to
get a true thumbnail is to embed the real page in an `<iframe>` and CSS-scale it down
(no screenshots to maintain):

```html
<a class="thumb" href="concepts/2a.html">
  <div class="frame">
    <iframe src="concepts/2a.html" scrolling="no" tabindex="-1"></iframe>
  </div>
  <div class="meta"><b>2A · Warm &amp; Approachable</b><span>Cream + bold blue + coral</span></div>
</a>
```

```css
.thumb{display:block;text-decoration:none;color:inherit}
.frame{position:relative;width:100%;aspect-ratio:16/10;overflow:hidden;
       border-radius:14px;border:1px solid rgba(0,0,0,.1);box-shadow:0 6px 24px rgba(0,0,0,.1)}
/* The page is authored at 1440px wide. Scale = frame width / 1440.
   Render the iframe at 1440px then scale it to fit. */
.frame iframe{position:absolute;top:0;left:0;width:1440px;height:2400px;border:0;
              transform:scale(calc(var(--w) / 1440));transform-origin:top left;pointer-events:none}
```
Set `--w` to the rendered card width (e.g. with a `ResizeObserver`, or hardcode the grid
column width and compute the scale from it — e.g. a 520px column → `transform:scale(.361)`).
Put a transparent overlay div above the iframe so the whole card stays clickable and the
iframe doesn't swallow the click (`pointer-events:none` on the iframe already handles this).

Give the page a calm neutral background (`#e8e5df`) and a short header:
"Smiles Northwest — Homepage concepts · click any concept to view the full page."

### Step 3 (optional, nicer thumbnails)
If scaled iframes feel heavy (4 live pages loading at once), swap them for static preview
images: screenshot each full page top-section at 1440px wide, save to
`assets/previews/<id>.png`, and use `<img>` instead of `<iframe>`. Ask the client which they
prefer. Lazy-load either way (`loading="lazy"`).

---

## The four concepts

All share: primary CTA is **Call (503) 644‑9200** (placeholder number — confirm real line),
secondary CTA "Request appointment / Become a patient". Location line: *Beaverton · serving
SW Portland & Hillsboro*. Doctors: **Dr. Chen** (implants/advanced) and **Dr. Dotson** (25+ yrs).
Services featured: Cosmetic, Dental Implants, Invisalign, Myobrace (labeled "rare in area"),
TMJ & Sleep Apnea, General & Periodontal. Technology callouts: Overjet Vision AI, iTero &
Primescan, infrared imaging, Smilecloud.

### 2A — "Warm & Approachable"
- **Mood:** friendly, community, family-forward. Inspired by a reference site the client liked.
- **Type:** Spectral (serif headings), Mulish (body).
- **Palette:** cream `#f4ead2` / off-cream `#fbf7ec`, bold blue `#22508f`, coral `#f2694f`.
- **Signature moves:** top contact bar (blue), centered logo nav, **pill buttons** (fully
  rounded), a **blue brush-stroke band** (hand-drawn SVG wave top & bottom edges) framing a
  quote, three **care-pillar circles** (coral / blue / charcoal) with line icons, smile-swoosh
  under the hero echoing the logo.
- **Logo:** navy version (`logo-navy.png`).

### 1A — "Editorial Boutique"
- **Mood:** dark, warm, magazine-like, luxe (closest to the client's Salathe reference).
- **Type:** Cormorant Garamond (display serif, incl. italics), Manrope (body/labels).
- **Palette:** espresso `#211e19` / `#1b1814`, bone text `#efe9df`/`#f5efe5`, gold-taupe accent
  `#b79f86` / `#cbb193`.
- **Signature moves:** full-bleed photography, giant serif headline with italic accent, oversized
  serif phone number in the CTA band, 3-col signature-services grid on dark, thin-rule stat strip.
- **Logo:** white version (`logo-white.png`).

### 1B — "Clean Clinical Calm"
- **Mood:** light, airy, trustworthy, most approachable/clinical of the set.
- **Type:** Spectral (serif headings), Mulish (body).
- **Palette:** warm white `#f7f5f1`/`#fbfaf7`, ink `#212b29`, sage `#6f8f84`, deep green CTA
  `#3c7a67`, star gold `#e0b84e`.
- **Signature moves:** split hero (text + image), "Trusted technology" logo strip (mockup partner
  logos — see Assets), 3×2 white service cards each with a **rounded inset photo** on top, a
  circular patient portrait above the testimonial, rounded green CTA panel.
- **Logo:** teal/green version (`logo-teal.png`).

### 1C — "Modern Tech-Forward"
- **Mood:** bold, confident, structured; leans into the advanced-technology story.
- **Type:** Space Grotesk (display), Manrope (body).
- **Palette:** forest green `#123a2e` / `#16241d`, cream `#f4f1ea`, amber accent `#c98a3a`,
  mint text `#8fbfa8`.
- **Signature moves:** large tight headline, **hero background is an uploadable image with a
  left-to-right dark-green overlay** (`.c1-hero-ov` — overlay only appears once an image is
  present, keeping the headline legible), a stat bar card overlapping the hero, an **asymmetric
  service grid** (one tall "Signature" card + smaller cards) where **each card has a full-bleed
  photo on top** and the image→text edge is the divider, and a **scroll-reveal**: card images
  sit under a cream wash (`.c-img::after`, `rgba(244,241,234,.62)`) that fades to full colour
  when the card scrolls into view (`IntersectionObserver`, threshold 0.4, reveal-once).
- **Logo:** white version (`logo-white.png`).

---

## Design Tokens (quick reference)
- **Neutrals / creams:** `#e8e5df` (gallery bg), `#f4ead2`, `#fbf7ec`, `#f4f1ea`, `#f7f5f1`, `#fbfaf7`, `#efe3c6`
- **Blues:** `#22508f` (2A brand), `#1b447c`
- **Coral:** `#f2694f` (2A accent)
- **Greens:** `#3c7a67` (1B CTA / teal logo), `#6f8f84` (sage), `#123a2e` & `#16241d` (1C forest), `#8fbfa8` (mint)
- **Warm darks:** `#211e19`, `#1b1814`, `#17140f` (1A)
- **Gold/amber:** `#b79f86`, `#cbb193` (1A), `#c98a3a` (1C), `#e0b84e` (star rating)
- **Radius:** pill buttons `40px` (2A); cards `16–18px`; inset images `12px`
- **Buttons:** 2A = fully-rounded pills; 1A = square/upper-case let-spaced; 1B = rounded 30px;
  1C = square, weight 700.
- **Fonts (Google):** Cormorant Garamond, Manrope, Spectral, Mulish, Space Grotesk.

## Image placeholders (`image-slot.js`)
Every photo area is a `<image-slot id="…" placeholder="…">` web component: a drag-and-drop /
click-to-browse target that persists the dropped image. In this review site it lets the client
drop real photos into the mockups. **For your production build, replace each `<image-slot>` with
a normal `<img>`** pointing at the final asset (or keep the component if you want the client to
keep swapping images during review). Slot ids describe the intended shot (e.g.
`nw-1c-svc-cosmetic`, `nw-1a-hero`, `nw-1b-testimonial`). Note: 1B's service-card slots
intentionally share ids with 1C's (`nw-1c-svc-*`) so the same dropped photo appears in both.

## Assets
- `assets/logo-white.png`, `logo-navy.png`, `logo-teal.png` — the Smiles Northwest wordmark
  (serif + smile-swoosh) in three colourways. Recolour from the white master if you need others.
- The "Trusted technology" row in 1B and the tech list use **mockup partner logos** (simple
  grey mark + wordmark for Invisalign, iTero, Overjet, Primescan, Smilecloud) built inline — swap
  for the real brand logos (with permission) in production.
- All photography is placeholder — supply real practice/team/case photos.

## Files
- `Smiles Northwest Homepages.dc.html` — the source containing all four concepts (sections `2a`, `1a`, `1b`, `1c`).
- `image-slot.js` — the photo-placeholder web component.
- `assets/` — logo colourways.
