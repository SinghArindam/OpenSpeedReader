## 2025-12-13 18:41 (Detailed Session Changes)

### Documentation & Project Structure
- **Why**: The project lacked a proper `README.md`, and the changelog files were fragmented (some in root, some in `development/`).
- **Implementation**:
    - Rewrote `README.md` to include a full feature list, usage guide, and shortcuts table.
    - Updated links in `README.md` to reference `development/changelog.md` and `development/detailed_changelog.md`.
    - Deleted `CHANGELOG.md` and `CHANGELOG_DETAILED.md` from the root directory to avoid duplication.
- **Code Snippets**:
  ```markdown
  # OpenSpeedReader
  ...
  - [Changelog](./development/changelog.md)
  ```

## 2025-12-13 18:23 (Detailed Session Changes)

### Mobile Viewport & Safe Area Fixes
- **Why**: User reported the UI was getting cut off at the bottom on mobile devices. Standard `100vh` does not account for dynamic browser elements like address bars on mobile.
- **Implementation**:
    - Updated `body` height from `h-screen` to `h-[100dvh]` to respect the dynamic viewport.
    - Added `.pb-safe` utility using `env(safe-area-inset-bottom)` and applied it to the footer to prevent content from being hidden behind home gestures.
- **Code Snippets**:
  ```html
  <body class="... h-[100dvh] ...">
  ```
  ```css
  .pb-safe {
      padding-bottom: env(safe-area-inset-bottom);
  }
  ```

### CSS Standards Compliance
- **Why**: IDE flagged a warning for non-standard CSS property usage in range sliders.
- **Implementation**: Added the standard `appearance: none` property alongside the vendor-prefixed version.
- **Code Snippets**:
  ```css
  input[type=range] {
      -webkit-appearance: none;
      appearance: none; /* Added standard property */
      background: transparent;
  }
  ```

## 2025-12-13 17:27 (Detailed Session Changes)

### Responsive Context Words
- **Why**: User requested context words to be visible (1 before/after on mobile, 2 before/after on desktop) for better reading flow.
- **Implementation**: Updated `#word-stage` HTML structure and `RSVPEngine.render()` logic in `SpeedReaderProV1.html`.
- **Code**:
```javascript
// Render Context Words
const getW = (off) => this.words[this.index + off] || "";
this.ui.ctxL2.innerText = getW(-2); // Desktop only
this.ui.ctxL1.innerText = getW(-1);
this.ui.ctxR1.innerText = getW(1);
this.ui.ctxR2.innerText = getW(2); // Desktop only
```

### Fixed Center Alignment (Anti-Shift)
- **Why**: The red focus character was shifting left/right depending on word length, disturbing the reading focus point.
- **Implementation**: Refactored `#word-stage` from Flexbox to Absolute positioning. The red focus character is now pinned to `left: 50%` with `transform: translateX(-50%)`.
- **Snippet**:
```html
<div class="absolute inset-0 flex items-center justify-center pointer-events-none">
    <span id="text-focus" class="absolute left-1/2 -translate-x-1/2 ..."></span>
</div>
```

### Improved Speed Menu Interaction
- **Why**: The hover-based menu was closing unintentionally on PC when the cursor moved.
- **Implementation**: Changed to `onclick="uiMgr.toggleSpeed()"`. Added logic to close the menu when clicking outside.

### Font Size Control
- **Why**: User wanted ability to adjust text size.
- **Implementation**: Added a font size button and slider. Used CSS `transform: scale()` on the `#word-stage` container for smooth, artifact-free resizing.
- **Code**:
```javascript
setFontScale(scale) {
    document.getElementById('word-stage').style.transform = `scale(${scale})`;
}
```

### Auto-Load Sample File
- **Why**: To provide an immediate "try it out" experience without needing to upload a file manually.
- **Implementation**: Refactored file parsing into `handleFile()`. Added an IIFE that fetches `sample/sample.epub` and passes it to `handleFile` if found.
```javascript
// Auto-load sample
const samples = ['sample/sample.epub', 'sample/sample.pdf'];
for (const path of samples) {
    // try fetch... handleFile(file);
}
```
