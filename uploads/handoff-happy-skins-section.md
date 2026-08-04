# "Happy skins / wall of grins" section — port spec

Paste or attach this in the other chat.

---

## What it is
A full-bleed section of client photos laid out as **polaroids on two auto-scrolling rows** moving in opposite directions. Hovering pauses the row; clicking a photo opens it in the site's lightbox.

## Copy
- Eyebrow (12px, uppercase, letter-spacing 3px, muted): `happy skins`
- Heading (Gaegu, clamp(40px,6vw,68px)): `the wall of grins`
- Sub (16px, max-width 44ch, `text-wrap:pretty`): `every piece leaves with a person. these are my people — freshly inked, mid-flex, very pleased with themselves. hover to slow it down, tap to see big ♡`

Header block is centred and max-width 1180px; the rows themselves are full-bleed, so the section is `overflow:hidden` with `padding: clamp(30px,5vw,60px) 0 clamp(46px,7vw,90px)` (no horizontal padding).

## The two rows
Wrapper: `display:grid; gap:18px`.

Each row: `display:flex; width:max-content; gap:18px; padding:6px 9px` (the padding keeps the rotated cards' shadows from clipping) with

- row 1: `animation: marquee 52s linear infinite`
- row 2: `animation: marquee 46s linear infinite reverse`

Both get `style-hover="animation-play-state:paused"` so hovering slows the wall to a stop.

```css
@keyframes marquee { from { transform: translateX(0) } to { transform: translateX(-50%) } }
```

**Seamless loop rule:** each row renders its slice of photos **twice, concatenated** (`arr.concat(arr)`) so `-50%` lands exactly on the duplicate. Row 1 gets photos 0–6, row 2 gets 7–12 — no photo appears in both rows.

## The polaroid card
- `flex:0 0 auto; width:236px; padding:11px 11px 8px; border-radius:6px; background:#fff` — deliberately *low* radius and asymmetric padding (fatter at the bottom) so it reads as film, not as a web card.
- Shadow `0 14px 30px rgba(70,140,205,.2)`, on hover `0 26px 48px rgba(70,140,205,.32)`.
- Each card gets a small baked-in tilt from a repeating array, e.g. row 1 `[-2.5, 1.8, -1.2, 2.6, -3, 1.2, -1.8]`, row 2 `[2.2, -2.8, 1.4, -1.6, 2.8, -1.1]` (degrees) — applied as `transform: rotate(Ndeg)`.
- Hover: `transform: rotate(0deg) translateY(-10px) scale(1.04)` over `.45s cubic-bezier(.2,.8,.3,1)` — the card straightens itself and lifts, which is the whole charm.
- Photo well: `aspect-ratio:4/5; overflow:hidden; border-radius:3px`, pale tint background, `img` with `object-fit:cover` and `loading="lazy"`.
- Caption under the photo: Gaegu 21px, centred, accent colour — handwritten-on-the-white-strip feel. Keep captions short and human: `first ever ✿`, `matching set`, `healed & happy`, `dad's flowers`, `mid-flex`.
- `cursor:zoom-in`, click opens the lightbox with the caption as the title.

## Performance (important — this bit bit me)
Camera-original photos are 3–8 MB each and 13 of them will hang the page. Generate two sizes:
- **thumbs** ~620px wide, JPEG q0.82 (~60 KB) → used in the wall
- **large** ≤1400px wide, JPEG q0.86 (~300 KB) → used only in the lightbox

Card data shape: `{ id, thumb, src /* large */, cap }`.

Also guard the `<img>` inside the repeat with an `sc-if` on the src, so the streaming placeholder pass doesn't fire a request for an unresolved URL.

## Fonts / palette
Gaegu for headings + captions, Quicksand for body. Reuse the host project's own accent colours instead of these blues — the structure is the part worth copying.
