## Why

Celadon has no public wallpapers. Eight AI-generated scenes (two per theme variant) exist in a packaged zip with prompts, manifest, and checksums, but nothing is published. The earlier plan to ship script-generated Voronoi and bauhaus sets was dropped on 2026-09-10 because the output was not desktop-worthy.

## What Changes

- Add the eight-image collection as committed files: `originals/` (1672×941 PNG, canonical), `desktop-4k/` (3840×2160 JPEG, Lanczos upscale), `previews/` (thumbnails), `contact-sheet.jpg`.
- Add provenance: `PROMPTS.md`, `manifest.json`, `SHA256SUMS.txt`, and `palettes/` (hub `ports/json` snapshot at `8a5090c`).
- Write a public `README.md`: contact sheet hero, per-wallpaper download table, plain statement that 4K files are upscales, palette provenance, generation method, license.
- Remove `HANDOFF.md` (decision brief, superseded by this change).
- **Not** ported: the Python generators in private `celadon-docs`.

## Capabilities

### New Capabilities
- `wallpaper-collection`: the published set of wallpaper files, their layout, naming, integrity record, and the README that documents them.

### Modified Capabilities
<!-- none — first capability in this repo -->

## Impact

- Repo grows by ~24 MB of binaries, committed permanently (mirrors `rose-pine/wallpapers`).
- No code, no dependencies, no build step. Nothing consumes the palette feed at runtime.
- Hub (`celadon-theme/celadon-theme`) is unaffected; it may later link here from its README.
