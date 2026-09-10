## 1. Import the collection

- [x] 1.1 Copy `originals/`, `desktop-4k/`, `previews/`, `palettes/`, `contact-sheet.jpg`, `PROMPTS.md`, `manifest.json`, `SHA256SUMS.txt` from the unpacked zip into the repo root, layout unchanged
- [x] 1.2 Verify `shasum -a 256 -c SHA256SUMS.txt` passes and every `desktop-4k/` file is 3840×2160

## 2. README

- [x] 2.1 Adapt the zip's `README.md` into the repo README: title, contact sheet, upscale disclosure before the table, download table, palette provenance at `8a5090c`, generation method, MIT license line
- [x] 2.2 Check every relative link in the README resolves to a committed file

## 3. Cleanup

- [x] 3.1 Delete `HANDOFF.md`
- [x] 3.2 Commit on `feature/init`; open PR to `main`
