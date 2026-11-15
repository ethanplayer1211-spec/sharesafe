ShareSafe

A browser-local screenshot editor. Scan for private details, review the suggestions, use plain black or strong blur, and export a flattened PNG.

![ShareSafe editor](preview.jpg)

Try it

- Download **ShareSafe.html** and open it in a current desktop browser. In the source archive, **dist/index.html** is the same self-contained application.
- The roughly 9 MB HTML includes the editor, English OCR model, worker and examples. No account, API key or image upload is needed.
- Open, paste or drop a PNG, JPG or WebP. New images scan automatically. Maximum 25 MB, 24 megapixels and 12,000 pixels per side.
- Choose **DM example** for a fictional desktop conversation or **Try filter example** for personal fields.

Using the editor

- **Twelve privacy filters:** names, usernames, message text, birthdays, personal information, profile photos, pictures, email/phone, credentials, payment cards, work details and IP addresses.
- **Two OCR passes:** a contrast-normalized copy helps colored chat bubbles; a second pass reads the original colors. Overlapping matches are merged. The scan surface is capped at eight megapixels; export retains the original dimensions.
- **Chat / DMs:** layout checks find plain handles, message text and list previews. Auto mode also recognizes workspace chats. Set a conversation area when automatic bounds are wrong.
- **Work details:** review likely domains, repository paths, channels and organizations. These may be public; they start unchecked.
- **Extra words:** add names or phrases separated by commas/new lines. They remain included when other filters are off.
- **Black:** the default, fully opaque cover replaces the selected pixels with true black.
- **Blur:** a fixed strong blur is drawn into the exported PNG. It can retain visual clues; use Black for stronger concealment. Browsers without canvas-filter support use Black instead.
- **All covers:** switching Black/Blur changes applied covers together and updates the pending preview. Style changes support undo/redo.
- **Preview / Adjust:** inspect suggestions without applying them, move or resize their boxes, and reject mistakes. Inferred names, work details and generic picture estimates start unchecked.
- **Redact (R):** draw a cover over anything missed. **Select (V)** moves/resizes edits; arrows nudge one source pixel and Shift moves ten. **Number (N)** adds pins.
- **Undo/redo:** Ctrl/Command+Z and Ctrl/Command+Shift+Z; Ctrl/Command+Y also redoes. History retains up to 60 changes.
- **Compare:** hold the eye button to see the original. **Export:** review the full image, confirm the review checkbox, then download the PNG.

Five independently selected examples

Open **docs/review-five/index.html** for Original, Black, Blur and full-size views. All five examples were approved by the project owner on 16 November 2026.

- Random seed: 8508867. Five distinct app types were sampled, then one screenshot per type, before testing: Arc, Slack, GitHub, Terminal and Cursor.
- Source: Louis Beaumont's [ScreenLeak synthetic sample corpus](https://github.com/screenpipe/screenleak), commit 3670788846e99a64ae93128c0fa97f180f5825f6, [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). All details are fictional. Modified copies were censored with ShareSafe.
- The reviewed black exports passed pixel checks for 39 marked regions containing 82,326 visible private-detail pixels. Source annotation boxes were tightened to visible values; labels, separators and ellipses were excluded. This is a small workflow check, not a full ScreenLeak benchmark score.
- GitHub needed one incorrect name suggestion rejected and a manual rectangle over profile icons. Cursor needed a manual rectangle over its status-bar workspace path. Arc, Slack and Terminal needed no additional manual rectangles with all filters enabled. An avatar in the Slack example is also covered by a message suggestion.
- Both cover styles retain source dimensions, have opaque PNG pixels and omit source/undo attachments. Blur preserves pixels outside the corresponding black-mask areas.
- The separate fictional DM regression covers five username occurrences, eight message lines/previews and six avatars. Its black export also preserved 1,017,398 pixels outside the allowed cover areas.

Privacy and limits

Image bytes and recognized text stay in memory. The app uses no AI API, analytics, local storage or IndexedDB image cache. CSP blocks application network connections; the bundled worker and WebAssembly run locally. Closing or reloading loses the editing session. Hosting the HTML requires an ordinary request to the host.

English OCR, name recognition and layout/image heuristics can miss content or suggest incorrect regions. These are suggestions, not a safety verdict. Photos, cropped layouts, unfamiliar interfaces, non-English text, tiny text, emojis and unlabelled personal information may need manual covers. The user's own Instagram screenshot has not been supplied or tested. Always inspect the full-resolution result and the cover edges.

The original stays in memory for editing and comparison. Export paints the source and applied covers into a separate canvas, without editor overlays, original attachments, editable layers or undo history. Cover boundaries round outward to whole pixels. Source EXIF/text metadata is not carried into the new PNG. Details outside covers remain visible.

Build and checks

The modular source is in **ShareSafe-source.zip**. Use Node.js 20 or later:

```sh
npm ci --ignore-scripts
npm run build
npm run check
npm test
node tools/check-review-coverage.mjs
node tools/check-style-exports.mjs
npm run dev
```

The optional server runs at http://127.0.0.1:4173/; Windows users can use **Start ShareSafe.cmd**. The three HTML entry files are identical and self-contained. Thirty-one tests cover geometry/history, cover fallback, OCR matching/merging, chat layout, filters, offline assets and CSP hashes. Browser checks exercised import, preview cancellation, apply, undo/redo, style switching and download. Evidence and limits are in **docs/VALIDATION.md** and **docs/review-five/**.

Implementation and credits

Vanilla HTML/CSS/JavaScript, Canvas 2D, pinned Tesseract.js 7.0.0 and its existing English LSTM model, and Compromise 14.20.0. The models/components were not trained for this project. Application development and examples used significant assistance from OpenAI Codex. Licenses are in **THIRD_PARTY_NOTICES.txt** and the HTML.

The 60-second silent, captioned demo uses actual editor captures and exported PNGs. YouTube upload awaits the owner's separate video approval.
