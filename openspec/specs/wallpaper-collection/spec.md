# wallpaper-collection Specification

## Purpose

The published set of Celadon wallpaper files, their layout, naming, integrity record, and the README that documents them.

## Requirements

### Requirement: Collection layout
The repository SHALL contain eight wallpapers, two per theme variant (`celadon-sky`, `celadon-powder`, `celadon`, `celadon-jade`), in three renditions: `originals/<NN>-<slug>-<variant>.png` (1672×941, untouched model output), `desktop-4k/<NN>-<slug>-<variant>-3840x2160.jpg` (3840×2160), and `previews/<NN>-<slug>-<variant>.jpg` (thumbnail).

#### Scenario: Every wallpaper has all three renditions
- **WHEN** `manifest.json` is read
- **THEN** each of its eight `images` entries names an `original`, `desktop`, and `preview` path, and every named file exists in the repository

#### Scenario: Desktop renditions are 4K
- **WHEN** any file in `desktop-4k/` is inspected
- **THEN** its pixel dimensions are exactly 3840×2160

### Requirement: Integrity record
The repository SHALL include `SHA256SUMS.txt` covering every file in `originals/`, `desktop-4k/`, and `previews/`, and every listed checksum MUST match the committed file.

#### Scenario: Checksums verify
- **WHEN** `shasum -a 256 -c SHA256SUMS.txt` is run from the repository root
- **THEN** every line reports `OK`

### Requirement: Provenance record
The repository SHALL include `PROMPTS.md` (one full generation prompt per wallpaper), `manifest.json` (per-image slug, title, variant, category, file paths, sizes, upscale flag, prompt, and the palette revision), and `palettes/<variant>.json` copies of the hub palette at the pinned revision.

#### Scenario: Manifest pins the palette revision
- **WHEN** `manifest.json` is read
- **THEN** `paletteRepository` is the hub URL and `paletteRevision` is a full commit SHA, and `palettes/` contains one JSON file per variant

### Requirement: README documents the collection honestly
The `README.md` SHALL show the contact sheet, list every wallpaper with its title, style, variant, and download links for both the 4K JPEG and the original PNG, state that the 4K files are upscaled from 1672×941 originals, link the palette source at its pinned revision, name the generation method, and state the license.

#### Scenario: Upscale disclosure
- **WHEN** a reader opens `README.md`
- **THEN** before any download link they are told the 4K files are resampled upscales that do not contain native 4K detail

#### Scenario: Every download link resolves
- **WHEN** each relative link in the README table is followed
- **THEN** it points at a committed file in `desktop-4k/` or `originals/`

### Requirement: No generator code
The repository SHALL NOT contain wallpaper generator scripts, a Makefile, or any build step; the committed images are the deliverable.

#### Scenario: Clone is the install
- **WHEN** the repository is cloned
- **THEN** the wallpapers are usable without running anything
