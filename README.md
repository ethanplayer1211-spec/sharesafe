ShareSafe

A browser-local screenshot editor. Open an image, cover private details with solid rectangles, add numbered pins, and export a flattened, full-resolution PNG.

![ShareSafe editor](preview.jpg)

Quickest way to try it

Download **ShareSafe.html** from this repository and open it in a current browser. This portable version contains the complete application and fictional example in one file, with no server, install, account or external assets required. Its inline code and styles are protected by explicit Content Security Policy hashes. It works offline after download.

The development source is also provided as **ShareSafe-source.zip**. Extract it to inspect the separate source files, tests and tools. To regenerate the single-file version from that source, run `npm run portable`.

The repository also includes **ShareSafe-demo.mp4**, a 48-second captioned walkthrough made from actual editor captures. All example details are fictional.

Run locally

Use a recent Node.js version (20 or later). There are no runtime dependencies and no install step. On Windows, double-click `Start ShareSafe.cmd`, then open the address it prints. Or run:

```sh
npm run dev
```

Open `http://127.0.0.1:4173/`. `dist/` is also ready for any static web host.

Using the editor

- Open, paste, or drop one PNG, JPG, or WebP. Maximum 25 MB, 24 megapixels, and 12,000 pixels per side.
- **Redact (R):** drag a rectangle. Ink, paper and coral are all opaque cover colors.
- **Number (N):** click to place a pin. Pins renumber when one is deleted.
- **Select (V):** click an edit to move it. Drag a cover's corner to resize it. Arrow keys nudge a selected edit by one pixel; Shift moves ten pixels.
- **Delete:** use the toolbar trash button, edit list or Delete/Backspace key.
- **Undo/redo:** Ctrl/Command+Z and Ctrl/Command+Shift+Z. Ctrl/Command+Y also redoes. The history retains up to 60 changes.
- **Compare:** hold the eye button, or hold Space/Enter while that button is focused. It temporarily displays the unedited source.
- **Zoom:** use +/− and Fit. Scroll the workspace when zoomed in.
- **Export:** review the image, check the review box, and download a PNG at the source dimensions.

All fictional example content and artwork were created for this project. The bundled example starts with two covers and one pin; Undo removes those sample edits.

Privacy and export behavior

Image bytes stay in browser memory. There is no image upload, server processing, analytics, external font, OCR, or AI API in the application. The document's Content Security Policy blocks application fetch/WebSocket connections (`connect-src 'none'`). Hosting the application still requires normal requests for its static files and may have a host-level access gate.

The editor retains the original in memory for undo and comparison. It creates a **separate canvas** for export: an opaque white background, decoded source image, opaque rectangular covers, then numbered pins. Cover edges round outward to whole pixels. The exported PNG has no editable layers, original image attachment, editor controls, or undo history. Re-encoding does not carry the original file's EXIF/text metadata into the new PNG.

No local storage, database, image history, or automatic saved copy is created. Closing/reloading the tab loses its editing session. A replacement/close warning protects unexported edits where the browser supports it. The download itself is saved by the browser at the user's request.

**Manual review is required.** The app does not detect private information or guarantee that the user has covered everything. Information remaining visible outside covers remains visible in the export. It cannot protect the unedited original, other files, clipboard copies, screen recordings, or screenshots made while comparing. Only share the exported PNG after reviewing it.

Checks

```sh
npm run check
npm test
node tools/verify-example-export.mjs "path/to/A little example-sharesafe.png"
```

The final command verifies an actual export of the untouched bundled example. It independently decodes PNG filters with Node's standard library, checks all 9,894 pixels in both sample cover regions for the exact opaque ink color, checks source dimensions, and rejects EXIF/text chunks. This is a fixture check, not an automatic privacy scan for arbitrary images.

Browser QA and its limits are recorded in `docs/VALIDATION.md`.

Implementation

Vanilla HTML/CSS/JavaScript, Canvas 2D, File/Clipboard events, Pointer Events, Blob PNG export, and Node's built-in test runner. `dist/core.js` owns geometry, history and the shared preview/export renderer; `dist/app.js` owns the editor workflow. No framework, build step, paid service or API key is required.

This is a new project, written with significant assistance from OpenAI Codex. No pre-existing application code, media, models or dependencies were reused.
