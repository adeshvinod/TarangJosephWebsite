# Tarang Joseph — Official Website

A single-page, long-scroll artist site for Tarang Joseph. Static HTML — no build step, no framework, no server-side code. Drop it on any static host and it works.

---

## What's in this folder

```
design_handoff_tarang_website/
├── index.html          ← The entire site (HTML + CSS + vanilla JS, ~75KB)
├── assets/             ← 13 images (logo, photos, album covers)
│   ├── logo-cropped.png
│   ├── tarang-stage.jpg
│   ├── tarang-portrait.jpg
│   ├── irresistible.png
│   ├── one-track-mind.jpg
│   ├── liquid-sunshine.jpeg
│   ├── take-me-away.jpeg
│   ├── make-it-right.jpeg
│   ├── superhuman.jpeg
│   ├── live-echoes.png
│   ├── live-bira.jpg
│   ├── band-1.jpg
│   └── band-2.jpg
└── README.md           ← This file
```

The HTML file is self-contained — all CSS is in a `<style>` block, all JS is inline, and the only external requests are to Google Fonts and YouTube/Spotify embeds.

---

## Preview locally

Just double-click `index.html` — it'll open in your browser. No build, no server.

For a more accurate local preview (some browsers restrict file:// origin features), run a quick local server:

```bash
# Python 3 (already installed on macOS/Linux)
cd design_handoff_tarang_website
python3 -m http.server 8000
# Then open http://localhost:8000

# Or with Node
npx serve .
```

---

## Deploy to a real host

Pick one. All three are free for a personal site.

### Option 1 — Netlify (drag-and-drop, easiest)
1. Go to https://app.netlify.com/drop
2. Drag the entire `design_handoff_tarang_website/` folder into the browser
3. Netlify gives you a `*.netlify.app` URL instantly
4. (Optional) Buy a domain like `tarangjoseph.com`, then in Netlify → Domain settings → Add custom domain

### Option 2 — Vercel CLI
```bash
npm i -g vercel
cd design_handoff_tarang_website
vercel
# Follow the prompts. First deploy gives you a preview URL,
# `vercel --prod` deploys to production.
```

### Option 3 — GitHub Pages
1. Create a new repo on GitHub, e.g. `tarangjoseph/tarangjoseph.github.io`
2. Push the contents of `design_handoff_tarang_website/` to the repo root
3. Repo → Settings → Pages → Source: "Deploy from branch", branch: `main`, folder: `/ (root)`
4. Site goes live at `https://tarangjoseph.github.io`

---

## Custom domain (recommended)

Once deployed, point a real domain at it:

1. Buy `tarangjoseph.com` (Namecheap, Cloudflare, Porkbun — ~$10/yr)
2. In your host's dashboard (Netlify/Vercel), add the custom domain
3. Copy the DNS records they show you and paste them into your domain registrar
4. SSL is automatic and free on all three hosts above

---

## Editing the site

The whole site is in `index.html`. Open it in any code editor (VS Code, Cursor, etc.) and search for what you want to change.

### Common edits — search for these strings:

| What you want to change | Search for | Notes |
|---|---|---|
| New single name / date | `Irresistible` | Hero pill, marquee, release section |
| Discography (album list) | `const releases =` | Edit the array; add/remove tracks, change covers |
| Tour dates / venues | `const shows =` | Edit the array |
| Band members | `class="band-member"` | Edit each card directly |
| Bio paragraphs | `class="bio-text"` | Plain HTML `<p>` tags |
| Press quotes | `class="quote-card"` | Edit the three article blocks |
| Streaming links | `SPOTIFY = '` and `APPLE = '` | Top of the `<script>` block |
| Contact email / phone | `tarangjosephmusic@gmail.com` | Bookings section + footer |
| Roland endorsement video | `KMQnH0qWdSc` | YouTube video ID in the iframe |

### Swap an album cover
Replace the file in `assets/` (keep the same filename) — the site picks it up automatically.

### Add a new release
In the `const releases = [...]` array, add an object like:
```js
{ title: 'New Track', year: '2026', type: 'Single', cover: 'assets/new-track.jpg',
  spotify: SPOTIFY, apple: APPLE, youtube: YT },
```
…and drop `new-track.jpg` into `assets/`.

---

## Embeds (Spotify, YouTube)

These iframes load live content from the platforms — no setup needed.

- **Spotify artist player** (Music section) — links to `artist/76Qn1AJEps9AcUgwIOHplM`
- **YouTube** — three embedded videos (Superhuman MV, Liquid Sunshine @ BIRA, One Track Mind @ Pondicherry) plus the Roland Byte Size Jams short

If the artist ID or any video ID changes, search for `76Qn1AJEps9AcUgwIOHplM` or the YouTube ID and update.

---

## Tech notes

- **No framework, no build step.** Single HTML file with inline `<style>` and `<script>`. ~75KB before fonts/images.
- **Fonts:** Bowlby One (display), Anton (alt display), Pinyon Script (script accents), Inter (body), IBM Plex Mono (mono). All from Google Fonts.
- **Logo rendering:** the Tarang Joseph wordmark is a CSS `mask-image` of `logo-cropped.png`, color-filled by `background-color`. This lets the logo recolor itself per palette without needing multiple PNG variants.
- **Responsive:** breakpoints at 1000px and 600px. Mobile nav docks to the viewport bottom (thumb-reach).
- **Performance:** All YouTube iframes use `loading="lazy"`. Images can be further optimized — see "Performance polish" below.
- **Browser support:** any modern browser (Chrome, Safari, Firefox, Edge — last 2 versions). Uses CSS `color-mix()`, `mask-image`, `aspect-ratio`, and `backdrop-filter` which all have wide modern support but won't work in IE11 or ancient Safari.

---

## Performance polish (optional, when you have time)

The site is already fast (single file, no JS framework), but the photo assets are large. To squeeze more:

1. **Convert JPEG/PNG to WebP** — typically 30–50% smaller at the same visual quality.
   ```bash
   # Install cwebp (one-time)
   brew install webp           # macOS
   # or apt install webp       # Linux
   
   # Convert
   cd assets
   for f in *.jpg *.jpeg *.png; do
     cwebp -q 82 "$f" -o "${f%.*}.webp"
   done
   ```
   Then update `index.html` to point at `.webp` versions.

2. **Add `<picture>` fallbacks** for browsers that don't support WebP (rare in 2026, but safe):
   ```html
   <picture>
     <source srcset="assets/tarang-stage.webp" type="image/webp">
     <img src="assets/tarang-stage.jpg" alt="…">
   </picture>
   ```

3. **Add `loading="lazy"` to images below the fold** (most are not lazy right now). Add to every `<img>` except the hero ones.

4. **Resize huge images.** `logo-cropped.png` is 1600px wide — fine. But `tarang-stage.jpg`, `band-1.jpg`, `live-echoes.png` may be larger than they need to be at display size. Resize to ~1600px max dimension.

5. **Defer Google Fonts.** Currently it's a blocking `<link>`. For best LCP:
   ```html
   <link rel="stylesheet" href="https://fonts.googleapis.com/css2?…" media="print" onload="this.media='all'">
   ```

---

## Handing off to Claude Code

If you want to make further changes via Claude Code in your CLI:

1. `cd` into this folder
2. Run `claude` (or `claude-code`)
3. Tell it what to change — for example:
   > "Add a 'Newsletter signup' section between Press and Bookings. Single email input, big CTA button, match the existing pill style. Form should `mailto:` me for now."

Claude Code will edit `index.html` in place. Commit to git as you go.

---

## Domain + email (full pro setup)

When you're ready to look fully legit:

1. **Domain:** `tarangjoseph.com` — buy from Cloudflare Registrar (cheapest renewal pricing, no upsells).
2. **Email:** `hello@tarangjoseph.com` — use Google Workspace ($6/month) or Zoho Mail (free for 1 user). Replace `tarangjosephmusic@gmail.com` in the site.
3. **Analytics:** Plausible.io or Cloudflare Web Analytics — both privacy-friendly, no cookie banner needed.

---

## Questions / changes

The whole site lives in one HTML file by design — easier to grep, edit, and version-control than a sprawling framework setup. If you outgrow it later (need a CMS, a mailing list, a tour-dates database), the natural next step is moving to Astro or Next.js. Until then, this is genuinely the right shape.
