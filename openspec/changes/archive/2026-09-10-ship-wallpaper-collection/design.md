## Context

The zip at `~/Library/Mobile Documents/com~apple~CloudDocs/celadon-wallpapers.zip` is a complete, self-describing package: images in three renditions, prompts, manifest, checksums, palette snapshot, and a draft README. The repo holds only `LICENSE`, `HANDOFF.md`, and `openspec/config.yaml`. Native 4K regeneration is unavailable on the current image-tool subscription and API spend is declined, so the package ships as it is.

## Goals / Non-Goals

**Goals:**
- Publish the collection with an honest README and intact provenance.
- Keep the zip's file names and directory shape so `manifest.json` and `SHA256SUMS.txt` stay valid without edits.

**Non-Goals:**
- Regenerating any image, at 4K or otherwise.
- Porting the private Python generators or any reproducible build.
- Deriving image colours from the palette feed; palette hexes were prompt targets only.

## Decisions

- **Ship upscaled 4K JPEGs alongside originals.** Alternative: ship only the 1672×941 originals and let the OS scale. Rejected: most users want a file that is already 3840×2160, and the README disclosure keeps it honest.
- **Keep the zip's layout verbatim.** Alternative: flatten to the handoff's earlier `celadon-*.png` naming. Rejected: renaming would invalidate the manifest and checksums for no user benefit.
- **Commit binaries, no Releases.** Same precedent as `rose-pine/wallpapers`; 24 MB is small.
- **Adapt the zip README rather than rewrite.** It already carries the disclosure, table, and provenance. Edits: repo-facing title, license line, tighten wording to match hub port READMEs.
- **Palette snapshot stays in `palettes/`.** Provenance, not a feed. Not refreshed when the hub moves.

## Risks / Trade-offs

- [Upscaled 4K looks soft on close inspection] → README says so up front; originals are committed for anyone who wants to re-upscale.
- [Palette drift after `8a5090c`] → images are pinned to that revision in the manifest; no attempt to track.
- [Binary history is permanent] → accepted; the set is small and unlikely to churn.
