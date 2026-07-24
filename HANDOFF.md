# Handoff — Celadon Wallpapers

Decision brief from the exploration that created this repo. Feed this to
`/opsx:propose` here; delete it once the proposal exists (it's captured
thinking, not permanent repo content).

## Goal

A public home for Celadon's **generative desktop wallpapers** — two sets, four
variants each, **generated, not painted** (same creed as the palette: a small
set of parameters + rules, colours taken from the generated palette). Today the
generators and preview images live only in the **private** `celadon-docs`; this
repo is where they ship in public. The docs README says so outright: *"ship them
from a public wallpapers repo when the palette locks."* The palette **locked
2026-07-16** — so it's time.

## Decision locked in exploration

- **Own repo** (this one, `celadon-theme/wallpapers`), *not* a folder in the
  hub's `ports/`. Wallpapers are large binaries and a wallpaper is not a
  curl-a-file port, so it doesn't fit the hub's "flat files you copy" model — it
  earns its own repo, exactly like `celadon-theme/google-chrome`.

## The two sets

- **`celadon-glaze-*` — crackle glaze (the good one).** A Ru/Ge ice-crackle
  Voronoi field: the brand's locked craquelure motif full-bleed. The field stays
  a calm uniform glaze and the **fissures carry the accents** (Ge ware's "golden
  thread and iron wire"). The palette's own thesis as an image — low-chroma
  field, accents carry the work.
- **`celadon-*` — paper-cut geometric.** Opaque bauhaus shapes on the variant's
  true terminal background, soft layered drop-shadows, matte film grain. Focal
  cluster on a rule-of-thirds anchor, size hierarchy, negative space (leaves room
  for desktop icons).

## Variants

| suffix     | theme            | field                     |
|------------|------------------|---------------------------|
| `*-sky`    | `celadon sky`    | light, sage paper         |
| `*-powder` | `celadon powder` | dark, low contrast        |
| `*-celadon`| `celadon`        | dark, medium — the default|
| `*-jade`   | `celadon jade`   | dark, high contrast       |

2 sets × 4 variants = **8 images**.

## Where the source lives now

`~/dev/celadon/celadon-docs/assets/wallpapers/` (**PRIVATE**):

- `celadon_glaze.py` — the crackle-glaze generator
- `celadon_wallpapers.py` — the paper-cut generator
- `README.md` — the prose (planning-grade; adaptable but **has errors**, below)
- 8 committed `*.jpg` — 3840×2160, q92, ~2.5 MB each

**Copy the two `.py` here and regenerate images here.** The generators need only
`numpy` + `pillow` (no scipy; the Voronoi is hand-rolled and tiled). Both are
deterministic — fixed seeds, so a given variant reproduces exactly.

```sh
python3 celadon_glaze.py            # all four glaze
python3 celadon_glaze.py celadon jade
python3 celadon_wallpapers.py       # the paper-cut set
```

The docs `README.md` prose about the *sets* is fine to adapt for a public README
(brand/logo motifs are public-bound). Do **not** copy palette-decision rationale
or anything planning-grade — `celadon-docs` is private.

## Must fix before shipping — do NOT port as-is

1. **Sky is still pre-lock.** Every non-sky variant's field/bg hex matches
   `ports/json` exactly; sky doesn't:
   - glaze `sky.field` = `#dcead6` → should be locked **surface `#deedda`**
     (`celadon_glaze.py:28`)
   - paper-cut `sky.bg` = `#eef6ea` → should be locked **base `#eaf6e8`**
     (`celadon_wallpapers.py:29`)
   - **Leave** the paper-cut sky *accents* — that curated tonal-sage set is a
     deliberate "calm on paper" choice, not ANSI, not a bug.

   Fix = two hex edits + rerun. Seeds are fixed, so composition is unchanged.

2. **The format claim is false — the code emits JPG, not PNG.** Both generators
   `save(..., quality=92)` to `*.jpg` (`celadon_glaze.py:178-180`,
   `celadon_wallpapers.py:213-215`). There is **no PNG output path** in either
   file. The committed JPGs *are* the generator output. Yet the docs README
   claims *"the generators emit full 3840×2160 PNGs"* and *"Full PNGs are
   ~10-13 MB … deliberately not committed."* That's fiction — do not repeat it.
   Pick the real format (decision B) and make the code + docs agree.

## Decisions the proposal must make

- **B — Ship format & resolution.** Generators emit 3840×2160 **q92 JPG**
  (~2.5 MB). A wallpaper is set full-screen on a flat glaze field, where JPG
  ringing shows. Recommendation: emit **PNG** for the shipped download and a
  small JPG/WebP preview for the README. Whatever you choose, change the
  `save()` calls so output matches the claim.
- **C — Commit binaries, or ship via Releases?** 8 images. As q92 JPG ≈ 20 MB
  total (livable); as full-res PNG ≈ 80–100 MB in git history (heavy, forever).
  Options: (a) commit compressed previews + full-res via GitHub **Release
  assets**; (b) commit everything; (c) commit nothing, regenerate on demand +
  Releases. Recommend (a). If a `rose-pine/rose-pine-wallpapers`-style reference
  exists, mirror its model; if not, decide fresh — don't assume one.
- **D — Sky field fix (issue 1)** is mandatory regardless of B/C.

## Palette feed — the generated source of truth

Same feed every Celadon port consumes: the hub's `ports/json`, `role → hex`, one
file per variant, at a pinned ref:

- `https://raw.githubusercontent.com/celadon-theme/celadon-theme/main/ports/json/celadon.json`
- …`/celadon-powder.json`, …`/celadon-jade.json`, …`/celadon-sky.json`

Roles: `base, surface, overlay, muted, subtle, text, red, green, yellow, blue,
magenta, cyan, br_red, br_green, br_yellow, br_blue, br_magenta, br_cyan`.

**Important nuance vs `google-chrome`:** these generators are **not** a pure
`hex → rgb` of the feed. On the dark variants, `field`/`bg` and `accents` come
straight from the palette — but `crack`, `wash`, `sat`, and `seed` are
**hand-tuned compositional parameters**, and sky's paper-cut accents are
deliberately off-feed. Do **not** "purify" these into mechanical feed-consumption
the way the Chrome port does. The palette governs the colours that carry meaning
(field + accents on the dark variants); the rest is composition. When the feed
changes, re-derive the dark field/accents and leave the compositional params
alone. Where the two disagree today, the feed wins (that's issue 1).

## Suggested repo shape

```
celadon_glaze.py          # crackle-glaze generator
celadon_wallpapers.py     # paper-cut generator
Makefile                  # optional: `make` regenerates all (numpy+pillow)
previews/                 # small committed preview images for the README
README.md                 # the two sets, variants table, install, regen, license
LICENSE                   # MIT — already present
# full-res downloads via GitHub Release assets (decision C)
```

## Next step

Run `/opsx:propose` in this repo using this brief. Then implement: copy the two
generators, **fix sky to the locked hexes**, settle format + distribution
(B/C), regenerate, write an honest README. Keep the creed — the colours that
matter trace to the generated palette; nothing meaningful is eyedropped.
