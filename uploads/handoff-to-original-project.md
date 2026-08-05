# Port these three things into the original Lucky tattoo site

Paste this whole file (or attach it) into the other chat.

---

## Fonts used here
- **Headings / handwritten bits:** `Gaegu` (Google Fonts, weights 400 & 700) — the round marker-pen look.
- **Body / UI / buttons:** `Quicksand` (400–700).

```html
<link href="https://fonts.googleapis.com/css2?family=Gaegu:wght@400;700&family=Quicksand:wght@400;500;600;700&display=swap" rel="stylesheet">
```

---

## 1. Filterable gallery with save-to-favourites + lightbox

Behaviour to reproduce:
- Pill filter row above the grid: `everything / fine line / watercolor / oriental brush / cute stuff / pets`. Active pill = lavender→pink gradient fill, white text, soft glow. Inactive = white with a light lavender border.
- Each artwork carries a `tags` array; clicking a pill filters the grid instantly (no page reload, no animation on the grid itself beyond the cards' own reveal).
- Cards: 4/5 aspect image, rounded ~28px, white frame, soft lavender shadow. On hover the card lifts and rotates about −0.7deg and the image scales to 1.07 over ~0.9s.
- Bottom of each card: title in Gaegu, tiny uppercase caption, and a round **heart button**. Clicking it toggles "saved": heart fills with a pink gradient and a burst of ~14 sparkle particles pops from the button. Saved count shows in the header pill.
- Clicking the image opens a **lightbox**: dim lavender backdrop with blur, image centred, title/caption underneath, ← → buttons, and keyboard `←` `→` `Esc`. Arrows cycle within the *currently filtered* set.

Data shape:
```js
{ id, src, title, cap, tags: ['fineline','watercolor','oriental','cute','pets'] }
```

## 2. "Find your ink" mini game

- Card panel with a soft lavender→pink→blue gradient, one blurred floating blob, centred heading in Gaegu.
- Thin progress bar that animates as you answer.
- 3 questions, each with 2–4 answer cards (big symbol, Gaegu label, small grey sub-line). Cards lift + rotate slightly on hover; clicking pops sparkles.
  1. *what mood are you in?* → soft & dreamy / quiet & inky / silly & sweet / delicate & precise
  2. *how big are we going?* → a tiny secret / palm-sized / a whole story
  3. *pick a colour feeling* → lavender whisper / watercolour bloom / black ink only / all the colours
- Every answer maps to a style key (`fineline`, `watercolor`, `oriental`, `cute`). Tally the three picks, most-picked key wins (ties → first pick).
- Result panel: matching photo in a white frame + style name + 2-sentence blurb, a "book this vibe" button and a "play again ↺" reset. Big sparkle burst fires when the result appears.

## 3. Infinite right-to-left ticker (the "billboard")

- Full-width strip, faint lavender→pink gradient background, hairline borders top and bottom, Gaegu ~26px lavender text.
- `overflow:hidden` outer div; inner `display:flex; width:max-content` containing the **same span repeated 4×** (identical content and identical `padding-right`), animated `@keyframes marquee { from { transform: translateX(0) } to { transform: translateX(-50%) } }`, `26s linear infinite`.
- The seamlessness depends on: all copies byte-identical, no stray `&nbsp;`, translate exactly −50% with an even number of copies. That's what makes it loop forever instead of snapping back.

Content: `fine line ✿ watercolor ✿ oriental brushstroke ✿ pet portraits ✿ cute characters ✿ tiny lettering ✿ busan, korea ✿`

---

## Shared sparkle system (used by 2 and 3's siblings)
One fixed, pointer-events-none layer appended to `<body>` on mount. A `spark(x, y, count, size, colors)` helper spawns little 8-point-star divs (`clip-path: polygon(50% 0,60% 40%,100% 50%,60% 60%,50% 100%,40% 60%,0 50%,40% 40%)`) and animates them outward/downward with the Web Animations API, removing each on finish. Palette: `#C9AEF6 #EFA3CD #A9CCEF #E8D9FF`. It also runs on `mousemove` (throttled to ~60ms, 1 particle) for the cursor trail.

Keep everything inline-styled and match the original site's existing spacing, palette and copy voice rather than pasting this page's colours wholesale.
