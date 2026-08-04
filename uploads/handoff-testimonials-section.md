# "Kind words" testimonials section — port spec

Paste or attach this in the other chat, then use the prompt at the bottom.

---

## Principle
Reviews are rendered as **real text**, never as the Google screenshots. Screenshots don't reflow, can't be searched, look blurry on retina, and lock the styling to Google's. Transcribe each review into a data array instead.

## Layout
Section id `words`, max-width 1180px, `padding: clamp(30px,5vw,60px) 20px clamp(46px,7vw,90px)`, `scroll-margin-top:80px`.

### Header (centred)
1. Eyebrow — 12px, uppercase, letter-spacing 3px, muted (`#93B2CA`), weight 700: `kind words`
2. Heading — Gaegu, `clamp(40px,6vw,68px)`, `#2F4A63`: `what my people say`
3. **Rating badge** — an inline-flex pill: white bg, 1px border `#D9EBFA`, `border-radius:999px`, `padding:12px 22px`, shadow `0 10px 26px rgba(70,140,205,.14)`. Left side: `5.0` in Gaegu 44px accent blue. Right side stacked: gold `★★★★★` (`#F5B841`, 17px, letter-spacing 2px) over a 12px uppercase caption `google reviews · every single one`.

### Language filter chips
Same pill treatment as the gallery filters: `all reviews / english / 한국어 / deutsch`. Active = gradient fill `linear-gradient(135deg,#6FB0E4,#E9A6CE)` + white text + glow; inactive = white with `#D4E9F8` border and `#3F7EB0` text. Hover `translateY(-3px) scale(1.05)`. Clicking fires the sparkle burst.

Filtering is a simple `lang` key match on each review (`'en' | 'ko' | 'de'`).

### The grid — CSS columns, not flex/grid
```css
columns: 3 320px;
column-gap: 22px;
```
with each card `break-inside: avoid; margin-bottom: 22px`. This gives true masonry so wildly different review lengths pack tightly with no gaps. (A CSS grid would leave ragged holes.)

### Review card
- `padding:24px 24px 20px; border-radius:26px; background:#fff; border:1px solid #E2EFFA; box-shadow:0 12px 30px rgba(70,140,205,.13)`
- Hover: `translateY(-8px) rotate(-.5deg)` + `0 26px 48px rgba(70,140,205,.26)`, `.45s cubic-bezier(.2,.8,.3,1)`
- **Header row** (flex, gap 12px): a 46px circle avatar showing the reviewer's first initial in Gaegu 24px white on that person's **own gradient** (each review carries its own `avBg` — brown, purple, teal, orange etc., loosely matching their real Google avatar so the wall feels like different people); then name in Gaegu 24px, and beneath it gold `★★★★★` at 13px + an 11px uppercase language tag (`english`, `한국어`, `deutsch`).
- **Quote mark**: a Gaegu `“` at 40px, `line-height:.6`, pale accent (`#C3E2F7`), `padding-top:16px` — decorative, sits above the text.
- **Body**: 14.5px, `line-height:1.72`, muted (`#567A94`), `text-wrap:pretty`.

### Read more (this is the important bit)
Two reviews are 1000+ characters and would otherwise dominate the whole grid. Truncate **in the logic, not with CSS clamp**:

```js
const LIM = 260;
const hasMore = r.text.length > LIM;
text: (!hasMore || open) ? r.text : r.text.slice(0, LIM).replace(/[\s,.]+$/, '') + '…'
```
Toggle button only renders when `hasMore`; label flips `read more ↓` / `show less ↑`. Style it as a small soft chip: `background:#EDF6FD; border:1px solid #D4E9F8; color:#3F7EB0; border-radius:999px; padding:8px 16px; font-size:13px; weight 700`. Expanded state is per-review in an `open: {}` state object.

Truncating in JS (rather than `-webkit-line-clamp`) keeps the masonry columns from reflowing weirdly and works with any font size.

### Closing CTA
Centred gradient pill button linking to the booking section: `be the next kind word ♡`.

## Data shape
```js
{ id, name, lang: 'en'|'ko'|'de', flag: 'english', avBg: 'linear-gradient(...)', text: '…full review…' }
```
Transcribe reviews verbatim, keeping their emoji and their little quirks (`ㅠㅠ`, `haha`, `정말 감사합니다^^`) — the imperfection is what makes them read as real.

## Also add
A nav link to `#words` labelled `kind words`, placed between the gallery and the quiz links.

## Gotcha
If images end up inside a repeated list in this section, give them `loading="lazy"` — otherwise the streaming placeholder pass fires a request for the literal unresolved template hole and the console fills with 404s.

---

## What to say in the other chat

> Add a "kind words" testimonials section using this spec — real transcribed text (not screenshots), masonry CSS columns, per-person gradient initial avatars, language filter chips, and JS-truncated long reviews with a read more toggle. Reuse my project's existing palette, fonts and copy voice instead of the colors in the spec, and add a nav link to it. Don't change anything else. I'll paste the review text next.

Then paste the reviews themselves (or attach the Google screenshots and ask it to transcribe them).
