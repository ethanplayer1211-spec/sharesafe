ShareSafe

A browser-local screenshot editor with automatic private-detail suggestions. Open an image, review the scan, apply solid covers, and export a flattened PNG.

![ShareSafe editor](preview.jpg)

Quickest way to try it

Download **ShareSafe.html** and open it in a current desktop browser with WebAssembly SIMD support (current Chrome, Edge, Firefox or Safari). In the extracted source, **dist/index.html** is the same complete application. The roughly 9 MB HTML contains the editor, example, English OCR model and worker. No account, server, API key or network download is needed to scan.

New images scan automatically. To test the bundled fictional example, click **Scan private details**. It starts with no pre-applied covers, so you can see the scan's suggestions and resulting changes. Choose **Try filter example** in the footer to scan a fictional profile with a name, username, birthday, address, student ID, avatar and picture.

Using the editor

- **Privacy filters:** ten independent categories: names, usernames, birthdays, personal info, profile photos, pictures, email/phone, credentials, payment cards and IP addresses. Change them before or after scanning; results update without another OCR pass. Applied covers stay in place.
- **Scan:** bundled English OCR and local name recognition find likely names, @handles, labeled usernames/birthdays, address and ID fields, contact details, payment cards, IPs and credentials.
- **Pictures / profile photos:** local color/texture analysis suggests image regions. Small avatar-like regions also use nearby name/username context. These are visual estimates, not face identification; inspect and resize their covers as needed.
- **Extra words:** add specific names, usernames or phrases, separated by commas/new lines. They stay included even when other categories are off.
- **Review:** numbered dashed boxes show the suggestions. Inspect the image, select individual suggestions, then choose **Cover selected**. Suggestions do not become covers until applied. Applying a batch is one undo step. Cancel stops the worker immediately.
- **Redact (R):** draw additional opaque covers over anything the scan missed. Choose ink, paper or coral.
- **Number (N):** add numbered pins for context.
- **Select (V):** move edits or resize covers from a corner. Arrow keys nudge by one pixel; Shift moves ten.
- **Undo/redo:** Ctrl/Command+Z and Ctrl/Command+Shift+Z. Ctrl/Command+Y also redoes. History retains up to 60 changes.
- **Compare:** hold the eye button to view the original. Zoom or Fit as needed.
- **Export:** check the full image, confirm the review box, and download a PNG at the original resolution. Uncovered scan suggestions produce a warning.

Open, paste or drop one PNG, JPG or WebP. Maximum 25 MB, 24 megapixels and 12,000 pixels per side. The scan surface is capped at six megapixels; export keeps the original dimensions.

Privacy and limitations

The neural OCR model runs in a dedicated browser worker. Compromise provides bundled English name recognition; structured pattern checks find personal fields and other private text. A local image-region heuristic estimates picture and avatar areas. This is OCR-assisted detection, not a cloud language model or a model we trained.

Image bytes and recognized text stay in browser memory. No image upload, AI API, analytics, local storage, IndexedDB cache or automatic saved copy is used. CSP blocks application network connections with `connect-src 'none'`; a bundled Blob worker and WebAssembly compilation are permitted. Closing or reloading loses the editing session. Hosting the HTML still requires an ordinary request to the host.

**Review the whole screenshot.** English OCR can misread or miss text, especially small, rotated, low-contrast or unusual text. Name recognition is English-focused; labeled names also support Unicode text. Birthdays need a birthday/DOB/born label. Addresses and other personal fields use patterns and labels, so contextless details may be missed. Picture/profile-photo regions can miss photos, confuse icons with photos, or cover only part of an image. QR codes and arbitrary sensitive messages are not reliably identified. A clean scan is not proof that an image is safe. Inspect the cover edges and add manual covers as needed.

The original remains in memory for editing and comparison. Export renders the source and applied opaque covers into a separate canvas. It omits dashed suggestions, editor controls, editable layers, original attachments and undo history. Cover edges round outward to whole pixels. Re-encoding does not carry source EXIF/text metadata into the new PNG. Information outside covers remains visible.

Development and checks

The full modular source is in **ShareSafe-source.zip**. The distributed HTML runs without installation. To rebuild after editing `src/editor.html` or JavaScript/CSS in `dist/`, use Node.js 20 or later:

```sh
npm ci --ignore-scripts
npm run build
npm run check
npm test
npm run dev
```

The optional server opens at `http://127.0.0.1:4173/`. Windows users can use `Start ShareSafe.cmd`. All three HTML entry files are identical and self-contained; code and styles are protected by CSP hashes.

Twenty tests cover editing geometry, history, opaque export rendering, structured-detail matching, OCR word-to-image coordinates, split OCR blocks, offline assets and CSP hashes. The earlier browser QA found both structured private details in the inbox and all six expected details in a separate fictional fixture. The v1.2 filter example adds names, a username, birthday, personal fields, an illustrated avatar and a textured picture. Selective application, undo/redo, cancel, automatic import scanning and phone-width review were exercised. This small fixture check is not a general accuracy benchmark.

The browser exports were independently decoded with `tools/verify-ocr-export.mjs`: 1280 × 800, 4,510 changed pixels in the two intended regions, all opaque ink; no dashed preview marks or text/EXIF metadata. `tools/verify-example-export.mjs` is retained as a historical check for the earlier manual example. More evidence and limits are in `docs/VALIDATION.md`.

Implementation and credits

Vanilla HTML/CSS/JavaScript, Canvas 2D, a pinned Tesseract.js 7.0.0 worker/core with the existing English LSTM model, and Compromise 14.20.0 for local name recognition. The offline builder embeds dependencies; no paid service is needed. Third-party licenses are in **THIRD_PARTY_NOTICES.txt** and embedded in each HTML file.

ShareSafe's application code and fictional example were created for this project with significant assistance from OpenAI Codex. The OCR engine, model and their dependencies are existing open-source components, not original model training.

The repository includes a captioned demo made from actual app captures. It has not been uploaded to YouTube; that upload awaits the owner's review and approval.