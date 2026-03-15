# ACE Photo Studio v1.1.0

ACE Photo Studio is a macOS Electron desktop app for DJI DNG bracket workflows:

1. Import single photos/folders or HDR RAW folders.
2. Detect 3-shot or 5-shot bracket groups.
3. Merge each valid bracket set into a 16-bit TIFF master.
4. Load merged TIFFs into the editor.
5. Apply cleanup adjustments/presets.
6. Export final JPEGs.

![ACE Photo Studio Main v1.1.0](assets/Ace%20Photo%20Studio%20Main%20v1.1.0.png)

Current focus areas:
- merge correctness and workflow safety
- practical editor workflow quality
- ongoing Lightroom-like pro-app polish direction in the UI

This README is implementation-aligned with the current codebase (not aspirational behavior).

## Install on macOS

1. Choose the correct DMG for your Mac:
   - Apple Silicon (M1/M2/M3): `arm64`
   - Intel Mac: `x64`
2. Open the DMG and drag **ACE Photo Studio** into **Applications**.
3. Open **Applications** and launch **ACE Photo Studio**.

### If macOS says the app cannot be verified

1. Try opening the app once from **Applications**.
2. Go to **System Settings > Privacy & Security**.
3. Scroll to the security message for ACE Photo Studio and click **Open Anyway**.
4. Confirm the follow-up prompt to open the app.
5. Optional: right-click the app in **Applications** and choose **Open**.

## Current Feature Set

- Top-bar workflow:
  - **Import** menu (`Add Photos...`, `Add Folder...`, `Add HDR Folder...`)
  - single **Export** button that opens **Export Settings**
- Photo library import (buttons + drag/drop)
- DJI DNG pipeline checks before RAW-heavy workflows
- Batch HDR merge queue with:
  - bracket detection
  - queue progress/error reporting
  - cancel (safe stop after current write)
  - retry failed sets only
- Merged HDR master validation:
  - merged outputs validated as 16-bit TIFF
- Editor controls:
  - library + preview + adjustments workflow
  - split/slider compare
  - fit/zoom/rotate controls in preview toolbar
  - top-row adaptive presets (`Natural`, `Real Estate`, `Punchy`, `Soft`)
  - `PICK PRESET` dropdown with user preset save/load/delete
  - auto fix workflow
  - luminance histogram panel
  - luminance-only **Tone Curve** (point curve v1, reset, histogram backdrop)
  - per-photo adjustments
- Export (modal-based normal flow):
  - **Export Settings** scopes: `Current Preview`, `Current Selection`, `All Loaded Photos`
  - output-folder-first export flow (normal path)
  - `Current Preview` uses folder-based export in normal flow (no per-file Save dialog when output folder is set)
  - ACE-styled **Export Complete** result modal
  - strict-named merged-HDR JPEG export via **Batch HDR Export**
- HDR queue completion:
  - ACE-styled **HDR Merge Complete** result modal
- Recent usability polish:
  - cleaner Batch HDR area/queue readability
  - right Adjustments panel scroll behavior for longer control stacks
  - 90/270 rotation path keeps geometry stable
- Validation coverage:
  - merge isolation test
  - bracket grouping test
  - sample HDR workflow validation
  - editor regression test suite (`tests/validate-editor-regressions.js`)

## Lens Correction Status (Current)

- For DJI Mavic 3 DNGs, manual/profile lens correction is active in both contexts when camera detection matches:
  - `hdr-merge-source` (batch/manual HDR merge inputs)
  - `single-import` (normal imported RAW photos)
- Safe disable override is available:
  - `ACE_DISABLE_DJI_M3_LENS_CORRECTION_PREMERGE=1`
- Embedded-opcode correction is probed, but is not the active production path in the current helper stack.
- Current production path is DJI Mavic 3 manual/profile correction with neutral fallback when not applicable.

## Architecture At A Glance

- Renderer/editor responsibilities are partially split:
  - `renderer.js`: UI orchestration/wiring and editor flow integration
  - `preview-pipeline.js`: preview processing path helpers
  - `histogram.js`: histogram rendering helpers
  - `auto-fix.js`: Auto Fix estimation helpers
  - `renderer-state.js`: renderer state creation/aliasing helpers
  - `editor-regression-core.js`: shared editor logic used by regression checks
- `preload.js`: secure bridge exposing explicit `aceApi` IPC methods/events
- Main process responsibilities are beginning to split:
  - `main.js`: app bootstrap, queue orchestration, normalization, menu/wiring
  - `presets-ipc.js`: cleanup preset IPC handlers
  - `exports-ipc.js`: export-related IPC handlers
- `raw-service.js`: RAW -> TIFF conversion and preview JPEG generation
- `hdr-service.js`: per-set merge orchestration and worker launch
- `merge-worker.js`: alignment/fusion/HDR helper execution, set-isolation checks, final TIFF validation
- `bracket-detector.js` + `file-grouping.js`: bracket grouping/partitioning/validation

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for flow-level detail.

## Primary Workflows

### 1) Standard Photo Cleanup

1. Add photos/folder.
2. Main process normalizes inputs (RAW files become preview-backed working assets).
3. Edit in renderer preview.
4. Use top-bar **Export** -> **Export Settings** (`Current Preview` / `Current Selection` / `All Loaded Photos`) -> export JPEGs.

### 2) Batch HDR Merge (DJI DNG)

1. Add HDR folder.
2. Detect bracket groups.
3. Start merge queue.
4. Merge outputs are validated as 16-bit TIFF masters.
5. Completed merged TIFFs are loaded into library with HDR metadata.
6. Review ACE-styled HDR merge completion modal, then edit/export as JPEG.

## Setup / Run / Test

```bash
npm install
npm start
```

Validation scripts:

```bash
npm run test:editor-regressions
npm run test:bracket-grouping
npm run test:merge-isolation
npm run test:hdr-samples
```

Packaging / helper checks:

```bash
npm run stage:helpers
npm run verify:helpers
npm run pack
npm run dist
```

## Documentation Map

- [docs/USER_QUICK_START.md](docs/USER_QUICK_START.md)
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)
- [docs/IPC_CONTRACT.md](docs/IPC_CONTRACT.md)
- [docs/PRESETS_AND_AUTOFIX.md](docs/PRESETS_AND_AUTOFIX.md)
- [docs/OUTPUT_NAMING_AND_FILE_RULES.md](docs/OUTPUT_NAMING_AND_FILE_RULES.md)
- [docs/RELEASE_GATE_CHECKLIST.md](docs/RELEASE_GATE_CHECKLIST.md)
- [docs/QA_BASELINE_2026-03-13.md](docs/QA_BASELINE_2026-03-13.md)

## Current Limitations / Known Status

- Some interactive release-gate behaviors still rely on manual human verification (compare interaction, visual parity checks, full UI sweep).
- Embedded DNG opcode correction is not the reliable active production path in the current helper stack.
- DJI Mavic 3 manual/profile correction is the active path for both merge-source and single-import contexts.
- Preview quality/performance tuning is ongoing and should continue to be validated with `hdr-set-2` and `hdr-set-5`.
