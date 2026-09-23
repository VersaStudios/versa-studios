# Show logos

Drop logo files in here and they replace the show name on that job's card.

## Expected filenames

| Show          | File                        | Supplied |
|---------------|-----------------------------|----------|
| Morning Live  | `logos/morning-live.png`    | yes      |
| ShopOn TV     | `logos/shopon.png`          | yes      |
| Bullseye      | `logos/bullseye.png`        | yes      |
| Mock the Week | `logos/mock-the-week.png`   | **no**   |

Mock the Week's colours are set up but the file has not been added, so that
card shows the show name until it is.

`.svg` works too — change the extension in the `_VS_SHOWS` map in `index.html`
(search for `_VS_SHOWS`) if you use SVG or a different filename.

## What works best

- **Transparent background** (PNG-24 or SVG). A white box around the logo will
  show as a white box on the coloured card.
- **Dark artwork.** Cards are light pastel blocks with near-black text, so a
  dark logo sits correctly. If you only have a white version, set
  `invert:true` against that show in `_VS_SHOWS` and it will be flipped.
- **Roughly landscape**, up to about 4:1. Very tall logos get scaled down to
  fit the card and will look small.
- **At least 160px tall** so it stays sharp on a retina screen. Bigger is fine.

## Adding another show

Add a line to `_VS_SHOWS` in `index.html`:

```js
var _VS_SHOWS = {
  'morninglive': {src:'logos/morning-live.png'},
  'shopon':      {src:'logos/shopon.png'},
  'bullseye':    {src:'logos/bullseye.png'},
};
```

The key is the show name lowercased with spaces and punctuation stripped.
A job matches if its name equals the key or starts with it — which is why
`shopon` catches "ShopOn TV".

If a file is missing or fails to load, the card falls back to the show name
in text, so nothing breaks.


## Card colours

A show in `_VS_SHOWS` also paints its card with its own brand colour instead
of the usual status colour. Every other job keeps status colouring — sage for
Confirmed, mauve for Ongoing, and so on.

Each entry carries four colours, all sampled from the logo artwork:

| Field      | What it is                                          |
|------------|-----------------------------------------------------|
| `tint`     | pale version, used by default in the light theme    |
| `tintDark` | slightly deepened, used in the dark theme           |
| `full`     | the real brand colour, used when `?brand=full`      |
| `ink`      | text colour needed at full strength (black or white)|

Switch between them with `?brand=full` and `?brand=tint`. The choice sticks
in local storage, so you only have to say it once.

**Caveat at full strength:** a logo drawn for a white background — ShopOn and
Bullseye both are — loses definition on its own saturated brand colour. The
pale tints avoid that. Morning Live has the opposite problem: the file is the
social avatar, white lettering on an orange square, so it reads as a small
orange tile on a pale card and only looks right at full strength.


## How each of the four is handled

They needed different treatment because the supplied artwork differs:

- **Morning Live** — the file was a social avatar: white lettering on a solid
  orange square. The orange was keyed out and the lettering recoloured to ink,
  leaving a transparent wordmark that reads on the pale tint and on full
  amber alike. Original kept as `orig-morning-live.png` in the session
  scratchpad.
- **ShopOn TV** — knocked out to solid white (`shopon-white.png`) and pinned
  to the purple from the middle of its own gradient, `#9B4695`. Marked
  `fixed:true`, so it ignores the tint/full switch and always sits on the
  purple. The full-colour original is still here as `shopon.png`.
- **Bullseye** — the wordmark and the bull are a single overlapping artwork
  and cannot be separated, so the show name is set in the app's own type with
  the roundel trimmed and placed after it (`markAfter:true`).
- **Mock the Week** — supplied as a globe on a solid white square. The white
  was flood-filled away from the edges, so the background went but the white
  lettering inside the globe survived. Like Bullseye it is a round badge, so
  it uses `markAfter:true`. Original kept in the session scratchpad.

**The rule that emerged:** a wide wordmark (Morning Live, ShopOn) replaces the
show name. A round badge (Bullseye, Mock the Week) sits *after* the name in
type, because at card size the lettering inside a circular mark is unreadable.

`markAfter:true` on any entry gives you the name-then-mark treatment instead
of the logo replacing the name.
