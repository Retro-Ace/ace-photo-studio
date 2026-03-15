# ACE Photo Studio v1.1.0 - Quick Start Guide

## 1) What ACE Photo Studio Is

ACE Photo Studio is a macOS photo editor focused on DJI DNG bracketed HDR workflows.  
You can import photos, merge bracket sets into 16-bit TIFF masters, edit, compare before/after, and export JPGs.

## 2) Getting Started

1. Launch **ACE Photo Studio** from Applications.
2. Use the **Import** menu in the top bar:
   - **Add Photos...**: import one or more image files.
   - **Add Folder...**: import all supported images from a folder.
   - **Add HDR Folder...**: load a bracketed RAW folder for HDR detection/merge workflow.
3. Imported items appear in the **Library** (left side). Click a photo to edit it.

## 3) HDR Workflow

1. Click **Add HDR Folder...** and choose your bracketed RAW folder.
2. ACE scans and detects complete bracket sets (3-shot / 5-shot based on your bracket setting).
3. In **Batch HDR Merge**, review:
   - detected sets
   - merge mode
   - bracket size
   - queue status
4. Click **Start HDR Merge**.
5. Each complete set is merged to a **16-bit TIFF master**.
6. Completed merged TIFFs are loaded into the Library so you can edit them.
7. Use:
   - **Cancel** to stop after the current safe write finishes
   - **Retry Failed** to retry only failed sets
   - **Open Output Folder** to jump to HDR outputs in Finder
8. When the queue finishes, an **HDR Merge Complete** result modal summarizes outputs and provides a quick Finder reveal action.

## 4) Preview and Compare

- The center panel shows your current image.
- In split compare mode:
  - left side = **Original** (or **Merged 16-bit TIFF Master** for merged HDR files)
  - right side = **Cleaned Preview** (your current edits)
- Click **Slider View** / **Split View** to switch compare style.
- Use:
  - **Fit** to fit image to panel
  - **+ / -** to zoom
  - trackpad/mouse wheel to zoom in preview
  - click-drag to pan when zoomed
  - **Rotate** to rotate by 90 degrees

## 5) Adjustments

- The **Adjustments** panel is where you edit exposure, contrast, highlights/shadows, color, detail, and more.
- **Auto Fix** analyzes the selected photo and applies an automatic correction starting point.
- Built-in preset buttons:
  - **Natural**
  - **Real Estate**
  - **Punchy**
  - **Soft**
- **PICK PRESET** opens saved preset options (built-in + your user presets).
- **Save Preset** stores your current adjustment values as a reusable preset.
- **Tone Curve** (Luminance) lets you shape tonal response with point controls over a histogram backdrop.
- **Reset Curve** returns the tone curve to a neutral straight line.
- Optional helpers:
  - **Reset**: return selected photo to neutral adjustments.
  - **Copy to All**: copy selected photo adjustments to all loaded photos.
  - **Apply edits to all photos**: apply ongoing adjustment changes across loaded photos.
- The **Luminance Histogram** updates to reflect the selected photo/edit state.

## 6) Export Workflow

1. Click **Export** in the top bar.
2. In **Export Settings**, choose scope:
   - **Current Preview**: exports the currently selected photo only.
   - **Current Selection**: exports selected Library items (or current photo if only one).
   - **All Loaded Photos**: exports everything currently loaded in Library.
3. Set **Output Folder**:
   - **Select Folder...** to choose a fixed folder
   - **Clear** to reset to "choose on each export"
   - **Reveal Folder** to open current export folder in Finder
4. Set **JPG Quality** (60-100; default 92).
5. Click **Export** to run.
6. When finished, **Export Complete** shows:
   - saved count
   - failed count (if any)
   - output folder
   - optional saved filename for single-file export

Notes:
- When an output folder is set, **Current Preview** export uses the same folder-based flow (no standard per-file Save dialog in the normal path).
- If no output folder is set, ACE asks you to choose an export folder during export.
- **Batch HDR Export** in the HDR panel exports loaded merged HDR photos with strict HDR naming.

## 7) Tips / Basic Troubleshooting

- **No export folder set?**  
  Open **Export**, then click **Select Folder...** in **Export Settings** (or export once and choose a folder when prompted).

- **macOS says app cannot be verified?**  
  1. Try opening once.  
  2. Go to **System Settings > Privacy & Security**.  
  3. Click **Open Anyway** for ACE Photo Studio.  
  4. Confirm the open prompt.  
  5. Optional: right-click app in Applications and choose **Open**.

- **Need a before/after check quickly?**  
  Use **Split View** or **Slider View** and move the slider to compare edits.

- **Need to export multiple photos?**  
  In Export Settings, choose **Current Selection** or **All Loaded Photos**.

- **HDR merge found no complete sets?**  
  Check bracket size mode, and review incomplete/skipped info in the HDR details panel.

## 8) Keep It Simple

- Start with **Auto Fix** or a built-in preset, then fine-tune sliders.
- Use **Fit** often while adjusting.
- For bracketed DNG shoots: **Add HDR Folder -> Start HDR Merge -> edit merged TIFFs -> Export**.
- If something looks off, use **Reset** and try a different preset/profile path.
